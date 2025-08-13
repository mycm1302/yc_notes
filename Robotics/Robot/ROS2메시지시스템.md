# ROS2메시지시스템

> 상위: [[ROS2]]

msg, srv, action 메시지 정의와 직렬화, 타입 시스템을 다룹니다.

---

## 📋 메시지 타입

### 3가지 인터페이스 타입
- **Messages (.msg)**: Topic 통신용 데이터 구조
- **Services (.srv)**: Service 요청/응답 정의
- **Actions (.action)**: Action 목표/피드백/결과 정의

### 기본 데이터 타입
- **정수형**: int8, uint8, int16, uint16, int32, uint32, int64, uint64
- **실수형**: float32, float64
- **논리형**: bool
- **문자열**: string, wstring
- **시간**: builtin_interfaces/Time, Duration

---

## 📝 Message 정의

### .msg 파일 구조
```msg
# 헤더 정보
std_msgs/Header header

# 기본 타입
int32 id
string name
float64 position
bool is_active

# 배열
float64[] values
int32[10] fixed_array

# 상수
int32 STATUS_OK=0
int32 STATUS_ERROR=1
```

### 복합 메시지
- **중첩 구조**: 다른 메시지 타입 포함
- **배열**: 동적/고정 크기 배열
- **상수**: 컴파일 타임 상수 정의
- **기본값**: 필드 기본값 설정

---

## 🔄 Service 정의

### .srv 파일 구조
```srv
# 요청 (Request)
string name
int32 age
---
# 응답 (Response)
bool success
string message
int32 result_code
```

### 특징
- **구분자**: `---`로 요청/응답 분리
- **타입 안전성**: 강타입 검사
- **버전 호환성**: 필드 추가/제거 규칙
- **중첩 가능**: 복합 타입 사용 가능

---

## ⚡ Action 정의

### .action 파일 구조
```action
# 목표 (Goal)
float64 target_position
float64 max_velocity
---
# 결과 (Result)  
float64 final_position
bool success
string error_message
---
# 피드백 (Feedback)
float64 current_position
float64 remaining_distance
```

### 3단계 구조
- **Goal**: 달성하고자 하는 목표
- **Result**: 최종 실행 결과
- **Feedback**: 실행 중 진행 상황

---

## 🔧 메시지 생성 및 빌드

### 패키지 구성
```
my_interfaces/
├── CMakeLists.txt
├── package.xml
├── msg/
│   └── CustomMessage.msg
├── srv/
│   └── CustomService.srv
└── action/
    └── CustomAction.action
```

### CMakeLists.txt 설정
```cmake
find_package(rosidl_default_generators REQUIRED)

rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/CustomMessage.msg"
  "srv/CustomService.srv"
  "action/CustomAction.action"
  DEPENDENCIES std_msgs
)
```

### package.xml 의존성
```xml
<build_depend>rosidl_default_generators</build_depend>
<exec_depend>rosidl_default_runtime</exec_depend>
<member_of_group>rosidl_interface_packages</member_of_group>
```

---

## 🔄 직렬화 시스템

### IDL (Interface Definition Language)
- **OMG IDL**: 표준 인터페이스 정의 언어
- **언어 독립적**: 다양한 언어로 매핑
- **플랫폼 독립적**: 아키텍처 무관
- **버전 관리**: 하위 호환성 지원

### 직렬화 방식
- **CDR**: Common Data Representation (기본)
- **JSON**: 사람이 읽기 쉬운 형식
- **XML**: 구조화된 마크업
- **Custom**: 사용자 정의 직렬화

---

## 📊 타입 시스템

### 강타입 시스템
- **컴파일 타임 검증**: 타입 불일치 사전 방지
- **런타임 안전성**: 타입 변환 오류 방지
- **인터페이스 호환성**: 타입 진화 규칙
- **메타데이터**: 타입 정보 런타임 접근

### 타입 적응
- **Type Adaptation**: 기존 타입 재사용
- **Custom Allocators**: 메모리 관리 최적화
- **Zero-copy**: 메모리 복사 없는 전송
- **Shared Memory**: 공유 메모리 활용

---

## 🛠️ 개발 도구

### 코드 생성
- **rosidl**: 인터페이스 컴파일러
- **Language Bindings**: C++, Python, Java 등
- **Type Support**: 타입 지원 라이브러리
- **Introspection**: 런타임 타입 정보

### 디버깅 도구
- **ros2 interface show**: 인터페이스 정보 출력
- **ros2 topic echo**: 메시지 내용 확인
- **rqt_msg**: GUI 메시지 뷰어
- **rosbag2**: 메시지 기록/재생

### 예제 명령어
```bash
# 메시지 정의 확인
ros2 interface show sensor_msgs/msg/Image

# Topic 메시지 출력
ros2 topic echo /camera/image_raw

# Service 호출
ros2 service call /add_two_ints example_interfaces/srv/AddTwoInts "{a: 1, b: 2}"
```

---

## 📖 주요 참고문헌

1. **ROS2 Interfaces**: https://docs.ros.org/en/humble/Concepts/About-ROS-Interfaces.html
2. **IDL Specification**: OMG IDL 4.2 표준 문서
3. **Message Design Guidelines**: 메시지 설계 모범 사례
4. **Type System**: ROS2 타입 시스템 설계 문서