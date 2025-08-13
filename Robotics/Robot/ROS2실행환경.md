# ROS2실행환경

> 상위: [[ROS2]]

Executors, Threading, Scheduling을 통한 효율적인 실행 관리를 다룹니다.

---

## ⚙️ Executor 시스템

### Executor 타입
- **SingleThreadedExecutor**: 단일 스레드 순차 실행
- **MultiThreadedExecutor**: 멀티 스레드 병렬 실행
- **StaticSingleThreadedExecutor**: 정적 메모리 할당
- **EventsExecutor**: 이벤트 기반 실행 (실험적)

### 실행 모델
- **Pull Model**: Executor가 콜백 요청
- **Push Model**: 이벤트가 콜백 트리거
- **Callback Groups**: 콜백 그룹화 및 스케줄링
- **Wait Sets**: 대기 가능한 엔티티 집합

---

## 🧵 Threading 모델

### Callback Groups
- **Reentrant**: 동시 실행 가능한 콜백
- **Mutually Exclusive**: 상호 배타적 콜백
- **Custom**: 사용자 정의 동시성 제어

### Thread Safety
- **Node Thread Safety**: 노드 수준 스레드 안전성
- **Publisher/Subscriber**: 발행/구독 스레드 안전성
- **Service/Client**: 서비스 클라이언트 동시성
- **Timer**: 타이머 콜백 스레드 관리

### 예제 코드
```cpp
// Callback Group 생성
auto callback_group = node->create_callback_group(
    rclcpp::CallbackGroupType::Reentrant);

// Subscription with callback group
auto subscription = node->create_subscription<std_msgs::msg::String>(
    "topic", 10, callback, 
    rclcpp::SubscriptionOptions(), callback_group);

// MultiThreaded Executor
rclcpp::executors::MultiThreadedExecutor executor(
    rclcpp::ExecutorOptions(), 4); // 4 threads
```

---

## 📅 스케줄링

### 스케줄링 정책
- **FIFO**: 선입선출 방식
- **Priority**: 우선순위 기반
- **Round Robin**: 순환 방식
- **Real-time**: 실시간 스케줄링

### 우선순위 관리
- **Timer Priority**: 타이머 우선순위 설정
- **Subscription Priority**: 구독 우선순위
- **Service Priority**: 서비스 우선순위
- **Deadline Scheduling**: 마감시간 기반

---

## ⏱️ 타이머 시스템

### 타이머 타입
- **Wall Timer**: 실제 시간 기반
- **ROS Timer**: ROS 시간 기반 (시뮬레이션 고려)
- **Generic Timer**: 범용 타이머

### 시간 관리
- **Clock**: 시간 소스 추상화
- **Time Source**: 시간 제공자
- **Duration**: 시간 간격 표현
- **Rate**: 주기적 실행 제어

### 예제
```cpp
// Wall Timer
auto timer = node->create_wall_timer(
    std::chrono::milliseconds(100), timer_callback);

// ROS Timer with custom clock
auto clock = node->get_clock();
auto timer = node->create_timer(
    clock, std::chrono::milliseconds(100), timer_callback);
```

---

## 🎯 실시간 실행

### 실시간 특성
- **Deterministic Execution**: 결정적 실행 시간
- **Bounded Latency**: 제한된 지연시간
- **Priority Inheritance**: 우선순위 상속
- **Memory Pre-allocation**: 사전 메모리 할당

### RT 최적화
- **Lock-free Programming**: 무잠금 프로그래밍
- **Wait-free Algorithms**: 대기 없는 알고리즘
- **Memory Pools**: 메모리 풀 활용
- **CPU Affinity**: CPU 친화성 설정

---

## 📊 성능 모니터링

### 실행 통계
- **Callback Execution Time**: 콜백 실행 시간
- **Message Latency**: 메시지 지연시간
- **CPU Usage**: CPU 사용률
- **Memory Usage**: 메모리 사용량

### 프로파일링 도구
- **ros2_tracing**: 시스템 수준 추적
- **perf**: Linux 성능 분석
- **valgrind**: 메모리 프로파일링
- **gprof**: 함수 호출 분석

---

## 🔧 설정 및 튜닝

### Executor 설정
```cpp
// Executor 옵션 설정
rclcpp::ExecutorOptions options;
options.memory_strategy = 
    rclcpp::memory_strategies::create_default_strategy();

// Context 설정
rclcpp::ContextOptions context_options;
context_options.use_global_arguments = false;
```

### 환경 변수
- **RCL_EXECUTOR**: Executor 타입 선택
- **RMW_IMPLEMENTATION**: RMW 구현체 선택
- **ROS_DOMAIN_ID**: 도메인 ID 설정
- **RCUTILS_LOGGING_SEVERITY**: 로그 레벨 설정

---

## 📖 주요 참고문헌

1. **ROS2 Executors**: https://docs.ros.org/en/humble/Concepts/About-Executors.html
2. **Real-time Programming Guide**: ROS2 실시간 프로그래밍 가이드
3. **Threading Best Practices**: 멀티스레딩 모범 사례
4. **Performance Tuning**: ROS2 성능 최적화 가이드