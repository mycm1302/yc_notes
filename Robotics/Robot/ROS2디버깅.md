# ROS2디버깅

> 상위: [[ROS2]]

Logging, rqt, debugging tools를 활용한 효과적인 ROS2 시스템 디버깅 기법을 다룹니다.

---

## 🔍 디버깅 도구 개요

### 핵심 디버깅 도구
- **rqt**: GUI 기반 디버깅 도구 모음
- **ros2 cli**: 명령줄 디버깅 도구
- **rviz2**: 3D 데이터 시각화
- **rosbag2**: 데이터 기록 및 재생

### 로그 시스템
- **rcutils**: 기본 로깅 라이브러리
- **spdlog**: 고성능 로깅 백엔드
- **console output**: 표준 출력
- **file logging**: 파일 로깅

---

## 📝 로깅 시스템

### 로그 레벨
```cpp
// C++ 로깅
RCLCPP_DEBUG(get_logger(), "Debug: %d", value);      // 상세 정보
RCLCPP_INFO(get_logger(), "Info: System started");   // 일반 정보
RCLCPP_WARN(get_logger(), "Warning: Low battery");   // 경고
RCLCPP_ERROR(get_logger(), "Error: Connection lost"); // 오류
RCLCPP_FATAL(get_logger(), "Fatal: System failure");  // 치명적 오류
```

```python
# Python 로깅
self.get_logger().debug('Debug message')
self.get_logger().info('Info message')
self.get_logger().warn('Warning message')
self.get_logger().error('Error message')
```

### 로그 설정
```bash
# 환경 변수로 로그 레벨 설정
export RCUTILS_LOGGING_SEVERITY=DEBUG

# 노드별 로그 레벨 설정
ros2 run my_package my_node --ros-args --log-level DEBUG

# 특정 로거만 설정
ros2 run my_package my_node --ros-args --log-level my_node:=DEBUG
```

---

## 🖥️ rqt 디버깅 도구

### 주요 rqt 플러그인
```bash
# rqt 전체 실행
rqt

# 개별 플러그인 실행
rqt_graph          # 노드 그래프 시각화
rqt_console        # 로그 메시지 뷰어
rqt_topic          # 토픽 모니터링
rqt_service_caller # 서비스 호출 테스트
rqt_param          # 파라미터 편집
rqt_plot           # 데이터 플롯
```

### rqt_graph 활용
- **노드 연결 상태**: 시각적 확인
- **토픽 흐름**: 데이터 흐름 추적
- **네임스페이스**: 계층 구조 확인
- **동적 업데이트**: 실시간 변경 반영

---

## 📊 데이터 모니터링

### 토픽 모니터링
```bash
# 토픽 목록 확인
ros2 topic list

# 토픽 데이터 확인
ros2 topic echo /my_topic

# 토픽 정보 확인
ros2 topic info /my_topic

# 토픽 주파수 측정
ros2 topic hz /my_topic

# 토픽 대역폭 측정
ros2 topic bw /my_topic
```

### 서비스 디버깅
```bash
# 서비스 목록
ros2 service list

# 서비스 타입 확인
ros2 service type /my_service

# 서비스 호출 테스트
ros2 service call /my_service my_interfaces/srv/MyService "{request_data: 'test'}"
```

---

## 🎬 rosbag2 활용

### 데이터 기록
```bash
# 모든 토픽 기록
ros2 bag record -a

# 특정 토픽만 기록
ros2 bag record /topic1 /topic2

# 압축 기록
ros2 bag record -a --compression-mode file --compression-format zstd
```

### 데이터 재생
```bash
# 기록 재생
ros2 bag play my_bag

# 속도 조절
ros2 bag play my_bag --rate 2.0

# 루프 재생
ros2 bag play my_bag --loop
```

### 데이터 분석
```bash
# bag 정보 확인
ros2 bag info my_bag

# 토픽별 메시지 수
ros2 bag info my_bag --verbose
```

---

## 🐛 코드 레벨 디버깅

### GDB 디버깅 (C++)
```bash
# 디버그 빌드
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Debug

# GDB로 실행
gdb --args ros2 run my_package my_node

# GDB 명령어
(gdb) break main
(gdb) run
(gdb) continue
(gdb) print variable_name
(gdb) backtrace
```

### Python 디버깅
```python
import pdb

class MyNode(Node):
    def callback(self, msg):
        pdb.set_trace()  # 브레이크포인트 설정
        # 디버깅할 코드
        self.process_message(msg)
```

### VS Code 디버깅 설정
```json
// .vscode/launch.json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "ROS2 Debug",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/install/my_package/lib/my_package/my_node",
            "args": [],
            "cwd": "${workspaceFolder}",
            "environment": [
                {"name": "ROS_DOMAIN_ID", "value": "0"}
            ]
        }
    ]
}
```

---

## 🔧 진단 도구

### ros2 doctor
```bash
# 시스템 전체 진단
ros2 doctor

# 상세 보고서 생성
ros2 doctor --report

# 특정 항목만 확인
ros2 doctor --include-warnings
```

### 네트워크 진단
```bash
# 멀티캐스트 테스트
ros2 multicast receive
ros2 multicast send

# DDS 통신 확인
ros2 daemon status
ros2 daemon stop
ros2 daemon start
```

---

## 🚨 일반적인 문제 해결

### 통신 문제
```bash
# 도메인 ID 확인
echo $ROS_DOMAIN_ID

# 방화벽 확인 (Ubuntu)
sudo ufw status

# 네트워크 인터페이스 확인
ip addr show
```

### 성능 문제
```bash
# CPU 사용률 확인
htop

# 메모리 사용량 확인
free -h

# 디스크 I/O 확인
iotop
```

### 의존성 문제
```bash
# 의존성 확인
rosdep check --from-paths src --ignore-src

# 의존성 설치
rosdep install --from-paths src --ignore-src -r -y
```

---

## 📋 디버깅 체크리스트

### 기본 확인사항
- ✅ 노드가 실행 중인가?
- ✅ 토픽이 발행되고 있는가?
- ✅ 메시지 타입이 올바른가?
- ✅ QoS 설정이 호환되는가?

### 네트워크 확인사항
- ✅ 도메인 ID가 일치하는가?
- ✅ 방화벽이 차단하고 있는가?
- ✅ 네트워크 연결이 정상인가?
- ✅ 멀티캐스트가 작동하는가?

### 성능 확인사항
- ✅ CPU 사용률이 정상인가?
- ✅ 메모리 누수가 있는가?
- ✅ 디스크 I/O가 과도한가?
- ✅ 네트워크 대역폭이 충분한가?

---

## 📖 주요 참고문헌

1. **ROS2 Troubleshooting**: https://docs.ros.org/en/humble/How-To-Guides/Troubleshooting.html
2. **rqt User Guide**: https://docs.ros.org/en/humble/Concepts/About-RQt.html
3. **rosbag2 Guide**: https://docs.ros.org/en/humble/Tutorials/Beginner-CLI-Tools/Recording-And-Playing-Back-Data/Recording-And-Playing-Back-Data.html
4. **Debugging Best Practices**: ROS2 디버깅 모범 사례