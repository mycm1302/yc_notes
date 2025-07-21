# Docker 개발 환경 템플릿

이 저장소는 C++와 Python 개발을 위한 Docker 기반 개발 환경 템플릿을 제공합니다. RBDL, Eigen, qpOASES 등의 C++ 라이브러리와 matplotlib, numpy 등의 Python 패키지가 사전 설치된 환경을 쉽게 구성할 수 있습니다.

## 🚀 기능

- 크로스 플랫폼 호환성 (Windows, macOS, Linux)
- VSCode와의 통합
- 사전 설치된 라이브러리 및 패키지:
  - **C++**: RBDL, Eigen, qpOASES, Boost
  - **Python**: matplotlib, numpy, scipy, pandas
- X11 포워딩으로 그래픽 애플리케이션 지원

## 📋 필요 사항

- [Docker](https://www.docker.com/products/docker-desktop) 설치됨
- [Visual Studio Code](https://code.visualstudio.com/) 설치됨
- VSCode 확장 프로그램:
  - [Dev Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
  - [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)

## 🛠️ 설치 및 사용 방법

### 1. 이 템플릿을 프로젝트에 적용하기

#### 새 프로젝트인 경우:
1. 이 저장소를 클론하거나 다운로드
2. 프로젝트 파일을 추가

#### 기존 프로젝트인 경우:
1. 이 저장소에서 다음 파일을 프로젝트의 루트 디렉토리에 복사:
   - `Dockerfile`
   - `requirements.txt`
   - `.devcontainer/` 폴더와 하위 파일들

### 2. VSCode에서 프로젝트 열기

1. VSCode를 실행하고 파일 > 폴더 열기에서 프로젝트 폴더 선택
2. 왼쪽 하단의 "><" 아이콘을 클릭
3. "Reopen in Container" 선택
4. Docker 환경이 구축되고 VSCode가 컨테이너 내부에서 재시작됨

### 3. 개발 시작

- 이제 모든 필요한 라이브러리와 도구가 설치된 환경에서 개발할 수 있습니다
- 터미널은 컨테이너 내부에서 실행됩니다
- 파일 변경은 자동으로 호스트 시스템에 반영됩니다

## 📁 파일 구조

```
project-root/
├── Dockerfile         # Docker 이미지 정의
├── requirements.txt   # Python 패키지 목록
├── .devcontainer/     # VSCode Dev Container 설정
│   └── devcontainer.json
└── (프로젝트 파일들)
```

## 📝 파일 설명

### Dockerfile

```dockerfile
FROM ubuntu:20.04

# 상호작용 없이 설치 진행
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Seoul

# 기본 도구 및 의존성 설치
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    python3 \
    python3-pip \
    python3-dev \
    python3-tk \
    libx11-dev \
    libxext-dev \
    libxrender-dev \
    libxtst-dev \
    wget \
    software-properties-common \
    pkg-config \
    libgl1-mesa-dev \
    libeigen3-dev \
    libboost-all-dev

# C++ 컴파일러 최신 버전 설치
RUN add-apt-repository -y ppa:ubuntu-toolchain-r/test && \
    apt-get update && \
    apt-get install -y gcc-10 g++-10 && \
    update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-10 100 && \
    update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-10 100

# RBDL 설치
RUN git clone https://github.com/rbdl/rbdl.git && \
    cd rbdl && \
    mkdir build && \
    cd build && \
    cmake -DRBDL_BUILD_PYTHON=ON -DRBDL_BUILD_ADDON_URDFREADER=ON .. && \
    make -j$(nproc) && \
    make install && \
    ldconfig

# qpOASES 설치
RUN git clone https://github.com/coin-or/qpOASES.git && \
    cd qpOASES && \
    mkdir build && \
    cd build && \
    cmake .. && \
    make -j$(nproc) && \
    make install && \
    ldconfig

# Python 패키지 설치
COPY requirements.txt /tmp/
RUN pip3 install --upgrade pip && \
    pip3 install -r /tmp/requirements.txt

# 환경 변수 설정
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
ENV PYTHONPATH=$PYTHONPATH:/usr/local/lib/python3.8/site-packages

# 작업 디렉토리 설정
WORKDIR /workspace

CMD ["/bin/bash"]
```

### requirements.txt

```
numpy>=1.20.0
matplotlib>=3.4.0
scipy>=1.6.0
pandas>=1.2.0
```

### .devcontainer/devcontainer.json

```json
{
    "name": "CPP Python Development",
    "build": {
        "dockerfile": "../Dockerfile",
        "context": ".."
    },
    "settings": {
        "terminal.integrated.defaultProfile.linux": "bash",
        "editor.formatOnSave": true,
        "C_Cpp.default.includePath": [
            "/usr/local/include",
            "/usr/include/eigen3",
            "/usr/local/include/rbdl"
        ],
        "python.defaultInterpreterPath": "/usr/bin/python3",
        "python.linting.enabled": true,
        "python.linting.pylintEnabled": true
    },
    "extensions": [
        "ms-vscode.cpptools",
        "ms-python.python",
        "ms-vscode.cmake-tools",
        "twxs.cmake"
    ],
    "forwardPorts": [],
    "runArgs": [
        "--cap-add=SYS_PTRACE",
        "--security-opt", "seccomp=unconfined",
        "-e", "DISPLAY=${env:DISPLAY}",
        "-v", "/tmp/.X11-unix:/tmp/.X11-unix"
    ],
    "mounts": [
        "source=${localWorkspaceFolder},target=/workspace,type=bind,consistency=cached"
    ],
    "workspaceFolder": "/workspace",
    "remoteUser": "root"
}
```

## 🖥️ 그래픽 애플리케이션 설정 (GUI)

### Linux
```bash
xhost +local:docker
```

### Windows
1. [VcXsrv](https://sourceforge.net/projects/vcxsrv/) 같은 X Server 설치
2. X Server 시작 (액세스 제어 비활성화)
3. Docker Desktop 설정에서 필요한 경우 추가 설정

### macOS
1. [XQuartz](https://www.xquartz.org/) 설치
2. XQuartz 실행 후 환경설정 > 보안 > "네트워크 클라이언트로부터의 연결 허용" 활성화
3. 터미널에서 `xhost + 127.0.0.1` 실행

## 📚 C++ 라이브러리 사용 예시

```cpp
#include <iostream>
#include <rbdl/rbdl.h>
#include <Eigen/Dense>
#include <qpOASES.hpp>

int main() {
    // Eigen 예시
    Eigen::MatrixXd m = Eigen::MatrixXd::Random(3, 3);
    std::cout << "Random matrix: \n" << m << std::endl;
    
    // 기타 코드...
    return 0;
}
```

## 🐍 Python 모듈 사용 예시

```python
import numpy as np
import matplotlib.pyplot as plt

# 데이터 생성
x = np.linspace(0, 10, 100)
y = np.sin(x)

# 그래프 그리기
plt.figure(figsize=(10, 6))
plt.plot(x, y)
plt.title('Sine Wave')
plt.xlabel('X')
plt.ylabel('Y')
plt.grid(True)
plt.show()
```

## 🔧 CMake 프로젝트 예시 (선택 사항)

`CMakeLists.txt` 파일:

```cmake
cmake_minimum_required(VERSION 3.10)
project(YourProjectName)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra")

find_package(Eigen3 REQUIRED)
find_package(RBDL REQUIRED)
find_package(qpOASES REQUIRED)

include_directories(
    ${EIGEN3_INCLUDE_DIR}
    ${RBDL_INCLUDE_DIR}
    ${QPOASES_INCLUDE_DIR}
)

# 소스 파일 추가
file(GLOB SOURCES "src/*.cpp")

# 실행 파일 생성
add_executable(${PROJECT_NAME} ${SOURCES})

target_link_libraries(${PROJECT_NAME}
    ${RBDL_LIBRARY}
    ${QPOASES_LIBRARIES}
)
```

## ⚙️ 커스터마이징

프로젝트 요구사항에 맞게 `Dockerfile`과 `requirements.txt`를 수정할 수 있습니다:

- 다른 C++ 라이브러리 추가: Dockerfile에 해당 라이브러리 설치 명령 추가
- 다른 Python 패키지 추가: requirements.txt에 패키지 추가
- 다른 VSCode 확장 프로그램: devcontainer.json의 "extensions" 섹션 수정

## 🔍 문제해결

1. **Docker 컨테이너가 시작되지 않는 경우**
   - Docker 서비스가 실행 중인지 확인
   - VSCode를 관리자 권한으로 실행해 보기

2. **라이브러리가 찾아지지 않는 경우**
   - 컨테이너 내 터미널에서 라이브러리 경로 확인 (`ldconfig -p | grep 라이브러리이름`)
   - 환경 변수 확인 (`echo $LD_LIBRARY_PATH`)

3. **X11 디스플레이 작동하지 않는 경우**
   - X Server가 실행 중인지 확인
   - 호스트에서 X11 액세스 허용 명령 실행 확인

## 🤝 기여

이 템플릿에 기여하려면 Pull Request를 제출해 주세요. 새로운 기능, 버그 수정, 문서 개선 등 모든 형태의 기여를 환영합니다.

## 📄 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다.