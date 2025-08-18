# ROS2내비게이션

> 상위: [[ROS2]]

Nav2 프레임워크를 통한 자율 내비게이션 시스템 구축과 최적화를 다룹니다.

---

## 🧭 Nav2 개요

### 핵심 기능
- **경로 계획**: 최적 경로 생성
- **장애물 회피**: 동적/정적 장애물 처리
- **위치 추정**: AMCL 기반 자기 위치 인식
- **지도 관리**: 점유 지도 활용

### Navigation 2 특징
- **Behavior Tree**: 지능적 행동 제어
- **모듈화**: 독립적 서버 구조
- **다양한 로봇**: Holonomic, Differential, Ackermann 지원
- **실시간 성능**: 산업급 안정성

---

## 🏗️ Nav2 아키텍처

### 핵심 서버
```
BT Navigator (행동 트리 관리)
    ↓
├── Planner Server (전역 경로 계획)
├── Controller Server (로컬 제어)
├── Recovery Server (복구 행동)
├── Waypoint Follower (웨이포인트 추종)
└── Behavior Tree Server (행동 관리)
```

### 지원 구성요소
- **Map Server**: 지도 서비스
- **AMCL**: 적응형 몬테카를로 위치추정
- **Costmap 2D**: 비용 지도 관리
- **Lifecycle Manager**: 노드 생명주기 관리

---

## 🗺️ 지도 생성 및 관리

### SLAM을 이용한 지도 생성
```bash
# SLAM Toolbox 실행
ros2 launch slam_toolbox online_async_launch.py

# 지도 저장
ros2 run nav2_map_server map_saver_cli -f my_map

# 생성된 파일
my_map.pgm    # 지도 이미지 (흑백 픽셀)
my_map.yaml   # 지도 메타데이터
```

### 지도 파일 구조
```yaml
# my_map.yaml
image: my_map.pgm
resolution: 0.05    # 미터/픽셀
origin: [-10.0, -10.0, 0.0]  # 지도 원점
negate: 0           # 색상 반전 여부
occupied_thresh: 0.65    # 점유 임계값
free_thresh: 0.196       # 자유공간 임계값
```

---

## 🎯 경로 계획

### 전역 경로 계획기
- **NavFn Planner**: Dijkstra 기반 계획
- **Smac Planner**: A* 기반 계획
  - **Smac Planner 2D**: 2D 그리드 기반
  - **Smac Hybrid-A***: 비홀로노믹 로봇용
  - **Smac State Lattice**: 격자 상태 공간

### 로컬 제어기
- **DWB Controller**: Dynamic Window Approach
- **Regulated Pure Pursuit**: 순수 추종 제어
- **TEB Controller**: Timed Elastic Band
- **MPPI Controller**: Model Predictive Path Integral

### 설정 예제
```yaml
planner_server:
  ros__parameters:
    planner_plugins: ["GridBased"]
    GridBased:
      plugin: "nav2_navfn_planner/NavfnPlanner"
      tolerance: 0.5
      use_astar: false

controller_server:
  ros__parameters:
    controller_plugins: ["FollowPath"]
    FollowPath:
      plugin: "nav2_regulated_pure_pursuit_controller/RegulatedPurePursuitController"
      desired_linear_vel: 0.5
      lookahead_dist: 0.6
```

---

## 📍 위치 추정 (AMCL)

### AMCL 개념
- **파티클 필터**: 확률적 위치 추정
- **센서 모델**: 라이다 기반 매칭
- **모션 모델**: 오도메트리 통합
- **적응적 샘플링**: 동적 파티클 수 조정

### AMCL 설정
```yaml
amcl:
  ros__parameters:
    # 파티클 필터 설정
    min_particles: 500
    max_particles: 2000
    
    # 모션 모델 (차동구동)
    robot_model_type: "nav2_amcl::DifferentialMotionModel"
    
    # 센서 모델
    laser_model_type: "likelihood_field"
    
    # 위치 추정 파라미터
    transform_tolerance: 1.0
    alpha1: 0.2  # 회전 잡음
    alpha2: 0.2  # 이동 잡음
```

---

## 🛡️ 비용 지도 (Costmap)

### 계층화된 구조
- **Static Layer**: 정적 지도 기반
- **Obstacle Layer**: 센서 기반 장애물
- **Inflation Layer**: 장애물 팽창 영역
- **Voxel Layer**: 3D 복셀 기반 (선택적)

### 비용 지도 설정
```yaml
global_costmap:
  global_costmap:
    ros__parameters:
      footprint: "[[0.3, 0.3], [0.3, -0.3], [-0.3, -0.3], [-0.3, 0.3]]"
      
      plugins: ["static_layer", "obstacle_layer", "inflation_layer"]
      
      static_layer:
        plugin: "nav2_costmap_2d::StaticLayer"
        map_subscribe_transient_local: True
        
      obstacle_layer:
        plugin: "nav2_costmap_2d::ObstacleLayer"
        observation_sources: scan
        
      inflation_layer:
        plugin: "nav2_costmap_2d::InflationLayer"
        cost_scaling_factor: 3.0
        inflation_radius: 0.55
```

---

## 🌳 Behavior Tree 활용

### 기본 BT 구조
```xml
<root main_tree_to_execute="MainTree">
  <BehaviorTree ID="MainTree">
    <RecoveryNode number_of_retries="6">
      <PipelineSequence>
        <RateController hz="1.0">
          <ComputePathToPose goal="{goal}" path="{path}"/>
        </RateController>
        <FollowPath path="{path}"/>
      </PipelineSequence>
      <ReactiveFallback>
        <GoalUpdated/>
        <Sequence>
          <ClearEntireCostmap service_name="global_costmap/clear_entirely_global_costmap"/>
          <ClearEntireCostmap service_name="local_costmap/clear_entirely_local_costmap"/>
        </Sequence>
      </ReactiveFallback>
    </RecoveryNode>
  </BehaviorTree>
</root>
```

---

## 🚀 Nav2 실행

### 기본 실행 명령
```bash
# Nav2 전체 스택 실행
ros2 launch nav2_bringup tb3_simulation_launch.py

# 특정 구성으로 실행
ros2 launch nav2_bringup navigation_launch.py \
  use_sim_time:=true \
  map:=/path/to/map.yaml

# AMCL 없이 실행 (오도메트리만 사용)
ros2 launch nav2_bringup navigation_launch.py \
  use_sim_time:=true \
  map:=/path/to/map.yaml \
  use_amcl:=false
```

### 목표 설정
```bash
# CLI로 목표 설정
ros2 action send_goal /navigate_to_pose nav2_msgs/action/NavigateToPose \
  "{pose: {header: {stamp: {sec: 0}, frame_id: 'map'}, 
           pose: {position: {x: 2.0, y: 0.5, z: 0.0}}}}"

# Python으로 목표 설정
import rclpy
from nav2_simple_commander.robot_navigator import BasicNavigator

navigator = BasicNavigator()
goal_pose = navigator.getPoseStamped([2.0, 0.5], 0.0)
navigator.goToPose(goal_pose)
```

---

## 🔧 매개변수 튜닝

### 성능 최적화 팁
- **DWB Controller 속도 제한**
  ```yaml
  max_vel_x: 0.5
  min_vel_x: -0.5
  max_vel_theta: 1.0
  ```

- **Pure Pursuit 거리 조정**
  ```yaml
  lookahead_dist: 0.6
  min_lookahead_dist: 0.3
  max_lookahead_dist: 0.9
  ```

- **비용 지도 해상도**
  ```yaml
  resolution: 0.05  # 5cm 해상도
  update_frequency: 5.0
  publish_frequency: 2.0
  ```

---

## 📊 모니터링 및 디버깅

### RViz2 시각화
- **Global Costmap**: 전역 비용 지도
- **Local Costmap**: 로컬 비용 지도
- **Global Path**: 전역 경로
- **Local Path**: 로컬 경로
- **Particle Cloud**: AMCL 파티클

### 디버깅 도구
```bash
# Nav2 상태 확인
ros2 topic echo /behavior_tree_log

# 비용 지도 모니터링
ros2 topic echo /global_costmap/costmap

# 성능 분석
ros2 run nav2_util lifecycle_bringup
```

---

## 📖 주요 참고문헌

1. **Nav2 Documentation**: https://docs.nav2.org/
2. **Nav2 GitHub**: https://github.com/ros-navigation/navigation2
3. **Nav2 Tuning Guide**: https://automaticaddison.com/ros-2-navigation-tuning-guide-nav2/
4. **Navigation Concepts**: https://docs.nav2.org/concepts/index.html
5. **BehaviorTree.CPP**: https://www.behaviortree.dev/