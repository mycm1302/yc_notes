# ROS2분산시스템

> 상위: [[ROS2]]

Discovery, Security, Multi-machine 환경에서의 분산 시스템 구성을 다룹니다.

---

## 🌐 분산 아키텍처

### 분산 특성
- **네트워크 투명성**: 로컬/원격 구분 없음
- **위치 독립성**: 노드 위치 무관
- **동적 발견**: 런타임 노드 발견
- **장애 허용성**: 일부 노드 장애 대응

### 네트워크 토폴로지
- **Star**: 중앙 집중형
- **Mesh**: 완전 연결형
- **Tree**: 계층형
- **Hybrid**: 혼합형

---

## 🔍 Discovery 메커니즘

### DDS Discovery Protocol
- **SPDP**: Simple Participant Discovery Protocol
- **SEDP**: Simple Endpoint Discovery Protocol
- **Multicast**: 멀티캐스트 기반 발견
- **Unicast**: 유니캐스트 대안

### 발견 과정
1. **Participant Discovery**: 참여자 발견
2. **Endpoint Discovery**: 엔드포인트 발견
3. **Matching**: 호환 가능한 엔드포인트 매칭
4. **Communication**: 통신 채널 설정

### Discovery 설정
```xml
<!-- DDS 설정 파일 -->
<profiles>
  <participant profile_name="participant_profile">
    <rtps>
      <builtin>
        <discovery_config>
          <discoveryProtocol>SIMPLE</discoveryProtocol>
          <use_SIMPLE_EndpointDiscoveryProtocol>true</use_SIMPLE_EndpointDiscoveryProtocol>
        </discovery_config>
      </builtin>
    </rtps>
  </participant>
</profiles>
```

---

## 🔒 보안 시스템

### SROS2 (Secure ROS2)
- **PKI 기반 인증**: 공개키 인프라
- **노드 인증**: 신원 확인
- **권한 부여**: 접근 제어
- **데이터 암호화**: 메시지 보호

### 보안 설정 과정
```bash
# 보안 키스토어 생성
ros2 security create_keystore demo_keystore

# 인증서 생성
ros2 security create_enclave demo_keystore /talker_listener/talker

# 정책 설정
ros2 security create_permission demo_keystore /talker_listener/talker

# 환경 변수 설정
export ROS_SECURITY_KEYSTORE=~/demo_keystore
export ROS_SECURITY_ENABLE=true
export ROS_SECURITY_STRATEGY=Enforce
```

### DDS Security
- **Authentication**: 플러그인 기반 인증
- **Access Control**: 세분화된 접근 제어
- **Cryptography**: 강력한 암호화
- **Key Management**: 동적 키 관리

---

## 🏢 Multi-machine 설정

### 네트워크 구성
- **도메인 ID**: 논리적 네트워크 분리 (0-232)
- **멀티캐스트**: 효율적 브로드캐스트
- **방화벽**: 포트 개방 및 규칙 설정
- **라우팅**: 서브넷 간 통신

### 포트 설정
```bash
# 기본 DDS 포트 범위
# Multicast: 7400 + (250 * domain_id)
# Unicast: 7410 + (250 * domain_id) + offset
```

### 환경 변수
```bash
# 도메인 설정
export ROS_DOMAIN_ID=42

# 네트워크 인터페이스 지정
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
export FASTRTPS_DEFAULT_PROFILES_FILE=/path/to/profile.xml
```

---

## 🔧 네트워크 최적화

### 대역폭 관리
- **QoS 설정**: 네트워크 품질 제어
- **압축**: 데이터 압축 전송
- **배칭**: 메시지 일괄 전송
- **필터링**: 불필요한 트래픽 차단

### 지연시간 최적화
- **로컬 우선**: 로컬 통신 우선
- **캐싱**: 메타데이터 캐싱
- **버퍼링**: 적응적 버퍼 크기
- **직접 연결**: 중계 노드 우회

---

## 🛡️ 장애 처리

### 네트워크 분할 대응
- **Partition Tolerance**: 네트워크 분할 허용
- **Graceful Degradation**: 우아한 성능 저하
- **Reconnection**: 자동 재연결
- **State Synchronization**: 상태 동기화

### 노드 장애 감지
- **Liveliness**: 생존 여부 확인
- **Heartbeat**: 주기적 신호
- **Timeout**: 응답 시간 제한
- **Health Monitoring**: 건강 상태 모니터링

---

## 🌍 클라우드 통합

### 클라우드 배포
- **AWS RoboMaker**: Amazon 로봇 서비스
- **Azure IoT**: Microsoft 클라우드 서비스
- **Google Cloud**: 구글 클라우드 플랫폼
- **Kubernetes**: 컨테이너 오케스트레이션

### Edge Computing
- **Edge-Cloud Hybrid**: 엣지-클라우드 하이브리드
- **Local Processing**: 로컬 처리 우선
- **Bandwidth Optimization**: 대역폭 최적화
- **Latency Reduction**: 지연시간 감소

---

## 🔍 모니터링 도구

### 네트워크 모니터링
```bash
# 노드 발견 상태 확인
ros2 daemon status

# 네트워크 그래프 확인
ros2 node list
ros2 topic list

# DDS 정보 확인
ros2 doctor --report
```

### 시각화 도구
- **rqt_graph**: 네트워크 그래프 시각화
- **Wireshark**: 네트워크 패킷 분석
- **DDS Spy**: DDS 트래픽 모니터링
- **System Monitor**: 시스템 리소스 모니터링

---

## 📖 주요 참고문헌

1. **ROS2 Networking**: https://docs.ros.org/en/humble/Concepts/About-Domain-ID.html
2. **DDS Security Specification**: OMG DDS Security 표준
3. **Multi-robot Systems**: 다중 로봇 시스템 네트워킹
4. **Cloud Robotics**: 클라우드 로보틱스 아키텍처