# ROS2아키텍처

> 상위: [[ROS2]]

DDS(Data Distribution Service) 기반의 분산 통신 아키텍처와 계층적 노드 구조를 다룹니다.

---

## 🏗️ 전체 아키텍처

### 계층 구조
- **응용 계층**: ROS2 노드 및 프로세스
- **RCL 계층**: ROS Client Library (rclcpp, rclpy)
- **RMW 계층**: ROS Middleware (DDS 추상화)
- **DDS 계층**: Data Distribution Service 구현체

### 핵심 구성요소
- **노드(Node)**: 독립적인 프로세스 단위
- **그래프(Graph)**: 노드들의 연결 구조
- **도메인(Domain)**: 논리적 네트워크 분리
- **컨텍스트(Context)**: 실행 환경 관리

---

## 🌐 DDS 기반 통신

### DDS 특징
- **발행-구독 모델**: Publisher-Subscriber 패턴
- **데이터 중심**: Topic 기반 통신
- **자동 발견**: 네트워크 상의 엔드포인트 자동 발견
- **QoS 정책**: 통신 품질 관리

### RMW 구현체
- **Fast DDS**: 기본 구현체 (eProsima)
- **CycloneDX**: Eclipse Cyclone DDS
- **RTI Connext**: 상용 DDS 솔루션
- **Gurum DDS**: 국산 DDS 구현체

---

## 📊 노드 그래프

### 그래프 구조
- **Directed Graph**: 방향성 그래프
- **노드**: 그래프의 정점 (Vertex)
- **연결**: Topic, Service, Action 통신
- **네임스페이스**: 계층적 이름 관리

### 발견 메커니즘
- **자동 발견**: 같은 도메인 내 자동 연결
- **메타데이터 교환**: 인터페이스 정보 공유
- **동적 연결**: 런타임 연결/해제
- **참여자 발견**: 새로운 노드 참여 감지

---

## 🔄 노드 라이프사이클

### 라이프사이클 상태
- **Unconfigured**: 초기 상태
- **Inactive**: 설정 완료, 비활성
- **Active**: 정상 동작 상태
- **Finalized**: 종료 상태

### 상태 전이
- **configure()**: Unconfigured → Inactive
- **activate()**: Inactive → Active
- **deactivate()**: Active → Inactive
- **cleanup()**: Inactive → Unconfigured
- **shutdown()**: Any → Finalized

---

## ⚙️ 실행 환경

### 실행자(Executor)
- **SingleThreadedExecutor**: 단일 스레드 실행
- **MultiThreadedExecutor**: 멀티 스레드 실행
- **StaticSingleThreadedExecutor**: 정적 단일 스레드
- **EventsExecutor**: 이벤트 기반 실행

### 메모리 관리
- **사전 할당**: 실시간성 보장
- **메모리 풀**: 동적 할당 최소화
- **참조 카운팅**: 스마트 포인터 활용
- **RAII 패턴**: 리소스 자동 관리

---

## 🔒 보안 아키텍처

### SROS2 (Secure ROS2)
- **인증**: 노드 신원 확인
- **권한 부여**: 접근 제어 정책
- **암호화**: 데이터 전송 보안
- **무결성**: 메시지 변조 방지

### DDS Security
- **PKI 인증**: 공개키 기반 인프라
- **접근 제어**: Topic별 권한 관리
- **암호화**: AES 기반 데이터 보호
- **키 관리**: 동적 키 배포

---

## 📖 주요 참고문헌

1. **ROS2 Architecture**: https://design.ros2.org/articles/architecture.html
2. **DDS for Robotics**: DDS 기반 로봇 시스템 설계
3. **ROS2 Security**: https://design.ros2.org/articles/ros2_security_design.html
4. **Real-time ROS2**: 실시간 시스템을 위한 ROS2 아키텍처