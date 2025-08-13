# ROS2성능최적화

> 상위: [[ROS2]]

Profiling, memory, CPU 최적화를 통한 ROS2 시스템의 성능 향상 기법을 다룹니다.

---

## 📊 성능 분석 도구

### 프로파일링 도구
- **ros2_tracing**: ROS2 전용 시스템 추적
- **perf**: Linux 성능 분석 도구
- **htop/top**: 실시간 시스템 모니터링
- **iotop**: I/O 성능 분석

### 기본 분석 명령어
```bash
# CPU 사용률 모니터링
htop

# 메모리 사용량 확인
free -h

# 디스크 I/O 확인
iotop

# 네트워크 사용량
iftop
```

---

## 🧠 메모리 최적화

### 메모리 사전 할당
```cpp
// 메시지 풀 사용
class OptimizedPublisher : public rclcpp::Node {
private:
  std::unique_ptr<std_msgs::msg::String> message_pool_;
  
public:
  OptimizedPublisher() : Node("optimized_publisher") {
    // 메시지 사전 할당
    message_pool_ = std::make_unique<std_msgs::msg::String>();
    publisher_ = create_publisher<std_msgs::msg::String>("topic", 10);
  }
  
  void publish_message() {
    message_pool_->data = "Hello World";
    publisher_->publish(*message_pool_);
  }
};
```

### 메모리 누수 방지
```cpp
// 스마트 포인터 사용
auto subscription = create_subscription<std_msgs::msg::String>(
  "topic", 10, 
  [this](std_msgs::msg::String::SharedPtr msg) {
    // 자동 메모리 관리
    process_message(msg);
  });

// RAII 패턴 준수
class ResourceManager {
  ResourceManager() { /* 리소스 할당 */ }
  ~ResourceManager() { /* 리소스 해제 */ }
};
```

---

## ⚡ CPU 최적화

### 콜백 최적화
```cpp
// 빠른 콜백 처리
void fast_callback(const sensor_msgs::msg::Image::SharedPtr msg) {
  // 최소한의 처리만 수행
  if (msg->data.empty()) return;
  
  // 무거운 작업은 별도 스레드로
  std::async(std::launch::async, [this, msg]() {
    heavy_processing(msg);
  });
}
```

### 스레딩 최적화
```cpp
// MultiThreaded Executor 활용
rclcpp::executors::MultiThreadedExecutor executor(
  rclcpp::ExecutorOptions(), 
  std::thread::hardware_concurrency() // CPU 코어 수만큼
);

// Callback Group 분리
auto fast_group = create_callback_group(rclcpp::CallbackGroupType::Reentrant);
auto slow_group = create_callback_group(rclcpp::CallbackGroupType::MutuallyExclusive);
```

---

## 🔄 QoS 최적화

### 네트워크 최적화 QoS
```cpp
// 고성능 센서 데이터
auto qos_sensor = rclcpp::QoS(1)
  .reliability(RMW_QOS_POLICY_RELIABILITY_BEST_EFFORT)
  .history(RMW_QOS_POLICY_HISTORY_KEEP_LAST)
  .durability(RMW_QOS_POLICY_DURABILITY_VOLATILE);

// 중요한 제어 명령
auto qos_control = rclcpp::QoS(10)
  .reliability(RMW_QOS_POLICY_RELIABILITY_RELIABLE)
  .deadline(std::chrono::milliseconds(100));
```

### 대역폭 관리
```cpp
// 메시지 압축 (지원되는 경우)
auto publisher = create_publisher<sensor_msgs::msg::CompressedImage>(
  "compressed_image", qos_sensor);

// 적응적 품질 조절
if (network_load > threshold) {
  // 낮은 해상도로 전환
  publish_low_resolution_image();
} else {
  // 고해상도 유지
  publish_high_resolution_image();
}
```

---

## 🚀 실시간 최적화

### 실시간 설정
```cpp
// 실시간 우선순위 설정
#include <pthread.h>

void set_realtime_priority() {
  struct sched_param param;
  param.sched_priority = 90;
  
  if (pthread_setschedparam(pthread_self(), SCHED_FIFO, &param) != 0) {
    RCLCPP_ERROR(get_logger(), "Failed to set real-time priority");
  }
}
```

### CPU 친화성 설정
```bash
# 특정 CPU 코어에 바인딩
taskset -c 0-3 ros2 run my_package my_node

# 환경 변수로 설정
export ROS_PYTHON_THREAD_AFFINITY="0,1,2,3"
```

---

## 📈 성능 모니터링

### 지연시간 측정
```cpp
class LatencyMonitor : public rclcpp::Node {
private:
  rclcpp::Time last_received_;
  
public:
  void callback(const std_msgs::msg::Header::SharedPtr msg) {
    auto now = this->now();
    auto latency = now - rclcpp::Time(msg->stamp);
    
    RCLCPP_INFO(get_logger(), "Latency: %f ms", 
                latency.seconds() * 1000.0);
  }
};
```

### 성능 로깅
```python
# Python 성능 측정
import time
import psutil

class PerformanceMonitor:
    def __init__(self):
        self.start_time = time.time()
        self.process = psutil.Process()
    
    def log_performance(self):
        cpu_percent = self.process.cpu_percent()
        memory_mb = self.process.memory_info().rss / 1024 / 1024
        
        self.get_logger().info(f'CPU: {cpu_percent}%, Memory: {memory_mb:.1f}MB')
```

---

## 🛠️ 최적화 체크리스트

### 메모리 최적화
- ✅ 메시지 사전 할당
- ✅ 스마트 포인터 사용
- ✅ 메모리 풀 활용
- ✅ 불필요한 복사 제거

### CPU 최적화
- ✅ 콜백 실행 시간 최소화
- ✅ 멀티스레딩 적절히 활용
- ✅ CPU 친화성 설정
- ✅ 컴파일러 최적화 활용

### 네트워크 최적화
- ✅ 적절한 QoS 정책
- ✅ 메시지 크기 최소화
- ✅ 불필요한 토픽 구독 제거
- ✅ 로컬 통신 우선

---

## 🔧 진단 명령어

### 시스템 성능 확인
```bash
# ROS2 시스템 진단
ros2 doctor

# 노드 성능 확인
ros2 node info /my_node

# 토픽 대역폭 측정
ros2 topic bw /my_topic

# 토픽 주파수 측정
ros2 topic hz /my_topic
```

---

## 📖 주요 참고문헌

1. **ROS2 Performance**: https://docs.ros.org/en/humble/Concepts/About-Quality-of-Service-Settings.html
2. **Real-time Programming**: ROS2 실시간 프로그래밍 가이드
3. **Memory Management**: C++ 메모리 관리 최적화
4. **Profiling Tools**: Linux 시스템 프로파일링 도구