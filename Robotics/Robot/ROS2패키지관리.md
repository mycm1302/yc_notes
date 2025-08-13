# ROS2패키지관리

> 상위: [[ROS2]]

Dependencies, ament, rosdep을 통한 효율적인 패키지 의존성 및 배포 관리를 다룹니다.

---

## 📦 패키지 구조

### 표준 패키지 레이아웃
```
my_package/
├── CMakeLists.txt     # C++ 빌드 설정
├── package.xml        # 패키지 메타데이터
├── src/              # C++ 소스 코드
├── include/          # C++ 헤더 파일
├── launch/           # Launch 파일
├── config/           # 설정 파일
├── msg/              # 메시지 정의
├── srv/              # 서비스 정의
└── action/           # 액션 정의
```

### package.xml 구성
```xml
<?xml version="1.0"?>
<package format="3">
  <name>my_package</name>
  <version>1.0.0</version>
  <description>패키지 설명</description>
  <maintainer email="user@domain.com">Name</maintainer>
  <license>MIT</license>
  
  <buildtool_depend>ament_cmake</buildtool_depend>
  <depend>rclcpp</depend>
  <depend>std_msgs</depend>
  <test_depend>ament_lint_auto</test_depend>
</package>
```

---

## 🔧 의존성 관리

### 의존성 타입
- **buildtool_depend**: 빌드 도구 (ament_cmake, colcon)
- **build_depend**: 빌드 시점 의존성
- **exec_depend**: 실행 시점 의존성
- **depend**: 빌드 + 실행 의존성
- **test_depend**: 테스트 의존성

### rosdep 활용
```bash
# 시스템 의존성 설치
rosdep install --from-paths src --ignore-src -r -y

# 의존성 업데이트
rosdep update

# 의존성 확인
rosdep check --from-paths src --ignore-src
```

---

## 🏗️ ament 빌드 시스템

### ament_cmake 주요 매크로
```cmake
# 패키지 선언
ament_package()

# 의존성 추가
ament_target_dependencies(target dependency1 dependency2)

# 내보내기 설정
ament_export_dependencies(dependency1)
ament_export_include_directories(include)
ament_export_libraries(my_library)
```

### Python 패키지 설정
```python
# setup.py
from setuptools import setup

setup(
    name='my_python_package',
    version='1.0.0',
    packages=['my_python_package'],
    install_requires=['setuptools'],
    zip_safe=True,
    entry_points={
        'console_scripts': [
            'my_node = my_python_package.my_node:main',
        ],
    },
)
```

---

## 📋 버전 관리

### 시맨틱 버전
- **MAJOR.MINOR.PATCH** (예: 1.2.3)
- **API 호환성**: Major 버전에서 변경
- **기능 추가**: Minor 버전에서 추가
- **버그 수정**: Patch 버전에서 수정

### 릴리즈 관리
```bash
# 태그 생성
git tag -a v1.0.0 -m "Release version 1.0.0"

# 변경사항 문서화
git log --oneline --since="2023-01-01"
```

---

## 🔍 패키지 발견

### 패키지 검색
```bash
# 설치된 패키지 목록
ros2 pkg list

# 패키지 정보 확인
ros2 pkg xml my_package

# 실행 파일 목록
ros2 pkg executables my_package
```

### 환경 변수
```bash
# 패키지 경로 확인
echo $AMENT_PREFIX_PATH

# 패키지 경로 추가
export AMENT_PREFIX_PATH=$AMENT_PREFIX_PATH:/custom/path
```

---

## 📤 패키지 배포

### 바이너리 배포
```bash
# 바이너리 빌드
colcon build --install-base /opt/ros/humble

# 바이너리 패키징
cpack -G DEB
```

### 소스 배포
```bash
# 소스 아카이브 생성
git archive --format=tar.gz --prefix=my_package-1.0.0/ HEAD > my_package-1.0.0.tar.gz
```

### 저장소 배포
- **공식 저장소**: ROS Index 등록
- **개인 저장소**: GitHub Releases
- **내부 저장소**: 회사 내부 패키지 서버

---

## 🛠️ 개발 워크플로우

### 개발 사이클
1. **패키지 생성**: `ros2 pkg create`
2. **개발**: 코드 작성 및 테스트
3. **빌드**: `colcon build`
4. **테스트**: `colcon test`
5. **배포**: 버전 태그 및 릴리즈

### 지속적 통합
```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Install dependencies
        run: rosdep install --from-paths . --ignore-src -r -y
      - name: Build
        run: colcon build
      - name: Test
        run: colcon test
```

---

## 📖 주요 참고문헌

1. **Package Creation**: https://docs.ros.org/en/humble/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html
2. **rosdep Guide**: https://docs.ros.org/en/humble/Tutorials/Intermediate/Rosdep.html
3. **ament Build System**: https://design.ros2.org/articles/ament.html
4. **Distribution Guidelines**: ROS2 패키지 배포 가이드라인