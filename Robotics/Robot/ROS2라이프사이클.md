# ROS2라이프사이클

> 상위: [[ROS2]]

Node lifecycle과 상태 관리를 통한 체계적인 노드 생명주기 제어를 다룹니다.

---

## 🔄 노드 라이프사이클

### 라이프사이클 상태
- **Unconfigured**: 초기 상태, 설정 전
- **Inactive**: 설정 완료, 비활성 상태
- **Active**: 정상 동작 상태
- **Finalized**: 종료된 상태

### 전이 상태
- **Configuring**: 설정 중
- **Activating**: 활성화 중
- **Deactivating**: 비활성화 중
- **CleaningUp**: 정리 중
- **ShuttingDown**: 종료 중
- **ErrorProcessing**: 오류 처리 중

---

## ⚡ 상태 전이

### 기본 전이
- **configure()**: Unconfigured → Inactive
- **activate()**: Inactive → Active  
- **deactivate()**: Active → Inactive
- **cleanup()**: Inactive → Unconfigured
- **shutdown()**: Any State → Finalized

### 오류 처리
- **on_error()**: 오류 발생 시 호출
- **Error Recovery**: 오류 복구 메커니즘
- **Failure Handling**: 실패 상황 처리
- **Safe Shutdown**: 안전한 종료 절차

---

## 🛠️ 라이프사이클 노드 구현

### LifecycleNode 클래스
```cpp
class MyLifecycleNode : public rclcpp_lifecycle::LifecycleNode
{
public:
  explicit MyLifecycleNode(const rclcpp::NodeOptions & options)
  : rclcpp_lifecycle::LifecycleNode("my_lifecycle_node", options)
  {
  }

  // 상태 전이 콜백 함수들
  rclcpp_lifecycle::node_interfaces::LifecycleNodeInterface::CallbackReturn
  on_configure(const rclcpp_lifecycle::State &);
  
  rclcpp_lifecycle::node_interfaces::LifecycleNodeInterface::CallbackReturn
  on_activate(const rclcpp_lifecycle::State &);
  
  // ... 기타 전이 콜백들
};
```

### 전이 콜백 구현
- **on_configure()**: 리소스 할당, 파라미터 로딩
- **on_activate()**: 통신 인터페이스 활성화
- **on_deactivate()**: 통신 인터페이스 비활성화
- **on_cleanup()**: 리소스 해제
- **on_shutdown()**: 최종 정리 작업

---

## 🎛️ 라이프사이클 관리

### 라이프사이클 매니저
- **Launch Integration**: Launch 파일과 통합
- **Service Interface**: 원격 상태 제어
- **Batch Operations**: 여러 노드 일괄 제어
- **Dependency Management**: 의존성 기반 순서 제어

### 관리 명령어
```bash
# 노드 상태 확인
ros2 lifecycle get /my_lifecycle_node

# 상태 전이 명령
ros2 lifecycle set /my_lifecycle_node configure
ros2 lifecycle set /my_lifecycle_node activate

# 상태 목록 조회
ros2 lifecycle list /my_lifecycle_node
```

---

## 🔗 상태 의존성

### 의존성 관리
- **Startup Sequence**: 시작 순서 관리
- **Shutdown Sequence**: 종료 순서 관리
- **Health Monitoring**: 상태 건전성 모니터링
- **Cascade Control**: 계단식 제어

### 시스템 통합
- **Service Dependencies**: 서비스 의존성
- **Hardware Dependencies**: 하드웨어 의존성
- **Network Dependencies**: 네트워크 의존성
- **Resource Dependencies**: 리소스 의존성

---

## 📊 상태 모니터링

### 상태 발행
- **State Publisher**: 상태 변경 알림
- **State Subscriber**: 상태 변경 수신
- **Event Monitoring**: 전이 이벤트 추적
- **Diagnostics**: 진단 정보 제공

### 시각화 도구
- **rqt_lifecycle**: GUI 라이프사이클 도구
- **rqt_graph**: 상태 그래프 시각화
- **Node Monitor**: 노드 상태 모니터링
- **System Dashboard**: 시스템 대시보드

---

## ⚙️ 고급 기능

### 자동 상태 관리
- **Auto Transition**: 조건부 자동 전이
- **Watchdog**: 감시 타이머 기능
- **Health Check**: 주기적 건전성 검사
- **Recovery Mechanisms**: 자동 복구 메커니즘

### 커스텀 전이
- **Custom States**: 사용자 정의 상태
- **Extended Transitions**: 확장 전이 규칙
- **State Machines**: 복잡한 상태 머신
- **Behavior Trees**: 행동 트리 통합

---

## 🏗️ 시스템 아키텍처

### 계층적 제어
- **System Level**: 시스템 수준 제어
- **Subsystem Level**: 하위 시스템 제어
- **Component Level**: 컴포넌트 수준 제어
- **Node Level**: 개별 노드 제어

### 분산 관리
- **Centralized Control**: 중앙 집중식 제어
- **Distributed Control**: 분산 제어
- **Federated Control**: 연합 제어
- **Autonomous Control**: 자율 제어

---

## 📖 주요 참고문헌

1. **ROS2 Lifecycle**: https://design.ros2.org/articles/node_lifecycle.html
2. **Navigation2 Lifecycle**: Nav2에서의 라이프사이클 활용
3. **System Integration**: 라이프사이클 기반 시스템 통합
4. **Best Practices**: 라이프사이클 노드 설계 모범 사례