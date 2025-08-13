# ROS2개발환경

> 상위: [[ROS2]]

Build system, colcon, IDE 설정을 통한 효율적인 ROS2 개발 환경 구축을 다룹니다.

---

## 🏗️ 빌드 시스템

### ament 빌드 시스템
- **ament_cmake**: CMake 기반 빌드
- **ament_python**: Python 패키지 빌드
- **ament_package**: 패키지 메타데이터 관리
- **ament_tools**: 빌드 도구 모음

### colcon 빌드 도구
- **병렬 빌드**: 다중 패키지 동시 빌드
- **의존성 해결**: 자동 빌드 순서 결정
- **증분 빌드**: 변경된 부분만 재빌드
- **테스트 통합**: 빌드와 테스트 연동

### 기본 명령어
```bash
# 워크스페이스 빌드
colcon build

# 특정 패키지만 빌드
colcon build --packages-select my_package

# 병렬 빌드 (4개 작업)
colcon build --parallel-workers 4

# 디버그 빌드
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Debug
```

---

## 📁 워크스페이스 구조

### 표준 워크스페이스 레이아웃
```
workspace/
├── src/              # 소스 코드
│   ├── package1/
│   └── package2/
├── build/            # 빌드 파일 (임시)
├── install/          # 설치된 파일
└── log/              # 빌드 로그
```

### 환경 설정
```bash
# 워크스페이스 소싱
source install/setup.bash

# 자동 소싱 설정 (.bashrc)
echo "source ~/ros2_ws/install/setup.bash" >> ~/.bashrc

# 언더레이 설정
source /opt/ros/humble/setup.bash
```

---

## 🛠️ IDE 통합

### Visual Studio Code
- **ROS Extension**: 공식 ROS 확장
- **C++ Extension**: IntelliSense 지원
- **Python Extension**: Python 디버깅
- **CMake Tools**: CMake 통합

### VS Code 설정
```json
// .vscode/settings.json
{
  "python.defaultInterpreterPath": "/usr/bin/python3",
  "ros.distro": "humble",
  "cmake.configureOnOpen": true,
  "cmake.buildDirectory": "${workspaceFolder}/build"
}
```

### CLion (JetBrains)
- **CMake 프로젝트**: 네이티브 CMake 지원
- **디버깅**: GDB 통합 디버깅
- **코드 분석**: 정적 분석 도구
- **Git 통합**: 버전 관리 도구

---

## 🔧 개발 도구

### 디버깅 도구
- **gdb**: GNU 디버거
- **valgrind**: 메모리 분석
- **rr**: 실행 기록 및 재생
- **strace**: 시스템 콜 추적

### 프로파일링
- **perf**: 성능 프로파일링
- **gperftools**: Google 성능 도구
- **htop**: 시스템 모니터링
- **iotop**: I/O 모니터링

### 코드 품질
```bash
# cpplint 사용
cpplint --recursive src/

# clang-format 사용
clang-format -i src/**/*.cpp

# static analysis
cppcheck --enable=all src/
```

---

## 📦 패키지 개발

### 패키지 생성
```bash
# C++ 패키지 생성
ros2 pkg create --build-type ament_cmake my_cpp_package

# Python 패키지 생성
ros2 pkg create --build-type ament_python my_python_package

# 혼합 패키지 생성
ros2 pkg create --build-type ament_cmake my_mixed_package \
  --dependencies rclcpp rclpy std_msgs
```

### CMakeLists.txt 구성
```cmake
cmake_minimum_required(VERSION 3.8)
project(my_package)

# 의존성 찾기
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)

# 실행파일 추가
add_executable(my_node src/my_node.cpp)
ament_target_dependencies(my_node rclcpp std_msgs)

# 설치 설정
install(TARGETS my_node DESTINATION lib/${PROJECT_NAME})

ament_package()
```

---

## 🧪 테스트 환경

### 테스트 프레임워크
- **gtest**: C++ 단위 테스트
- **pytest**: Python 단위 테스트
- **launch_testing**: 통합 테스트
- **ament_lint**: 코드 품질 검사

### 테스트 실행
```bash
# 모든 테스트 실행
colcon test

# 특정 패키지 테스트
colcon test --packages-select my_package

# 테스트 결과 확인
colcon test-result --verbose
```

### 지속적 통합 (CI)
```yaml
# GitHub Actions 예제
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Setup ROS2
        uses: ros-tooling/setup-ros@v0.3
        with:
          required-ros-distributions: humble
      - name: Build and Test
        run: |
          colcon build
          colcon test
```

---

## 🔍 문서화

### API 문서 생성
- **Doxygen**: C++ API 문서
- **Sphinx**: Python 문서
- **rosdoc2**: ROS2 전용 문서 도구
- **GitHub Pages**: 온라인 호스팅

### README 작성
```markdown
# My ROS2 Package

## Installation
```bash
git clone https://github.com/user/my_package.git
cd ros2_ws/src
colcon build
```

## Usage
```bash
ros2 run my_package my_node
```
```

---

## 🐳 컨테이너 개발

### Docker 활용
```dockerfile
FROM ros:humble
RUN apt-get update && apt-get install -y \
    python3-colcon-common-extensions \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /workspace
COPY . .
RUN . /opt/ros/humble/setup.sh && colcon build

CMD ["bash"]
```

### 개발 컨테이너
- **VS Code Dev Containers**: 개발 환경 표준화
- **Docker Compose**: 멀티 컨테이너 개발
- **Remote Development**: 원격 개발 지원
- **GPU 지원**: NVIDIA Docker 활용

---

## 📖 주요 참고문헌

1. **ROS2 Developer Guide**: https://docs.ros.org/en/humble/The-ROS2-Project/Contributing/Developer-Guide.html
2. **colcon Documentation**: https://colcon.readthedocs.io/
3. **ament Build System**: ROS2 빌드 시스템 가이드
4. **Best Practices**: ROS2 개발 모범 사례