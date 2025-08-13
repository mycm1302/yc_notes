# ROS2런치시스템

> 상위: [[ROS2]]

Launch files와 parameters를 통한 복잡한 시스템의 체계적인 실행 관리를 다룹니다.

---

## 🚀 Launch 시스템 개요

### 핵심 기능
- **다중 노드 실행**: 여러 노드 동시 시작
- **파라미터 설정**: 실행 시 파라미터 전달
- **조건부 실행**: 조건에 따른 노드 시작
- **네임스페이스**: 논리적 그룹화

### Launch 파일 형식
- **Python**: `.launch.py` (권장)
- **XML**: `.launch.xml`
- **YAML**: `.launch.yaml`

---

## 🐍 Python Launch 파일

### 기본 구조
```python
from launch import LaunchDescription
from launch_ros.actions import Node

def generate_launch_description():
    return LaunchDescription([
        Node(
            package='my_package',
            executable='my_node',
            name='my_node_name',
            namespace='my_namespace',
            parameters=[{'param1': 'value1'}],
            remappings=[('input_topic', 'remapped_topic')],
            output='screen'
        )
    ])
```

### 고급 기능
```python
from launch.actions import DeclareLaunchArgument, IncludeLaunchDescription
from launch.conditions import IfCondition
from launch.substitutions import LaunchConfiguration

def generate_launch_description():
    # Launch argument 선언
    declare_use_sim_time = DeclareLaunchArgument(
        'use_sim_time', default_value='false')
    
    # 조건부 노드
    robot_node = Node(
        package='robot_package',
        executable='robot_node',
        condition=IfCondition(LaunchConfiguration('use_sim_time'))
    )
    
    return LaunchDescription([
        declare_use_sim_time,
        robot_node
    ])
```

---

## 📝 XML/YAML Launch 파일

### XML 형식
```xml
<launch>
  <arg name="use_sim_time" default="false"/>
  
  <node pkg="my_package" exec="my_node" name="my_node">
    <param name="use_sim_time" value="$(var use_sim_time)"/>
    <remap from="input" to="camera/image"/>
  </node>
  
  <include file="$(find-pkg-share another_package)/launch/sub.launch.py">
    <arg name="param_name" value="param_value"/>
  </include>
</launch>
```

### YAML 형식
```yaml
launch:
  - arg:
      name: "use_sim_time"
      default: "false"
  
  - node:
      pkg: "my_package"
      exec: "my_node"
      name: "my_node"
      param:
        - name: "use_sim_time"
          value: "$(var use_sim_time)"
```

---

## ⚙️ 파라미터 관리

### 파라미터 파일
```yaml
# config/robot_params.yaml
my_node:
  ros__parameters:
    update_rate: 10.0
    max_velocity: 1.0
    topics:
      input: "/camera/image"
      output: "/processed_image"
```

### Launch에서 파라미터 로딩
```python
Node(
    package='my_package',
    executable='my_node',
    parameters=[
        {'param1': 'value1'},  # 직접 설정
        os.path.join(get_package_share_directory('my_package'), 
                    'config', 'params.yaml')  # 파일에서 로딩
    ]
)
```

---

## 🔧 Launch Substitutions

### 주요 Substitutions
- **LaunchConfiguration**: Launch argument 값
- **PathJoinSubstitution**: 경로 조합
- **FindPackageShare**: 패키지 공유 디렉토리
- **EnvironmentVariable**: 환경 변수 값

### 활용 예제
```python
from launch_ros.substitutions import FindPackageShare

config_file = PathJoinSubstitution([
    FindPackageShare('my_package'),
    'config',
    'robot_config.yaml'
])

Node(
    package='my_package',
    executable='my_node',
    parameters=[config_file]
)
```

---

## 🎯 Launch 테스트

### 테스트 작성
```python
import pytest
from launch import LaunchDescription
from launch_testing.actions import ReadyToTest

@pytest.mark.launch_test
def generate_test_description():
    return LaunchDescription([
        Node(package='my_package', executable='my_node'),
        ReadyToTest()
    ])

class TestMyNode:
    def test_node_startup(self, proc_info, proc_output):
        # 노드 시작 테스트
        proc_info.assertWaitForStartup(timeout=5)
```

### Launch 테스트 실행
```bash
# 테스트 실행
colcon test --packages-select my_package

# 특정 테스트만 실행
python3 -m pytest test_my_launch.py
```

---

## 🛠️ 실용적 패턴

### 로봇 시스템 Launch
```python
def generate_launch_description():
    # 하드웨어 드라이버
    hardware_launch = IncludeLaunchDescription(
        PythonLaunchDescriptionSource([
            FindPackageShare('robot_hardware'),
            '/launch/hardware.launch.py'
        ])
    )
    
    # 내비게이션 스택
    nav_launch = IncludeLaunchDescription(
        PythonLaunchDescriptionSource([
            FindPackageShare('nav2_bringup'),
            '/launch/navigation_launch.py'
        ]),
        launch_arguments={'use_sim_time': 'true'}.items()
    )
    
    return LaunchDescription([
        hardware_launch,
        nav_launch
    ])
```

### 개발/배포 모드 분리
```python
# development.launch.py
def generate_launch_description():
    return LaunchDescription([
        DeclareLaunchArgument('log_level', default_value='debug'),
        Node(
            package='my_package',
            executable='my_node',
            arguments=['--ros-args', '--log-level', 
                      LaunchConfiguration('log_level')]
        )
    ])
```

---

## 📋 Launch 명령어

### 실행 명령어
```bash
# Launch 파일 실행
ros2 launch my_package my_launch.launch.py

# 인수와 함께 실행
ros2 launch my_package my_launch.launch.py use_sim_time:=true

# 사용 가능한 Launch 파일 목록
ros2 launch my_package
```

### 디버깅
```bash
# Launch 파일 구문 검사
ros2 launch --check my_package my_launch.launch.py

# 실행 계획 출력
ros2 launch --show-args my_package my_launch.launch.py
```

---

## 📖 주요 참고문헌

1. **ROS2 Launch**: https://docs.ros.org/en/humble/Tutorials/Intermediate/Launch/Launch-Main.html
2. **Launch Testing**: https://github.com/ros2/launch/tree/rolling/launch_testing
3. **Best Practices**: Launch 파일 작성 모범 사례
4. **Migration Guide**: ROS1 Launch에서 ROS2로 마이그레이션