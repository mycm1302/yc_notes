# IMU (관성 측정 장치)

> 상위: [[센서시스템]]

IMU는 로봇의 자세, 가속도, 각속도를 측정하는 핵심 센서 시스템입니다.

## 📊 기본 개념

### 정의
관성 측정 장치(Inertial Measurement Unit, IMU)는 가속도계, 자이로스코프, 그리고 때로는 자력계를 결합하여 물체의 비력(specific force), 각속도, 자기장을 측정하는 전자 장치입니다.

### 구성 요소
- **3축 가속도계**: 선형 가속도 측정 (m/s²)
- **3축 자이로스코프**: 각속도 측정 (°/s 또는 rad/s)  
- **3축 자력계** (선택적): 자기장 측정 (μT 또는 Gauss)

---

## 🔧 센서 구성

### 6-DOF IMU (6자유도)
- **구성**: 3축 가속도계 + 3축 자이로스코프
- **측정**: 선형 가속도 및 각속도
- **용도**: 기본적인 자세 추정 및 운동 감지

### 9-DOF IMU (9자유도)
- **구성**: 6-DOF + 3축 자력계
- **측정**: 가속도, 각속도, 자기장
- **용도**: 절대 방위각 측정 포함한 완전한 자세 추정
- **별칭**: IMMU (Inertial Magnetic Measurement Unit)

### AHRS (자세 방위 기준 시스템)
- **정의**: IMU + 필터링 알고리즘
- **출력**: 자세 정보 (Roll, Pitch, Yaw)
- **특징**: 실시간 자세 추정 및 안정화

---

## ⚙️ 센서 기술

### MEMS 기술
- **원리**: 미세 전자기계 시스템
- **특징**:
  - 소형화 (마이크로미터 단위)
  - 저전력 소모
  - 대량 생산 가능
  - 상대적으로 저비용

### 가속도계 원리
- **구조**: 스프링에 매달린 증명 질량(proof mass)
- **측정**: 관성력에 의한 질량의 변위
- **출력**: 중력 포함한 전체 가속도
- **범위**: ±2g ~ ±16g (일반적)

### 자이로스코프 원리
- **MEMS 진동 자이로**: 진동하는 질량의 코리올리 효과 이용
- **측정**: 각속도 (회전률)
- **범위**: ±250°/s ~ ±2000°/s
- **정확도**: 0.01°/hr ~ 1°/hr (등급별)

---

## 🔌 전기적 특성

### 전원 사양
- **동작 전압**: 2.2V ~ 3.6V (일반적)
- **소비 전류**: 400μA ~ 1mA (동작 시)
- **슬립 모드**: 3μA 이하

### 통신 인터페이스
- **I2C**: 표준 주소 0x68, 0x69
- **SPI**: 고속 데이터 전송 (최대 20MHz)
- **UART**: 일부 고급 모듈에서 지원

### 신호 처리
- **ADC 해상도**: 16비트 ~ 24비트
- **샘플링 속도**: 최대 8kHz
- **내장 필터**: 로우패스, 하이패스 필터 옵션

---

## 🤖 로봇공학 응용

### 자세 제어
- **쿼드로터**: 안정화 제어의 핵심 센서
- **이족 보행 로봇**: 균형 유지 및 보행 제어
- **로봇 팔**: 베이스 및 엔드이펙터 자세 모니터링

### 내비게이션
- **관성 항법**: GPS 신호 차단 시 위치 추정
- **SLAM**: 로봇의 운동 모델 제공
- **오도메트리**: 휠 엔코더와 융합하여 정확도 향상

### 충격 감지
- **충돌 감지**: 예상치 못한 가속도 변화 감지
- **낙하 보호**: 자유낙하 상태 인식
- **진동 모니터링**: 기계적 이상 진단

---

## 📐 성능 사양

### 정확도 지표
- **초기 바이어스**: 
  - 가속도계: <5mg (일반용) ~ <100μg (내비게이션용)
  - 자이로스코프: <0.2°/s (일반용) ~ <0.01°/hr (내비게이션용)
- **노이즈 밀도**: 
  - 가속도: 150μg/√Hz
  - 자이로: 0.01°/s/√Hz

### 환경 특성
- **동작 온도**: -40°C ~ +85°C
- **충격 내성**: 10,000g (일시적)
- **진동 내성**: 우수한 기계적 안정성

---

## 🔄 데이터 처리

### 캘리브레이션
- **가속도계**: 6면 캘리브레이션으로 바이어스/스케일 보정
- **자이로스코프**: 정적 바이어스 제거
- **자력계**: 하드/소프트 아이언 왜곡 보정

### 센서 융합
- **상보 필터**: 가속도계와 자이로 데이터 융합
- **칼만 필터**: 확률적 상태 추정
- **Madgwick/Mahony 필터**: 효율적인 자세 추정

### 좌표계 변환
- **쿼터니언**: 특이점 없는 회전 표현
- **오일러각**: 직관적인 Roll-Pitch-Yaw
- **회전 행렬**: 3D 변환 계산

---

## 💻 구현 예제

### 하드웨어 연결 (I2C)
```
IMU Module:
VCC → 3.3V
GND → Ground  
SDA → A4 (Arduino Uno)
SCL → A5 (Arduino Uno)
```

### 기본 센서 융합
```cpp
// 상보 필터 예제
float alpha = 0.98;
float dt = 0.01; // 10ms

// 자이로 적분
angle_gyro += gyro_rate * dt;

// 가속도계 각도
angle_accel = atan2(ay, az) * 180/PI;

// 융합
angle = alpha * (angle + gyro_rate * dt) + 
        (1-alpha) * angle_accel;
```

---

## 🔗 상용 제품 예시

### 고성능 IMU
- **VectorNav VN-100**: 산업용 등급
- **Xsens MTi 시리즈**: 고정밀 AHRS
- **Bosch BMI088**: 고성능 MEMS

### 개발용 모듈
- **MPU-9250**: 9축 통합 모듈
- **ICM-20948**: 최신 9축 센서
- **Adafruit LSM9DS1**: 개발자 친화적

---

## 📚 참고 문헌

1. **학술 논문**:
   - ["Automatic calibration for inertial measurement unit"](https://ieeexplore.ieee.org/document/6485340/) - IEEE 2013
   - [Wikipedia: Inertial Measurement Unit](https://en.wikipedia.org/wiki/Inertial_measurement_unit) - 2025

2. **기술 자료**:
   - ["What is an Inertial Measurement Unit?"](https://www.vectornav.com/resources/detail/what-is-an-inertial-measurement-unit-imu) - VectorNav
   - ["Inertial Measurement Unit (IMU) – An Introduction"](https://www.advancednavigation.com/tech-articles/inertial-measurement-unit-imu-an-introduction/) - Advanced Navigation 2025

3. **교육 자료**:
   - ["Accelerometers, Gyros, and IMUs: The Basics"](https://itp.nyu.edu/physcomp/lessons/accelerometers-gyros-and-imus-the-basics/) - NYU ITP
   - ["A Complete Guide to Inertial Measurement Unit"](https://www.jouav.com/blog/inertial-measurement-unit.html) - JOUAV 2025

4. **산업 응용**:
   - [RobotShop: Inertia Measurement Units](https://www.robotshop.com/collections/sensors-imu)
   - [Robu: IMU, Accelerometer, Magnetometer & Gyroscope](https://robu.in/product-category/sensor-modules/imu-accelerometer-magnetometer-gyroscope/)

---

## 🔗 연결 분야
- 상위: [[센서시스템]]
- 응용: [[상태추정]], [[제어시스템]]
- 융합: [[센서융합]], [[칼만필터]]
- 관련: [[엔코더]], [[GPS]]