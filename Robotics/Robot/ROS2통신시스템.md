# ROS2통신시스템

> 상위: [[ROS2]]

Topics, Services, Actions 통신 방식과 QoS 정책을 통한 품질 관리를 다룹니다.

---

## 📡 통신 패러다임

### 3가지 통신 방식
- **Topics**: 비동기 발행-구독 통신
- **Services**: 동기 요청-응답 통신  
- **Actions**: 장기간 비동기 작업 통신

### 통신 특성
- **느슨한 결합**: 송신자와 수신자 독립
- **동적 발견**: 런타임 연결 설정
- **타입 안전성**: 강한 타입 시스템
- **네트워크 투명성**: 로컬/원격 구분 없음

---

## 📰 Topics 통신

### 발행-구독 모델
- **Publisher**: 메시지 발행자
- **Subscriber**: 메시지 구독자
- **Topic**: 메시지 채널 이름
- **Message**: 데이터 구조

### 특징
- **1:N 통신**: 하나의 Publisher, 여러 Subscriber
- **비동기**: 논블로킹 통신
- **버퍼링**: 메시지 큐 관리
- **멀티캐스트**: 효율적 네트워크 활용

### 예제
```cpp
// Publisher
auto publisher = node->create_publisher<std_msgs::msg::String>("topic", 10);

// Subscriber  
auto subscription = node->create_subscription<std_msgs::msg::String>(
  "topic", 10, callback);
```

---

## 🔄 Services 통신

### 요청-응답 모델
- **Service Server**: 서비스 제공자
- **Service Client**: 서비스 요청자
- **Request**: 요청 메시지
- **Response**: 응답 메시지

### 특징
- **1:1 통신**: 요청-응답 쌍
- **동기/비동기**: 블로킹/논블로킹 지원
- **타임아웃**: 응답 대기 시간 제한
- **오류 처리**: 예외 및 오류 상태 관리

### 예제
```cpp
// Service Server
auto service = node->create_service<example_interfaces::srv::AddTwoInts>(
  "add_two_ints", handle_service);

// Service Client
auto client = node->create_client<example_interfaces::srv::AddTwoInts>(
  "add_two_ints");
```

---

## ⚡ Actions 통신

### 목표-피드백-결과 모델
- **Action Server**: 작업 수행자
- **Action Client**: 작업 요청자
- **Goal**: 목표 메시지
- **Feedback**: 진행 상황 피드백
- **Result**: 최종 결과

### 특징
- **장기간 작업**: 수초~수분 소요 작업
- **취소 가능**: 실행 중 취소 요청
- **진행 모니터링**: 실시간 상태 확인
- **상태 관리**: 복잡한 작업 상태 추적

### 예제
```cpp
// Action Server
auto action_server = rclcpp_action::create_server<Fibonacci>(
  node, "fibonacci", handle_goal, handle_cancel, handle_accepted);

// Action Client  
auto action_client = rclcpp_action::create_client<Fibonacci>(
  node, "fibonacci");
```

---

## 🎯 QoS (Quality of Service)

### QoS 정책
- **Reliability**: 신뢰성 (Best Effort, Reliable)
- **Durability**: 지속성 (Volatile, Transient Local)
- **History**: 히스토리 (Keep Last, Keep All)
- **Deadline**: 마감시간 제한
- **Lifespan**: 메시지 생존시간
- **Liveliness**: 생존성 확인

### 네트워크 최적화
- **Bandwidth**: 대역폭 제어
- **Latency**: 지연시간 최소화
- **Packet Loss**: 패킷 손실 처리
- **Congestion**: 네트워크 혼잡 관리

### QoS 프로파일
```cpp
// Sensor Data Profile
auto qos = rclcpp::SensorDataQoS();

// System Default Profile  
auto qos = rclcpp::SystemDefaultsQoS();

// Custom Profile
auto qos = rclcpp::QoS(10)
  .reliability(RMW_QOS_POLICY_RELIABILITY_RELIABLE)
  .durability(RMW_QOS_POLICY_DURABILITY_TRANSIENT_LOCAL);
```

---

## 🔍 발견 및 인트로스펙션

### 노드 발견
- **자동 발견**: DDS Discovery Protocol
- **메타데이터**: 인터페이스 정보 교환
- **네임스페이스**: 계층적 이름 공간
- **도메인 ID**: 네트워크 분리

### 인트로스펙션 도구
- **ros2 node list**: 노드 목록 조회
- **ros2 topic list**: Topic 목록 조회  
- **ros2 service list**: Service 목록 조회
- **ros2 action list**: Action 목록 조회
- **rqt_graph**: 그래프 시각화

---

## 📖 주요 참고문헌

1. **ROS2 Communication**: https://docs.ros.org/en/humble/Concepts/About-ROS-Interfaces.html
2. **QoS Design**: https://design.ros2.org/articles/qos.html
3. **DDS Interoperability**: DDS 표준 호환성 가이드
4. **Performance Tuning**: ROS2 통신 성능 최적화