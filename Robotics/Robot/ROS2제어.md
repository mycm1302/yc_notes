# ROS2제어

> 상위: [[ROS2]]

ros2_control 프레임워크를 활용한 하드웨어 독립적 로봇 제어 시스템을 다룹니다.

---

## 🎯 ros2_control 개요

### 핵심 개념
- **하드웨어 독립적**: 다양한 하드웨어 추상화
- **모듈화**: 컨트롤러와 하드웨어 인터페이스 분리
- **실시간성**: 실시간 제어 지원
- **플러그인 아키텍처**: 확장 가능한 구조

### 주요 구성요소
- **Controller Manager**: 컨트롤러 생명주기 관리
- **Hardware Interface**: 하드웨어 추상화 계층
- **Controllers**: 제어 알고리즘 구현
- **Resource Manager**: 하드웨어 리소스 관리

---

## 🏗️ 아키텍처

### 계층 구조
```
Application Layer
    ↓
Controllers (ros2_controllers)
    ↓
ros2_control Framework
    ↓
Hardware Interfaces
    ↓
Physical Hardware
```

### 주요 리포지토리
- **ros2_control**: 메인 프레임워크
- **ros2_controllers**: 표준 컨트롤러 모음
- **control_toolbox**: 제어 이론 구현체
- **realtime_tools**: 실시간 지원 도구

---

## ⚙️ 하드웨어 인터페이스

### 인터페이스 타입
- **Command Interface**: 명령 인터페이스 (position, velocity, effort)
- **State Interface**: 상태 인터페이스 (sensor data, feedback)
- **Custom Interface**: 사용자 정의 인터페이스

### 하드웨어 구성요소
```cpp
// System: 복합 하드웨어 시스템
class RobotSystemHardware : public hardware_interface::SystemInterface
{
  // 다중 관절 로봇, 복잡한 전달 메커니즘
};

// Actuator: 단일 액추에이터
class MotorHardware : public hardware_interface::ActuatorInterface
{
  // 단일 모터, 간단한 구동기
};

// Sensor: 센서 전용
class ForceTorqueSensor : public hardware_interface::SensorInterface
{
  // 힘/토크 센서, IMU, 카메라 등
};
```

---

## 🎮 표준 컨트롤러

### 기본 컨트롤러
- **JointTrajectoryController**: 관절 궤적 추종
- **DiffDriveController**: 차동 구동 제어
- **TricycleController**: 삼륜 차동 제어
- **MecanumDriveController**: 메카넘 휠 제어

### 특수 컨트롤러
- **ForwardCommandController**: 직접 명령 전달
- **EffortControllers**: 힘/토크 제어
- **VelocityControllers**: 속도 제어
- **PositionControllers**: 위치 제어

### 브로드캐스터
- **JointStateBroadcaster**: 관절 상태 발행
- **IMUBroadcaster**: IMU 데이터 발행
- **ForceTorqueSensorBroadcaster**: 힘/토크 센서 발행

---

## 📋 URDF 설정

### ros2_control 태그
```xml
<ros2_control name="RobotSystem" type="system">
  <hardware>
    <plugin>my_robot_hardware/MyRobotHardware</plugin>
    <param name="example_param_write_for_sec">2</param>
    <param name="example_param_read_for_sec">2</param>
  </hardware>
  
  <joint name="joint1">
    <command_interface name="position">
      <param name="min">-1</param>
      <param name="max">1</param>
    </command_interface>
    <state_interface name="position"/>
    <state_interface name="velocity"/>
    <state_interface name="effort"/>
  </joint>
  
  <sensor name="tcp_fts_sensor">
    <state_interface name="fx"/>
    <state_interface name="fy"/>
    <state_interface name="fz"/>
    <state_interface name="tx"/>
    <state_interface name="ty"/>
    <state_interface name="tz"/>
  </sensor>
</ros2_control>
```

---

## 🚀 컨트롤러 관리

### Controller Manager CLI
```bash
# 컨트롤러 목록 확인
ros2 control list_controllers

# 컨트롤러 로드
ros2 control load_controller joint_trajectory_controller

# 컨트롤러 시작
ros2 control switch_controllers --start joint_trajectory_controller

# 컨트롤러 정지
ros2 control switch_controllers --stop joint_trajectory_controller

# 하드웨어 인터페이스 정보
ros2 control list_hardware_interfaces
```

### 설정 파일 예제
```yaml
controller_manager:
  ros__parameters:
    update_rate: 100  # Hz
    
    joint_trajectory_controller:
      type: joint_trajectory_controller/JointTrajectoryController
      
    joint_state_broadcaster:
      type: joint_state_broadcaster/JointStateBroadcaster

joint_trajectory_controller:
  ros__parameters:
    joints:
      - joint1
      - joint2
      - joint3
    command_interfaces:
      - position
    state_interfaces:
      - position
      - velocity
```

---

## 🔧 실시간 성능

### 실시간 고려사항
- **Memory Management**: 동적 할당 최소화
- **Thread Priority**: 높은 우선순위 설정
- **Lock-free Communication**: 무잠금 통신
- **Bounded Execution**: 결정적 실행 시간

### 실시간 도구
```cpp
#include "realtime_tools/realtime_buffer.h"
#include "realtime_tools/realtime_publisher.h"

// 실시간 버퍼
realtime_tools::RealtimeBuffer<geometry_msgs::msg::Twist> command_buffer_;

// 실시간 퍼블리셔
realtime_tools::RealtimePublisher<sensor_msgs::msg::JointState> 
  realtime_pub_;
```

---

## 🧪 시뮬레이션 통합

### Gazebo 통합
```xml
<!-- Gazebo ros2_control 플러그인 -->
<gazebo>
  <plugin filename="libgazebo_ros2_control.so" 
          name="gazebo_ros2_control">
    <parameters>$(find robot_description)/config/controllers.yaml</parameters>
  </plugin>
</gazebo>
```

### Mock Hardware
```yaml
# 테스트용 가상 하드웨어
hardware:
  plugin: mock_components/GenericSystem
  param_file: mock_hardware_params.yaml
```

---

## 🔍 디버깅 도구

### rqt 플러그인
- **rqt_controller_manager**: 컨트롤러 관리 GUI
- **rqt_joint_trajectory_controller**: 궤적 제어 GUI
- **plotjuggler**: 실시간 데이터 시각화

### 로깅 및 모니터링
```bash
# 컨트롤러 상태 모니터링
ros2 topic echo /controller_manager/robot_description

# 성능 분석
ros2 run ros2_control_test_assets test_controller_manager_performance
```

---

## 📖 주요 참고문헌

1. **ros2_control Documentation**: https://control.ros.org/rolling/index.html
2. **ros2_control GitHub**: https://github.com/ros-controls/ros2_control
3. **Getting Started Guide**: https://control.ros.org/master/doc/getting_started/getting_started.html
4. **ros2_control Demos**: https://control.ros.org/master/doc/ros2_control_demos/doc/index.html
5. **ros2_controllers**: https://control.ros.org/master/doc/ros2_controllers/doc/controllers_index.html