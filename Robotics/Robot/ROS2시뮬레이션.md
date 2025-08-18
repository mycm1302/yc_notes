# ROS2시뮬레이션

> 상위: [[ROS2]]

Gazebo와 다양한 시뮬레이터를 활용한 ROS2 기반 로봇 시뮬레이션 환경 구축을 다룹니다.

---

## 🎮 ROS2 시뮬레이션 개요

### 핵심 시뮬레이터
- **Gazebo (Ignition)**: 물리 엔진 기반 3D 시뮬레이션
- **CoppeliaSim**: 다중 물리 엔진 시뮬레이션
- **Webots**: 실시간 로봇 시뮬레이션
- **Isaac Sim**: NVIDIA GPU 가속 시뮬레이션

### 시뮬레이션 장점
- **안전한 테스트**: 실제 하드웨어 손상 없음
- **빠른 프로토타이핑**: 빠른 알고리즘 검증
- **반복 실험**: 동일 조건 재현 가능
- **비용 절약**: 하드웨어 구입 없이 개발

---

## 🏗️ Gazebo 통합

### Gazebo 설치
```bash
# Gazebo 설치 (Humble 기준)
sudo apt install gazebo
sudo apt install ros-humble-gazebo-ros-pkgs

# 설치 확인
gazebo --version
```

### ros_gz_bridge 설정
```bash
# Bridge 패키지 설치
sudo apt install ros-humble-ros-gz-bridge

# Topic Bridge 실행
ros2 run ros_gz_bridge parameter_bridge \
  /model/robot/cmd_vel@geometry_msgs/msg/Twist]ignition.msgs.Twist

# 센서 데이터 Bridge
ros2 run ros_gz_bridge parameter_bridge \
  /lidar2@sensor_msgs/msg/LaserScan[ignition.msgs.LaserScan \
  --ros-args -r /lidar2:=/scan
```

---

## 🤖 URDF/SDF 로봇 정의

### URDF 로봇 모델
```xml
<?xml version="1.0"?>
<robot name="mobile_robot">
  
  <!-- Base Link -->
  <link name="base_link">
    <visual>
      <geometry>
        <box size="0.6 0.4 0.2"/>
      </geometry>
      <material name="blue">
        <color rgba="0 0 1 1"/>
      </material>
    </visual>
    <collision>
      <geometry>
        <box size="0.6 0.4 0.2"/>
      </geometry>
    </collision>
    <inertial>
      <mass value="10"/>
      <inertia ixx="0.4" iyy="0.4" izz="0.2" ixy="0" ixz="0" iyz="0"/>
    </inertial>
  </link>

  <!-- Wheels -->
  <link name="left_wheel">
    <visual>
      <geometry>
        <cylinder radius="0.1" length="0.05"/>
      </geometry>
    </visual>
    <collision>
      <geometry>
        <cylinder radius="0.1" length="0.05"/>
      </geometry>
    </collision>
    <inertial>
      <mass value="1"/>
      <inertia ixx="0.01" iyy="0.01" izz="0.01" ixy="0" ixz="0" iyz="0"/>
    </inertial>
  </link>

  <joint name="left_wheel_joint" type="continuous">
    <parent link="base_link"/>
    <child link="left_wheel"/>
    <origin xyz="0 0.22 -0.1" rpy="-1.5708 0 0"/>
    <axis xyz="0 0 1"/>
  </joint>

</robot>
```

### Gazebo 플러그인 추가
```xml
<gazebo>
  <!-- Differential Drive Plugin -->
  <plugin name="differential_drive_controller" 
          filename="libgazebo_ros_diff_drive.so">
    <update_rate>30</update_rate>
    <left_joint>left_wheel_joint</left_joint>
    <right_joint>right_wheel_joint</right_joint>
    <wheel_separation>0.44</wheel_separation>
    <wheel_diameter>0.2</wheel_diameter>
    <max_wheel_torque>20</max_wheel_torque>
    <max_wheel_acceleration>1.0</max_wheel_acceleration>
    <command_topic>cmd_vel</command_topic>
    <publish_odom>true</publish_odom>
    <publish_odom_tf>true</publish_odom_tf>
    <odometry_topic>odom</odometry_topic>
    <odometry_frame>odom</odometry_frame>
    <robot_base_frame>base_link</robot_base_frame>
  </plugin>
  
  <!-- Lidar Plugin -->
  <plugin name="gpu_lidar" filename="libgazebo_ros_ray_sensor.so">
    <ros>
      <remapping>~/out:=scan</remapping>
    </ros>
    <output_type>sensor_msgs/LaserScan</output_type>
    <frame_name>lidar_link</frame_name>
  </plugin>
</gazebo>
```

---

## 🌍 World 환경 구성

### Gazebo World 파일
```xml
<?xml version="1.0" ?>
<sdf version="1.5">
  <world name="warehouse">
    
    <!-- 조명 설정 -->
    <include>
      <uri>model://sun</uri>
    </include>
    
    <!-- 바닥 -->
    <include>
      <uri>model://ground_plane</uri>
    </include>
    
    <!-- 벽 -->
    <model name="wall">
      <static>true</static>
      <link name="wall_link">
        <visual name="wall_visual">
          <geometry>
            <box>
              <size>10 0.1 2</size>
            </box>
          </geometry>
          <material>
            <script>
              <name>Gazebo/Wood</name>
            </script>
          </material>
        </visual>
        <collision name="wall_collision">
          <geometry>
            <box>
              <size>10 0.1 2</size>
            </box>
          </geometry>
        </collision>
      </link>
      <pose>0 5 1 0 0 0</pose>
    </model>
    
  </world>
</sdf>
```

### Launch 파일 생성
```python
import os
from launch import LaunchDescription
from launch.actions import IncludeLaunchDescription, ExecuteProcess
from launch.launch_description_sources import PythonLaunchDescriptionSource
from launch_ros.actions import Node
from ament_index_python.packages import get_package_share_directory

def generate_launch_description():
    pkg_share = get_package_share_directory('robot_simulation')
    
    # World 파일 경로
    world_file = os.path.join(pkg_share, 'worlds', 'warehouse.world')
    
    # Gazebo 실행
    gazebo_server = ExecuteProcess(
        cmd=['gzserver', '--verbose', world_file],
        output='screen'
    )
    
    gazebo_client = ExecuteProcess(
        cmd=['gzclient'],
        output='screen'
    )
    
    # 로봇 Spawn
    spawn_robot = Node(
        package='gazebo_ros',
        executable='spawn_entity.py',
        arguments=['-entity', 'mobile_robot',
                  '-topic', '/robot_description',
                  '-x', '0', '-y', '0', '-z', '0.1'],
        output='screen'
    )
    
    # Robot State Publisher
    robot_state_publisher = Node(
        package='robot_state_publisher',
        executable='robot_state_publisher',
        name='robot_state_publisher',
        parameters=[{'robot_description': robot_description}],
        output='screen'
    )
    
    return LaunchDescription([
        gazebo_server,
        gazebo_client,
        robot_state_publisher,
        spawn_robot
    ])
```

---

## 📡 센서 시뮬레이션

### 라이다 센서
```xml
<gazebo reference="lidar_link">
  <sensor type="ray" name="head_hokuyo_sensor">
    <pose>0 0 0 0 0 0</pose>
    <visualize>false</visualize>
    <update_rate>40</update_rate>
    <ray>
      <scan>
        <horizontal>
          <samples>720</samples>
          <resolution>1</resolution>
          <min_angle>-1.570796</min_angle>
          <max_angle>1.570796</max_angle>
        </horizontal>
      </scan>
      <range>
        <min>0.10</min>
        <max>30.0</max>
        <resolution>0.01</resolution>
      </range>
      <noise>
        <type>gaussian</type>
        <mean>0.0</mean>
        <stddev>0.01</stddev>
      </noise>
    </ray>
    <plugin name="gazebo_ros_head_hokuyo_controller" 
            filename="libgazebo_ros_ray_sensor.so">
      <ros>
        <remapping>~/out:=scan</remapping>
      </ros>
      <output_type>sensor_msgs/LaserScan</output_type>
      <frame_name>lidar_link</frame_name>
    </plugin>
  </sensor>
</gazebo>
```

### 카메라 센서
```xml
<gazebo reference="camera_link">
  <sensor type="camera" name="camera1">
    <update_rate>30.0</update_rate>
    <camera name="head">
      <horizontal_fov>1.3962634</horizontal_fov>
      <image>
        <width>800</width>
        <height>600</height>
        <format>R8G8B8</format>
      </image>
      <clip>
        <near>0.02</near>
        <far>300</far>
      </clip>
    </camera>
    <plugin name="camera_controller" filename="libgazebo_ros_camera.so">
      <ros>
        <remapping>image_raw:=camera/image_raw</remapping>
        <remapping>camera_info:=camera/camera_info</remapping>
      </ros>
      <camera_name>head</camera_name>
      <frame_name>camera_link</frame_name>
      <hack_baseline>0.07</hack_baseline>
    </plugin>
  </sensor>
</gazebo>
```

---

## 🎛️ 로봇 제어

### 키보드 텔레오프
```bash
# 키보드 제어 노드 실행
ros2 run teleop_twist_keyboard teleop_twist_keyboard \
  --ros-args -r /cmd_vel:=/mobile_robot/cmd_vel
```

### 프로그래밍 제어
```python
import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist

class RobotController(Node):
    def __init__(self):
        super().__init__('robot_controller')
        self.publisher = self.create_publisher(
            Twist, '/mobile_robot/cmd_vel', 10)
        
        # 1초마다 명령 전송
        self.timer = self.create_timer(1.0, self.move_robot)
        self.step = 0
        
    def move_robot(self):
        msg = Twist()
        
        # 간단한 사각형 경로
        if self.step < 5:  # 전진
            msg.linear.x = 0.5
        elif self.step < 7:  # 회전
            msg.angular.z = 1.57  # 90도
        elif self.step < 12:  # 전진
            msg.linear.x = 0.5
        elif self.step < 14:  # 회전
            msg.angular.z = 1.57
        else:
            self.step = 0  # 리셋
            
        self.publisher.publish(msg)
        self.step += 1

def main():
    rclpy.init()
    controller = RobotController()
    rclpy.spin(controller)
    controller.destroy_node()
    rclpy.shutdown()
```

---

## 🔧 고급 시뮬레이션 기능

### 물리 엔진 설정
```xml
<world name="default">
  <physics name="default_physics" default="0" type="ode">
    <gravity>0 0 -9.8066</gravity>
    <ode>
      <solver>
        <type>quick</type>
        <iters>150</iters>
        <sor>1.4</sor>
        <use_dynamic_moi_rescaling>0</use_dynamic_moi_rescaling>
      </solver>
      <constraints>
        <cfm>0.00001</cfm>
        <erp>0.2</erp>
        <contact_max_correcting_vel>2000.0</contact_max_correcting_vel>
        <contact_surface_layer>0.01</contact_surface_layer>
      </constraints>
    </ode>
    <real_time_update_rate>1000.0</real_time_update_rate>
    <real_time_factor>1</real_time_factor>
    <max_step_size>0.001</max_step_size>
  </physics>
</world>
```

### 동적 객체 생성
```python
import rclpy
from gazebo_msgs.srv import SpawnEntity
from geometry_msgs.msg import Pose

class ObjectSpawner(Node):
    def __init__(self):
        super().__init__('object_spawner')
        self.spawn_client = self.create_client(SpawnEntity, '/spawn_entity')
        
    def spawn_box(self, name, x, y, z):
        request = SpawnEntity.Request()
        request.name = name
        request.xml = f"""
        <sdf version="1.5">
          <model name="{name}">
            <link name="link">
              <visual name="visual">
                <geometry>
                  <box><size>0.5 0.5 0.5</size></box>
                </geometry>
              </visual>
              <collision name="collision">
                <geometry>
                  <box><size>0.5 0.5 0.5</size></box>
                </geometry>
              </collision>
            </link>
          </model>
        </sdf>
        """
        
        pose = Pose()
        pose.position.x = x
        pose.position.y = y
        pose.position.z = z
        request.initial_pose = pose
        
        future = self.spawn_client.call_async(request)
        return future
```

---

## 🧪 테스트 및 디버깅

### Gazebo GUI 도구
- **Entity Tree**: 모델 계층 구조 확인
- **Topic Visualization**: Topic 데이터 모니터링
- **Plot**: 실시간 데이터 그래프
- **View Angle**: 다양한 시점에서 관찰

### RViz2 통합
```bash
# RViz2 실행 (로봇 모델 및 센서 데이터 시각화)
rviz2 -d robot_config.rviz
```

### 로그 분석
```bash
# Gazebo 로그 확인
gazebo --verbose

# ROS2 로그 확인
ros2 launch robot_simulation simulation.launch.py \
  --ros-args --log-level debug
```

---

## 📊 성능 최적화

### 실시간 성능
```xml
<!-- 실시간 팩터 조정 -->
<physics>
  <real_time_factor>1.0</real_time_factor>  <!-- 실시간 속도 -->
  <real_time_update_rate>1000</real_time_rate>  <!-- 업데이트 주기 -->
</physics>
```

### GPU 가속
```bash
# NVIDIA GPU 가속 활성화
export GAZEBO_GPU=1

# GUI 없이 헤드리스 실행 (성능 향상)
gzserver --verbose world.sdf
```

---

## 📖 주요 참고문헌

1. **Gazebo Tutorials**: https://gazebosim.org/docs/latest/ros2_integration/
2. **ROS2 Gazebo Integration**: https://docs.ros.org/en/humble/Tutorials/Advanced/Simulators/Gazebo/Gazebo.html
3. **gazebo_ros_pkgs**: https://github.com/ros-simulation/gazebo_ros_pkgs
4. **Robot Simulation Tutorial**: https://automaticaddison.com/how-to-simulate-a-robot-using-gazebo-and-ros-2/
5. **SDF Format**: http://sdformat.org/