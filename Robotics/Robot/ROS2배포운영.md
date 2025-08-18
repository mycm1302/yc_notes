# ROS2배포운영

> 상위: [[ROS2]]

Docker 컨테이너화, 자동 배포, Fleet 관리를 통한 ROS2 시스템의 프로덕션 운영을 다룹니다.

---

## 🚀 배포운영 개요

### 핵심 요소
- **컨테이너화**: Docker 기반 환경 표준화
- **자동 배포**: CI/CD 파이프라인 구축
- **Fleet 관리**: 다중 로봇 원격 관리
- **모니터링**: 실시간 상태 감시

### 배포 아키텍처
```
Development Environment
    ↓ CI/CD Pipeline
Container Registry (Docker Hub/ECR)
    ↓ Deployment
Production Fleet
    ↓ Monitoring
Central Management Dashboard
```

---

## 🐳 Docker 컨테이너화

### 기본 Dockerfile
```dockerfile
# 멀티스테이지 빌드를 위한 베이스 이미지
FROM ros:humble-ros-base as base

# 환경 변수 설정
ENV ROS_DISTRO=humble
ENV AMENT_PREFIX_PATH=/opt/ros/humble
ENV COLCON_PREFIX_PATH=/opt/ros/humble
SHELL ["/bin/bash", "-c"]

# 필수 패키지 설치
RUN apt-get update && apt-get install -y \
    python3-pip \
    python3-colcon-common-extensions \
    ros-humble-rmw-cyclonedx-cpp \
    && rm -rf /var/lib/apt/lists/*

# 의존성 단계
FROM base as dependencies
COPY package.xml /workspace/src/robot_package/package.xml
WORKDIR /workspace
RUN source /opt/ros/$ROS_DISTRO/setup.bash && \
    rosdep install --from-paths src --ignore-src -r -y

# 빌드 단계  
FROM dependencies as build
COPY . /workspace/src/robot_package/
RUN source /opt/ros/$ROS_DISTRO/setup.bash && \
    colcon build --packages-select robot_package

# 프로덕션 단계
FROM base as production
COPY --from=build /workspace/install /opt/overlay
RUN echo "source /opt/overlay/setup.bash" >> ~/.bashrc

# 개발 단계 (선택적)
FROM build as dev
RUN apt-get update && apt-get install -y \
    gdb \
    vim \
    git \
    && rm -rf /var/lib/apt/lists/*

# 사용자 설정 (보안)
ARG USERNAME=ros
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME
WORKDIR /home/$USERNAME

# 실행 명령
ENTRYPOINT ["/bin/bash", "-c"]
CMD ["source /opt/overlay/setup.bash && ros2 launch robot_package robot.launch.py"]
```

### Docker Compose 구성
```yaml
version: '3.8'

services:
  # 메인 로봇 서비스
  robot_main:
    build:
      context: .
      target: production
    container_name: robot_main
    network_mode: host
    ipc: host
    privileged: true
    environment:
      - ROS_DOMAIN_ID=0
      - RMW_IMPLEMENTATION=rmw_cyclonedx_cpp
    volumes:
      - /dev:/dev  # 하드웨어 액세스
      - robot_logs:/var/log/robot
    restart: unless-stopped
    command: ["ros2 launch robot_package main.launch.py"]

  # 내비게이션 서비스
  navigation:
    build:
      context: .
      target: production
    container_name: robot_navigation
    network_mode: host
    environment:
      - ROS_DOMAIN_ID=0
    volumes:
      - ./maps:/opt/maps:ro
      - robot_logs:/var/log/robot
    depends_on:
      - robot_main
    restart: unless-stopped
    command: ["ros2 launch nav2_bringup navigation_launch.py"]

  # 웹 인터페이스
  web_interface:
    image: ghcr.io/foxglove/studio:latest
    container_name: robot_web
    ports:
      - "8080:8080"
    environment:
      - FOXGLOVE_PORT=8080
    depends_on:
      - robot_main

  # 로그 수집
  log_collector:
    image: fluentd:latest
    container_name: log_collector
    volumes:
      - robot_logs:/fluentd/log
      - ./fluentd/conf:/fluentd/etc
    ports:
      - "24224:24224"

volumes:
  robot_logs:
```

---

## 🛠️ 빌드 자동화

### 빌드 스크립트
```bash
#!/bin/bash
# build.sh - 자동화된 Docker 빌드 스크립트

set -e  # 에러 시 스크립트 중단

# 설정
IMAGE_NAME="robot_system"
IMAGE_TAG="${CI_COMMIT_SHA:-latest}"
DOCKER_REGISTRY="your-registry.com"

# 함수 정의
print_info() {
    echo "[INFO] $1"
}

print_error() {
    echo "[ERROR] $1" >&2
}

# 공유 폴더 생성
create_shared_folder() {
    local shared_path="$HOME/robot_shared"
    if [ ! -d "$shared_path" ]; then
        mkdir -p "$shared_path"
        print_info "Created shared folder: $shared_path"
    fi
}

# Docker 이미지 빌드
build_docker_image() {
    print_info "Building Docker image: $IMAGE_NAME:$IMAGE_TAG"
    
    docker build \
        --target production \
        --build-arg USERNAME=$USER \
        --build-arg USER_UID=$(id -u) \
        --build-arg USER_GID=$(id -g) \
        -t $IMAGE_NAME:$IMAGE_TAG \
        -t $IMAGE_NAME:latest \
        --no-cache \
        .
    
    print_info "Docker image built successfully"
}

# 이미지 푸시 (CI 환경에서)
push_image() {
    if [ "$CI" = "true" ]; then
        print_info "Pushing image to registry"
        docker tag $IMAGE_NAME:$IMAGE_TAG $DOCKER_REGISTRY/$IMAGE_NAME:$IMAGE_TAG
        docker push $DOCKER_REGISTRY/$IMAGE_NAME:$IMAGE_TAG
        
        if [ "$CI_COMMIT_BRANCH" = "main" ]; then
            docker tag $IMAGE_NAME:$IMAGE_TAG $DOCKER_REGISTRY/$IMAGE_NAME:latest
            docker push $DOCKER_REGISTRY/$IMAGE_NAME:latest
        fi
    fi
}

# 테스트 실행
run_tests() {
    print_info "Running tests in container"
    
    docker run --rm \
        --network host \
        $IMAGE_NAME:$IMAGE_TAG \
        "source /opt/overlay/setup.bash && colcon test --packages-select robot_package"
}

# 메인 실행
main() {
    create_shared_folder
    build_docker_image
    run_tests
    push_image
    print_info "Build process completed successfully"
}

# 실행
main "$@"
```

---

## 🔄 CI/CD 파이프라인

### GitHub Actions
```yaml
# .github/workflows/build-and-deploy.yml
name: Build and Deploy ROS2 Robot

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
    - name: Checkout repository
      uses: actions/checkout@v4

    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v3

    - name: Log in to Container Registry
      uses: docker/login-action@v3
      with:
        registry: ${{ env.REGISTRY }}
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}

    - name: Extract metadata
      id: meta
      uses: docker/metadata-action@v5
      with:
        images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
        tags: |
          type=ref,event=branch
          type=ref,event=pr
          type=sha,prefix={{branch}}-

    - name: Build and test
      uses: docker/build-push-action@v5
      with:
        context: .
        target: build
        load: true
        tags: robot_system:test
        cache-from: type=gha
        cache-to: type=gha,mode=max

    - name: Run tests
      run: |
        docker run --rm robot_system:test \
          bash -c "source /opt/overlay/setup.bash && colcon test"

    - name: Build and push production image
      uses: docker/build-push-action@v5
      with:
        context: .
        target: production
        push: true
        tags: ${{ steps.meta.outputs.tags }}
        labels: ${{ steps.meta.outputs.labels }}
        cache-from: type=gha
        cache-to: type=gha,mode=max

  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - name: Deploy to fleet
      run: |
        # Fleet 배포 로직 (예: AWS IoT Greengrass, Kubernetes 등)
        echo "Deploying to production fleet..."
```

### GitLab CI
```yaml
# .gitlab-ci.yml
stages:
  - build
  - test
  - deploy

variables:
  DOCKER_DRIVER: overlay2
  DOCKER_BUILDKIT: 1

build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build --target production -t $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA .
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  only:
    - main
    - develop

test:
  stage: test
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker pull $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
    - docker run --rm $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
        bash -c "source /opt/overlay/setup.bash && colcon test"

deploy_staging:
  stage: deploy
  script:
    - echo "Deploying to staging environment"
    - docker-compose -f docker-compose.staging.yml up -d
  only:
    - develop

deploy_production:
  stage: deploy
  script:
    - echo "Deploying to production fleet"
    - ./scripts/deploy-to-fleet.sh $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  only:
    - main
  when: manual
```

---

## 🌐 Fleet 관리

### AWS IoT Greengrass 통합
```yaml
# greengrass-component-recipe.yaml
RecipeFormatVersion: "2020-01-25"
ComponentName: "com.robot.ros2.main"
ComponentVersion: "1.0.0"
ComponentType: "aws.greengrass.generic"
ComponentDescription: "ROS2 Robot Main Component"

ComponentConfiguration:
  DefaultConfiguration:
    robot_id: "{iot:thingName}"
    ros_domain_id: 0
    log_level: "INFO"

Manifests:
  - Platform:
      os: linux
    Lifecycle:
      Install:
        RequiresPrivilege: true
        Script: |
          # Docker 설치 확인
          which docker || {
            echo "Installing Docker..."
            curl -fsSL https://get.docker.com -o get-docker.sh
            sh get-docker.sh
          }
          
          # 이미지 풀
          docker pull {configuration:/docker_image}
          
      Run:
        RequiresPrivilege: true
        Script: |
          # 기존 컨테이너 정리
          docker stop robot_main || true
          docker rm robot_main || true
          
          # 새 컨테이너 실행
          docker run -d \
            --name robot_main \
            --network host \
            --privileged \
            -e ROS_DOMAIN_ID={configuration:/ros_domain_id} \
            -e ROBOT_ID={configuration:/robot_id} \
            -v /dev:/dev \
            {configuration:/docker_image}
            
      Shutdown:
        Script: |
          docker stop robot_main || true
          docker rm robot_main || true

ComponentDependencies:
  aws.greengrass.DockerApplicationManager:
    VersionRequirement: ">=2.0.0"
  aws.greengrass.TokenExchangeService:
    VersionRequirement: ">=2.0.0"
```

### Fleet 배포 스크립트
```bash
#!/bin/bash
# deploy-to-fleet.sh - Fleet 전체 배포

set -e

IMAGE_URI="$1"
FLEET_NAME="${2:-production-robots}"

print_info() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] INFO: $1"
}

# Greengrass 컴포넌트 생성
create_component() {
    local version=$(date +%s)
    
    print_info "Creating Greengrass component version $version"
    
    aws greengrassv2 create-component-version \
        --inline-recipe fileb://greengrass-component-recipe.yaml \
        --lambda-function '{}' \
        --configuration-values '{
            "docker_image": "'$IMAGE_URI'",
            "ros_domain_id": "0"
        }'
}

# Fleet에 배포
deploy_to_fleet() {
    local deployment_name="ros2-deployment-$(date +%s)"
    
    print_info "Creating deployment: $deployment_name"
    
    aws greengrassv2 create-deployment \
        --target-arn "arn:aws:iot:region:account:thinggroup/$FLEET_NAME" \
        --deployment-name "$deployment_name" \
        --components '{
            "com.robot.ros2.main": {
                "componentVersion": "1.0.0"
            }
        }' \
        --deployment-policies '{
            "failureHandlingPolicy": "ROLLBACK",
            "componentUpdatePolicy": {
                "timeoutInSeconds": 600,
                "action": "NOTIFY_COMPONENTS"
            }
        }'
}

# 배포 상태 확인
check_deployment_status() {
    print_info "Checking deployment status..."
    
    aws greengrassv2 list-deployments \
        --target-arn "arn:aws:iot:region:account:thinggroup/$FLEET_NAME" \
        --history-filter LATEST_ONLY
}

# 메인 실행
main() {
    if [ -z "$IMAGE_URI" ]; then
        echo "Usage: $0 <docker-image-uri> [fleet-name]"
        exit 1
    fi
    
    create_component
    deploy_to_fleet
    check_deployment_status
    
    print_info "Fleet deployment initiated successfully"
}

main "$@"
```

---

## 📊 모니터링 및 로깅

### Prometheus 메트릭
```python
# metrics_node.py - ROS2 메트릭 수집
import rclpy
from rclpy.node import Node
from prometheus_client import start_http_server, Gauge, Counter
import psutil
import os

class MetricsNode(Node):
    def __init__(self):
        super().__init__('metrics_node')
        
        # Prometheus 메트릭 정의
        self.cpu_usage = Gauge('robot_cpu_usage_percent', 'CPU usage percentage')
        self.memory_usage = Gauge('robot_memory_usage_bytes', 'Memory usage in bytes')
        self.disk_usage = Gauge('robot_disk_usage_percent', 'Disk usage percentage')
        self.node_count = Gauge('ros2_node_count', 'Number of active ROS2 nodes')
        self.topic_count = Gauge('ros2_topic_count', 'Number of active ROS2 topics')
        self.message_count = Counter('ros2_messages_total', 'Total ROS2 messages', 
                                   ['topic_name'])
        
        # HTTP 서버 시작 (메트릭 엔드포인트)
        start_http_server(8000)
        
        # 주기적 메트릭 수집
        self.timer = self.create_timer(5.0, self.collect_metrics)
        
        self.get_logger().info('Metrics collection started on port 8000')
    
    def collect_metrics(self):
        # 시스템 메트릭
        self.cpu_usage.set(psutil.cpu_percent())
        self.memory_usage.set(psutil.virtual_memory().used)
        self.disk_usage.set(psutil.disk_usage('/').percent)
        
        # ROS2 메트릭 (간단한 예제)
        # 실제로는 ROS2 introspection API 사용
        self.node_count.set(self.get_node_count())
        self.topic_count.set(self.get_topic_count())
    
    def get_node_count(self):
        # ros2 node list 명령어 실행하여 노드 수 계산
        import subprocess
        try:
            result = subprocess.run(['ros2', 'node', 'list'], 
                                  capture_output=True, text=True)
            return len(result.stdout.strip().split('\n'))
        except:
            return 0
    
    def get_topic_count(self):
        # ros2 topic list 명령어 실행하여 토픽 수 계산
        import subprocess
        try:
            result = subprocess.run(['ros2', 'topic', 'list'], 
                                  capture_output=True, text=True)
            return len(result.stdout.strip().split('\n'))
        except:
            return 0

def main():
    rclpy.init()
    metrics_node = MetricsNode()
    rclpy.spin(metrics_node)
    metrics_node.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

### 중앙 모니터링 대시보드
```yaml
# monitoring-stack.yml
version: '3.8'

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--web.console.libraries=/etc/prometheus/console_libraries'
      - '--web.console.templates=/etc/prometheus/consoles'

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
    volumes:
      - grafana_data:/var/lib/grafana
      - ./grafana/dashboards:/etc/grafana/provisioning/dashboards
      - ./grafana/datasources:/etc/grafana/provisioning/datasources

  loki:
    image: grafana/loki:latest
    container_name: loki
    ports:
      - "3100:3100"
    volumes:
      - ./loki-config.yml:/etc/loki/local-config.yaml
    command: -config.file=/etc/loki/local-config.yaml

  promtail:
    image: grafana/promtail:latest
    container_name: promtail
    volumes:
      - /var/log:/var/log:ro
      - ./promtail-config.yml:/etc/promtail/config.yml
    command: -config.file=/etc/promtail/config.yml

volumes:
  prometheus_data:
  grafana_data:
```

---

## 🔧 운영 도구

### 헬스체크 시스템
```python
# health_check.py - 로봇 상태 모니터링
import rclpy
from rclpy.node import Node
from diagnostic_msgs.msg import DiagnosticArray
import requests
import json

class HealthChecker(Node):
    def __init__(self):
        super().__init__('health_checker')
        
        # 중앙 서버 설정
        self.central_server = self.declare_parameter('central_server', 
                                                   'https://fleet.company.com').value
        self.robot_id = self.declare_parameter('robot_id', 'robot_001').value
        
        # 진단 메시지 구독
        self.diagnostics_sub = self.create_subscription(
            DiagnosticArray, '/diagnostics', self.diagnostics_callback, 10)
        
        # 주기적 상태 보고
        self.timer = self.create_timer(30.0, self.report_health)
        
        self.health_status = {
            'robot_id': self.robot_id,
            'status': 'UNKNOWN',
            'last_update': None,
            'diagnostics': []
        }
    
    def diagnostics_callback(self, msg):
        # 진단 정보 수집
        self.health_status['diagnostics'] = []
        
        for status in msg.status:
            self.health_status['diagnostics'].append({
                'name': status.name,
                'level': status.level,
                'message': status.message
            })
        
        # 전체 상태 판단
        error_count = sum(1 for d in self.health_status['diagnostics'] 
                         if d['level'] >= 2)  # ERROR 이상
        
        if error_count > 0:
            self.health_status['status'] = 'ERROR'
        elif any(d['level'] == 1 for d in self.health_status['diagnostics']):
            self.health_status['status'] = 'WARNING'
        else:
            self.health_status['status'] = 'OK'
    
    def report_health(self):
        # 중앙 서버에 상태 보고
        self.health_status['last_update'] = self.get_clock().now().to_msg()
        
        try:
            response = requests.post(
                f"{self.central_server}/api/robot/health",
                json=self.health_status,
                timeout=10
            )
            
            if response.status_code == 200:
                self.get_logger().debug('Health status reported successfully')
            else:
                self.get_logger().warning(f'Failed to report health: {response.status_code}')
                
        except Exception as e:
            self.get_logger().error(f'Health report failed: {e}')

def main():
    rclpy.init()
    health_checker = HealthChecker()
    rclpy.spin(health_checker)
    health_checker.destroy_node()
    rclpy.shutdown()
```

---

## 📖 주요 참고문헌

1. **Docker for ROS2**: https://roboticseabass.com/2023/07/09/updated-guide-docker-and-ros2/
2. **AWS IoT Greengrass**: https://aws.amazon.com/blogs/robotics/deploy-and-manage-ros-robots-with-aws-iot-greengrass-2-0-and-docker/
3. **ROS2 Docker Guide**: https://docs.ros.org/en/foxy/How-To-Guides/Run-2-nodes-in-single-or-separate-docker-containers.html
4. **Production Deployment**: https://medium.com/@jev.kuznetsov/speed-up-your-robotics-development-with-a-ros2-docker-stack-4332274a7a76
5. **Multi-container Setup**: https://automaticaddison.com/the-complete-guide-to-docker-for-ros-2-jazzy-projects/