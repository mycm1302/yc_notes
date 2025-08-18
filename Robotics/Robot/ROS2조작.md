# ROS2조작

> 상위: [[ROS2]]

MoveIt2 프레임워크를 활용한 로봇 매니퓰레이션과 모션 플래닝을 다룹니다.

---

## 🤖 MoveIt2 개요

### 핵심 기능
- **Motion Planning**: 충돌 회피 경로 계획
- **Kinematics**: 순/역기구학 해석
- **Collision Detection**: 3D 충돌 감지
- **Grasping**: 파지 계획 및 실행

### MoveIt2 특징
- **Real-time Performance**: ROS2 DDS 기반 실시간 성능
- **Modular Architecture**: 플러그인 기반 확장성
- **3D Perception**: 점군 및 깊이 센서 통합
- **Industrial Ready**: 산업용 로봇 지원

---

## 🏗️ MoveIt2 아키텍처

### 핵심 구성요소
```
Move Group Node (중앙 조정자)
    ↓
├── Planning Scene (환경 모델링)
├── Motion Planner (경로 계획)
├── Trajectory Execution (궤적 실행)
├── Kinematics Solver (기구학 해석)
└── Collision Checker (충돌 검사)
```

### Planning Pipeline
- **Planning Request**: 목표 상태 요청
- **Planning Scene**: 환경 및 로봇 상태
- **Motion Planner**: 경로 생성
- **Trajectory Processor**: 궤적 후처리
- **Execution Manager**: 실행 관리

---

## 📐 기구학 해석

### 순기구학 (Forward Kinematics)
```cpp
// 관절 각도 → 말단 위치
robot_state::RobotState robot_state(kinematic_model);
robot_state.setJointGroupPositions(joint_model_group, joint_values);

const Eigen::Isometry3d& end_effector_state = 
  robot_state.getGlobalLinkTransform("panda_link8");
```

### 역기구학 (Inverse Kinematics)
```cpp
// 말단 위치 → 관절 각도
geometry_msgs::msg::Pose target_pose;
target_pose.position.x = 0.28;
target_pose.position.y = 0.0;
target_pose.position.z = 0.5;

bool found_ik = robot_state.setFromIK(joint_model_group, target_pose);
```

### Kinematics Solvers
- **KDL Kinematics Plugin**: 기본 KDL 기반 해석기
- **TRAC-IK**: Track-based Inverse Kinematics
- **BioIK**: 생체영감 최적화 기법
- **Cached IK**: 캐시된 역기구학 솔버

---

## 🛣️ 모션 플래닝

### Planning Algorithms
- **OMPL (Open Motion Planning Library)**
  - **RRT**: Rapidly-exploring Random Tree
  - **RRT Connect**: 양방향 RRT
  - **PRM**: Probabilistic Roadmap
  - **EST**: Expansive Space Trees

### Planner 설정
```yaml
move_group:
  planning_plugin: ompl_interface/OMPLPlanner
  
  planning_pipelines:
    - ompl
    
  planner_configs:
    RRTConnect:
      type: geometric::RRTConnect
      range: 0.0  # 0은 최대 확장 거리 사용
      
    PRM:
      type: geometric::PRM
      max_nearest_neighbors: 10
```

---

## 🎯 MoveIt Servo

### Real-time Servoing
- **Velocity Control**: 속도 기반 제어
- **Jacobian-based**: 야코비안 기반 계산
- **Collision Avoidance**: 실시간 충돌 회피
- **Composable Node**: 컴포넌트 기반 실행

### Servo 설정
```yaml
servo:
  ## Parameters for Servo node
  use_gazebo: false
  status_topic: /servo_server/status
  
  ## Properties of incoming commands
  command_in_type: "speed_units"  # "unitless" or "speed_units"
  scale:
    # Scale parameters are only used if command_in_type=="unitless"
    linear: 0.4  # Max linear velocity. Meters per publish_period
    rotational: 0.8 # Max angular velocity. Rads per publish_period
    
  # Servo properties
  publish_period: 0.034  # 1/Nominal publish rate [seconds]
```

---

## 🔧 Planning Scene

### 환경 모델링
```cpp
// Planning Scene 생성
planning_scene::PlanningScene planning_scene(kinematic_model);

// 충돌 객체 추가
moveit_msgs::msg::CollisionObject collision_object;
collision_object.header.frame_id = "panda_link0";
collision_object.id = "box1";

// 박스 형태 정의
shape_msgs::msg::SolidPrimitive primitive;
primitive.type = primitive.BOX;
primitive.dimensions = {0.4, 0.1, 0.1};

collision_object.primitives.push_back(primitive);
planning_scene.getWorldNonConst()->addToObject("box1", primitive, pose);
```

### Octomap 통합
```yaml
planning_scene_monitor:
  publish_planning_scene: true
  publish_geometry_updates: true
  publish_state_updates: true
  publish_transforms_updates: true
  
sensors:
  - sensor_plugin: occupancy_map_monitor/PointCloudOctomapUpdater
    point_cloud_topic: /camera/depth/color/points
    max_range: 5.0
    point_subsample: 1
    padding_offset: 0.1
    padding_scale: 1.0
```

---

## 🎮 MoveGroupInterface 사용

### C++ 인터페이스
```cpp
#include <moveit/move_group_interface/move_group_interface.h>

// Move Group 인터페이스 생성
moveit::planning_interface::MoveGroupInterface move_group("panda_arm");

// 목표 위치 설정
geometry_msgs::msg::Pose target_pose;
target_pose.position.x = 0.28;
target_pose.position.y = -0.2;
target_pose.position.z = 0.5;
move_group.setPoseTarget(target_pose);

// 계획 및 실행
moveit::planning_interface::MoveGroupInterface::Plan my_plan;
bool success = (move_group.plan(my_plan) == 
                moveit::planning_interface::MoveItErrorCode::SUCCESS);

if (success) {
  move_group.execute(my_plan);
}
```

### Python 인터페이스
```python
import moveit_commander
import geometry_msgs.msg

# Move Group 초기화
move_group = moveit_commander.MoveGroupCommander("panda_arm")

# 목표 위치 설정
pose_target = geometry_msgs.msg.Pose()
pose_target.position.x = 0.28
pose_target.position.y = -0.2
pose_target.position.z = 0.5

move_group.set_pose_target(pose_target)

# 계획 및 실행
plan = move_group.go(wait=True)
move_group.stop()
move_group.clear_pose_targets()
```

---

## 🛠️ ros2_control 통합

### MoveIt과 ros2_control 연결
```yaml
# controllers.yaml
controller_manager:
  ros__parameters:
    joint_trajectory_controller:
      type: joint_trajectory_controller/JointTrajectoryController

joint_trajectory_controller:
  ros__parameters:
    joints:
      - panda_joint1
      - panda_joint2
      - panda_joint3
      - panda_joint4
      - panda_joint5
      - panda_joint6
      - panda_joint7
    
    command_interfaces:
      - position
    state_interfaces:
      - position
      - velocity
```

### MoveIt 컨트롤러 설정
```yaml
# moveit_controllers.yaml
moveit_controller_manager: moveit_simple_controller_manager/MoveItSimpleControllerManager

moveit_simple_controller_manager:
  controller_names:
    - joint_trajectory_controller
    - hand_controller

  joint_trajectory_controller:
    type: FollowJointTrajectory
    action_ns: follow_joint_trajectory
    default: true
    joints:
      - panda_joint1
      - panda_joint2
      - panda_joint3
      - panda_joint4
      - panda_joint5
      - panda_joint6
      - panda_joint7
```

---

## 🔍 3D 인식 및 충돌 감지

### Point Cloud 처리
```cpp
// Point Cloud → Octomap 변환
octomap::OcTree* octree = new octomap::OcTree(0.1);  // 해상도 10cm

for (const auto& point : point_cloud.points) {
  octree->updateNode(point.x, point.y, point.z, true);
}

// Planning Scene에 추가
planning_scene.getWorldNonConst()->setOctomap(
  std::shared_ptr<octomap::OcTree>(octree));
```

### 충돌 검사
```cpp
// 충돌 검사 요청
collision_detection::CollisionRequest collision_request;
collision_detection::CollisionResult collision_result;

planning_scene.checkCollision(collision_request, collision_result);

if (collision_result.collision) {
  RCLCPP_INFO(LOGGER, "Robot is in collision");
}
```

---

## 📊 시각화 및 디버깅

### RViz2 플러그인
- **Motion Planning**: 대화형 계획 도구
- **Planning Scene**: 환경 시각화
- **Robot State**: 로봇 상태 표시
- **Trajectory**: 궤적 애니메이션

### 디버깅 도구
```bash
# MoveIt 상태 확인
ros2 topic echo /move_group/status

# 계획 요청 모니터링
ros2 topic echo /move_group/goal

# 궤적 실행 상태
ros2 topic echo /joint_trajectory_controller/follow_joint_trajectory/status
```

---

## 📖 주요 참고문헌

1. **MoveIt2 Documentation**: https://moveit.ai/
2. **MoveIt2 GitHub**: https://github.com/moveit/moveit2
3. **MoveIt2 Tutorials**: https://moveit.picknik.ai/humble/index.html
4. **OMPL Documentation**: https://ompl.kavrakilab.org/
5. **MoveIt Servo**: https://moveit.ai/moveit/ros2/servo/jog/2020/09/09/moveit2-servo.html