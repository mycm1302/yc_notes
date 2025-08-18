# ROS2실시간시스템

> 상위: [[ROS2]]

RT 커널과 결정적 실행을 통한 ROS2 기반 실시간 로봇 제어 시스템 구축을 다룹니다.

---

## ⏱️ 실시간 시스템 개요

### 실시간성의 중요성
- **시간 제약**: 정해진 시간 내 응답 보장
- **결정적 실행**: 예측 가능한 동작 패턴
- **낮은 지연시간**: 최소한의 처리 지연
- **지터 최소화**: 일정한 응답 시간

### 실시간 분류
- **Hard Real-time**: 데드라인 위반 시 시스템 실패
- **Soft Real-time**: 데드라인 위반 허용, 성능 저하
- **Firm Real-time**: 가끔씩 데드라인 위반 허용

---

## 🔧 RT_PREEMPT 커널

### RT_PREEMPT 설치
```bash
# 의존성 설치
sudo apt-get update
sudo apt-get install build-dep linux
sudo apt-get install libncurses-dev flex bison openssl libssl-dev \
    dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf fakeroot

# 커널 소스 다운로드
mkdir ~/kernel && cd ~/kernel
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v6.x/linux-6.1.78.tar.gz
wget https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/6.1/patch-6.1.78-rt24.patch.gz

# 압축 해제 및 패치 적용
tar -xzf linux-6.1.78.tar.gz
cd linux-6.1.78
gunzip -c ../patch-6.1.78-rt24.patch.gz | patch -p1

# 기본 설정 복사
cp /boot/config-$(uname -r) .config
make olddefconfig
```

### 커널 설정
```bash
# 메뉴 설정 실행
make menuconfig

# 필수 설정 옵션들:
# General Setup -> Preemption Model
#   (X) Fully Preemptible Kernel (Real-Time)
# General setup -> Timers subsystem
#   [*] High Resolution Timer Support
#   Timer tick handling (Full dynticks system (tickless))
# Processor type and features -> Timer frequency
#   (X) 1000 HZ
# Power management and ACPI options -> CPU Frequency scaling
#   Default CPUFreq governor (performance)
```

### 커널 빌드 및 설치
```bash
# 빌드 (시간 소요)
make -j$(nproc) deb-pkg

# 패키지 설치
cd ..
sudo dpkg -i linux-*.deb

# 재부팅 후 확인
sudo reboot
uname -a  # RT 커널 확인
cat /sys/kernel/realtime  # 1이 출력되어야 함
```

---

## ⚙️ 시스템 설정

### CPU 설정
```bash
# CPU 주파수 고정 (성능 모드)
sudo cpupower frequency-set -g performance

# CPU 격리 (실시간 작업용)
# /etc/default/grub 편집
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash isolcpus=2,3 nohz_full=2,3 rcu_nocbs=2,3"
sudo update-grub

# IRQ 친화성 설정 (CPU 0,1만 사용)
echo 3 | sudo tee /proc/irq/*/smp_affinity
```

### 메모리 설정
```bash
# Swap 비활성화
sudo swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

# 메모리 잠금 한계 설정
# /etc/security/limits.conf 편집
echo "* soft memlock unlimited" | sudo tee -a /etc/security/limits.conf
echo "* hard memlock unlimited" | sudo tee -a /etc/security/limits.conf

# Transparent Huge Pages 비활성화
echo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled
```

### 실시간 우선순위 설정
```bash
# /etc/security/limits.conf에 추가
echo "* soft rtprio 99" | sudo tee -a /etc/security/limits.conf
echo "* hard rtprio 99" | sudo tee -a /etc/security/limits.conf

# PAM 모듈 활성화 확인
# /etc/pam.d/common-session에서 확인
grep "pam_limits.so" /etc/pam.d/common-session
```

---

## 🎯 실시간 ROS2 프로그래밍

### 실시간 노드 예제
```cpp
#include <rclcpp/rclcpp.hpp>
#include <std_msgs/msg/string.hpp>
#include <pthread.h>
#include <sched.h>
#include <sys/mlock.h>
#include <sys/resource.h>

class RealtimeNode : public rclcpp::Node
{
public:
  RealtimeNode() : Node("realtime_node")
  {
    // 메모리 잠금 (페이지 폴트 방지)
    if (mlockall(MCL_CURRENT | MCL_FUTURE) != 0) {
      RCLCPP_ERROR(this->get_logger(), "Failed to lock memory");
    }
    
    // 실시간 스케줄링 설정
    struct sched_param param;
    param.sched_priority = 80;  // 1-99 범위, 99가 최고 우선순위
    
    if (sched_setscheduler(0, SCHED_FIFO, &param) != 0) {
      RCLCPP_ERROR(this->get_logger(), "Failed to set real-time scheduling");
    }
    
    // 퍼블리셔 생성 (사전 할당)
    publisher_ = this->create_publisher<std_msgs::msg::String>("rt_topic", 10);
    
    // 메시지 사전 할당
    message_ = std::make_shared<std_msgs::msg::String>();
    
    // 실시간 타이머 (1ms 주기)
    timer_ = this->create_wall_timer(
      std::chrono::microseconds(1000),
      std::bind(&RealtimeNode::timer_callback, this));
    
    RCLCPP_INFO(this->get_logger(), "Real-time node initialized");
  }

private:
  void timer_callback()
  {
    // 실시간 제약이 있는 작업
    auto start = std::chrono::high_resolution_clock::now();
    
    // 메시지 업데이트 (동적 할당 피함)
    message_->data = "RT message: " + std::to_string(counter_++);
    
    // 발행
    publisher_->publish(*message_);
    
    // 실행 시간 측정
    auto end = std::chrono::high_resolution_clock::now();
    auto duration = std::chrono::duration_cast<std::chrono::microseconds>(
      end - start).count();
    
    // 지터 모니터링 (로깅은 실시간 경로에서 피해야 함)
    if (duration > 500) {  // 500us 초과 시 경고
      violation_count_++;
    }
  }
  
  rclcpp::Publisher<std_msgs::msg::String>::SharedPtr publisher_;
  rclcpp::TimerBase::SharedPtr timer_;
  std::shared_ptr<std_msgs::msg::String> message_;
  size_t counter_ = 0;
  size_t violation_count_ = 0;
};
```

### 실시간 Executor
```cpp
#include <rclcpp/executors/static_single_threaded_executor.hpp>

class RealtimeExecutor
{
public:
  RealtimeExecutor()
  {
    // 실시간 스케줄링 설정
    setup_realtime_scheduling();
    
    // 메모리 사전 할당
    setup_memory_management();
    
    // 정적 Executor 사용 (동적 할당 최소화)
    executor_ = std::make_shared<rclcpp::executors::StaticSingleThreadedExecutor>();
  }
  
  void add_node(rclcpp::Node::SharedPtr node)
  {
    executor_->add_node(node);
  }
  
  void spin()
  {
    // 실시간 루프
    while (rclcpp::ok()) {
      auto start = std::chrono::steady_clock::now();
      
      // Executor 스핀 (논블로킹)
      executor_->spin_some(std::chrono::nanoseconds(0));
      
      // 다음 사이클까지 대기 (정확한 타이밍 유지)
      auto end = start + std::chrono::microseconds(1000);  // 1ms 주기
      std::this_thread::sleep_until(end);
    }
  }

private:
  void setup_realtime_scheduling()
  {
    struct sched_param param;
    param.sched_priority = 90;
    
    if (sched_setscheduler(0, SCHED_FIFO, &param) != 0) {
      throw std::runtime_error("Failed to set real-time scheduling");
    }
  }
  
  void setup_memory_management()
  {
    // 메모리 잠금
    if (mlockall(MCL_CURRENT | MCL_FUTURE) != 0) {
      throw std::runtime_error("Failed to lock memory");
    }
    
    // 스택 크기 설정
    struct rlimit rlim;
    rlim.rlim_cur = 8 * 1024 * 1024;  // 8MB
    rlim.rlim_max = 8 * 1024 * 1024;
    setrlimit(RLIMIT_STACK, &rlim);
  }
  
  std::shared_ptr<rclcpp::executors::StaticSingleThreadedExecutor> executor_;
};
```

---

## 📊 실시간 성능 측정

### cyclictest를 이용한 지연시간 측정
```bash
# cyclictest 설치
sudo apt-get install rt-tests

# 기본 지연시간 테스트 (10분간)
sudo cyclictest -t1 -p 80 -n -i 1000 -l 600000

# 다중 CPU 테스트
sudo cyclictest -t4 -p 80 -n -i 1000 -l 600000 -a 0,1,2,3

# 히스토그램 출력
sudo cyclictest -t1 -p 80 -n -i 1000 -l 600000 -h 100 -q

# 결과 해석:
# - Min: 최소 지연시간 (일반적으로 < 10μs)
# - Avg: 평균 지연시간
# - Max: 최대 지연시간 (Hard RT에서는 < 100μs 목표)
```

### ROS2 성능 측정 도구
```cpp
// rttest를 이용한 ROS2 성능 측정
#include <rttest/rttest.h>

class PerformanceMeasurement : public rclcpp::Node
{
public:
  PerformanceMeasurement() : Node("perf_measurement")
  {
    // rttest 초기화
    rttest_set_sched_priority(80, SCHED_FIFO);
    rttest_lock_and_prefault_dynamic();
    
    // 측정 시작
    rttest_init_timer_thread();
    rttest_spin_wait_us(1000);  // 1ms 대기
    
    timer_ = this->create_wall_timer(
      std::chrono::microseconds(1000),
      std::bind(&PerformanceMeasurement::measure_callback, this));
  }
  
  ~PerformanceMeasurement()
  {
    // 통계 출력
    rttest_write_results();
    rttest_finish();
  }

private:
  void measure_callback()
  {
    rttest_spin_wait_us(100);  // 100μs 작업 시뮬레이션
  }
  
  rclcpp::TimerBase::SharedPtr timer_;
};
```

---

## 🔧 실시간 최적화 기법

### 메모리 관리
```cpp
class RealtimeMemoryManager
{
public:
  // 사전 할당된 메모리 풀
  template<typename T>
  class ObjectPool
  {
  public:
    ObjectPool(size_t pool_size) : pool_size_(pool_size)
    {
      // 메모리 사전 할당
      objects_.reserve(pool_size);
      available_.reserve(pool_size);
      
      for (size_t i = 0; i < pool_size; ++i) {
        objects_.emplace_back(std::make_unique<T>());
        available_.push_back(objects_.back().get());
      }
    }
    
    T* acquire()
    {
      if (available_.empty()) {
        return nullptr;  // 풀이 고갈됨
      }
      
      T* obj = available_.back();
      available_.pop_back();
      return obj;
    }
    
    void release(T* obj)
    {
      available_.push_back(obj);
    }

  private:
    size_t pool_size_;
    std::vector<std::unique_ptr<T>> objects_;
    std::vector<T*> available_;
  };
  
  // 실시간 안전한 큐
  template<typename T>
  class LockFreeQueue
  {
  public:
    LockFreeQueue(size_t capacity) 
      : buffer_(capacity), head_(0), tail_(0), capacity_(capacity) {}
    
    bool push(const T& item)
    {
      size_t next_tail = (tail_ + 1) % capacity_;
      if (next_tail == head_.load()) {
        return false;  // 큐가 가득참
      }
      
      buffer_[tail_] = item;
      tail_.store(next_tail);
      return true;
    }
    
    bool pop(T& item)
    {
      size_t current_head = head_.load();
      if (current_head == tail_.load()) {
        return false;  // 큐가 비어있음
      }
      
      item = buffer_[current_head];
      head_.store((current_head + 1) % capacity_);
      return true;
    }

  private:
    std::vector<T> buffer_;
    std::atomic<size_t> head_;
    std::atomic<size_t> tail_;
    size_t capacity_;
  };
};
```

### 실시간 동기화
```cpp
#include <atomic>
#include <memory>

// 실시간 안전한 데이터 공유
template<typename T>
class RealtimeBuffer
{
public:
  RealtimeBuffer()
  {
    // 두 개의 버퍼로 더블 버퍼링
    buffer_[0] = std::make_shared<T>();
    buffer_[1] = std::make_shared<T>();
    read_idx_.store(0);
    write_idx_.store(1);
  }
  
  // 실시간 안전한 읽기
  std::shared_ptr<T> readFromRT()
  {
    return buffer_[read_idx_.load()];
  }
  
  // 비실시간에서 쓰기
  void writeFromNonRT(const T& data)
  {
    int write_index = write_idx_.load();
    *buffer_[write_index] = data;
    
    // 버퍼 교체 (원자적 연산)
    int read_index = read_idx_.load();
    read_idx_.store(write_index);
    write_idx_.store(read_index);
  }

private:
  std::array<std::shared_ptr<T>, 2> buffer_;
  std::atomic<int> read_idx_;
  std::atomic<int> write_idx_;
};
```

---

## 🛠️ DDS 실시간 설정

### CycloneDDS 설정
```xml
<!-- cyclonedx_config.xml -->
<CycloneDX>
  <Domain>
    <General>
      <DomainId>0</DomainId>
      <ProcessPriority>high</ProcessPriority>
    </General>
    
    <Threads>
      <!-- 실시간 스레드 설정 -->
      <Thread Name="recv">
        <StackSize>1MB</StackSize>
        <Priority>80</Priority>
        <Policy>SCHED_FIFO</Policy>
        <Affinity>1</Affinity>
      </Thread>
      
      <Thread Name="send">
        <StackSize>1MB</StackSize>
        <Priority>70</Priority>
        <Policy>SCHED_FIFO</Policy>
        <Affinity>2</Affinity>
      </Thread>
    </Threads>
    
    <Reliability>
      <MaxRetransmitDelay>100ms</MaxRetransmitDelay>
      <HeartbeatPeriod>10ms</HeartbeatPeriod>
    </Reliability>
  </Domain>
</CycloneDX>
```

### 환경 변수 설정
```bash
# CycloneDDS 설정 파일 지정
export CYCLONEDX_URI="file:///path/to/cyclonedx_config.xml"

# RMW 구현체 설정
export RMW_IMPLEMENTATION=rmw_cyclonedx_cpp

# QoS 설정
export ROS_AUTOMATIC_DISCOVERY_RANGE=LOCALHOST
```

---

## 📈 모니터링 및 디버깅

### 실시간 모니터링
```cpp
#include <signal.h>
#include <sys/time.h>

class RealtimeMonitor
{
public:
  RealtimeMonitor() 
  {
    // 신호 핸들러 설정 (데드라인 미스 감지)
    signal(SIGALRM, deadline_miss_handler);
    
    // 통계 초기화
    reset_statistics();
  }
  
  void start_cycle()
  {
    cycle_start_ = std::chrono::high_resolution_clock::now();
    
    // 알람 설정 (데드라인)
    struct itimerval timer;
    timer.it_value.tv_sec = 0;
    timer.it_value.tv_usec = 1000;  // 1ms 데드라인
    timer.it_interval.tv_sec = 0;
    timer.it_interval.tv_usec = 0;
    setitimer(ITIMER_REAL, &timer, nullptr);
  }
  
  void end_cycle()
  {
    auto end = std::chrono::high_resolution_clock::now();
    auto duration = std::chrono::duration_cast<std::chrono::microseconds>(
      end - cycle_start_).count();
    
    // 통계 업데이트
    update_statistics(duration);
    
    // 알람 취소
    alarm(0);
  }
  
  void print_statistics()
  {
    std::cout << "Cycles: " << cycle_count_ << std::endl;
    std::cout << "Min latency: " << min_latency_ << " μs" << std::endl;
    std::cout << "Max latency: " << max_latency_ << " μs" << std::endl;
    std::cout << "Avg latency: " << avg_latency_ << " μs" << std::endl;
    std::cout << "Deadline misses: " << deadline_misses_ << std::endl;
  }

private:
  static void deadline_miss_handler(int sig)
  {
    deadline_misses_++;
  }
  
  void update_statistics(long duration)
  {
    cycle_count_++;
    min_latency_ = std::min(min_latency_, duration);
    max_latency_ = std::max(max_latency_, duration);
    avg_latency_ = (avg_latency_ * (cycle_count_ - 1) + duration) / cycle_count_;
  }
  
  void reset_statistics()
  {
    cycle_count_ = 0;
    min_latency_ = LONG_MAX;
    max_latency_ = 0;
    avg_latency_ = 0;
    deadline_misses_ = 0;
  }
  
  std::chrono::high_resolution_clock::time_point cycle_start_;
  static std::atomic<size_t> deadline_misses_;
  size_t cycle_count_;
  long min_latency_;
  long max_latency_;
  long avg_latency_;
};

std::atomic<size_t> RealtimeMonitor::deadline_misses_{0};
```

---

## 📖 주요 참고문헌

1. **ROS2 Real-time Design**: https://design.ros2.org/articles/realtime_background.html
2. **RT_PREEMPT Kernel Guide**: https://docs.ros.org/en/rolling/Tutorials/Miscellaneous/Building-Realtime-rt_preempt-kernel-for-ROS-2.html
3. **Real-time Performance Evaluation**: https://cjme.springeropen.com/articles/10.1186/s10033-023-00976-5
4. **ROS Real-time Working Group**: https://ros-realtime.github.io/
5. **Linux Real-time Kernel Builder**: https://github.com/ros-realtime/linux-real-time-kernel-builder