# ZMP (Zero Moment Point)

> 상위: [[휴머노이드 연구기술]]

## 정의

ZMP(Zero Moment Point)는 지면과 로봇 발 사이의 접촉점에서 수평 방향 모멘트의 합이 0이 되는 점으로, 이족 보행 로봇의 동적 안정성을 평가하는 핵심 지표입니다.

## 수학적 정의

### 기본 개념
```
ZMP 조건: Σ Mhorizontal = 0
여기서 Mhorizontal은 해당 점에서의 수평 모멘트
```

### 좌표 계산
```
xZMP = Σ(fi × yi) / Σ(fzi)
yZMP = Σ(fi × xi) / Σ(fzi)

여기서:
- fi: i번째 힘 벡터
- xi, yi: i번째 힘의 작용점
- fzi: i번째 수직 힘 성분
```

### 동역학적 표현
```
ZMP = (Σ(mi × g × ri) - Σ(mi × ai × ri)) / Σ(mi × g)

여기서:
- mi: i번째 링크 질량
- g: 중력 가속도
- ri: i번째 링크 위치
- ai: i번째 링크 가속도
```

## 물리적 의미

### 안정성 기준
- **안정**: ZMP ∈ Support Polygon (발바닥 영역 내부)
- **불안정**: ZMP ∉ Support Polygon (발바닥 영역 외부)
- **한계**: ZMP가 지지 영역 경계에 위치

### 지지 다각형 (Support Polygon)
- **단일 지지**: 한 발이 지면에 접촉 (발바닥 영역)
- **이중 지지**: 양발이 지면에 접촉 (두 발바닥을 잇는 볼록 껍질)
- **동적 변화**: 보행 중 지지 영역이 지속적으로 변화

## ZMP 기반 보행 제어

### 1. ZMP 궤적 계획

#### 미리 계획된 ZMP (Pre-planned ZMP)
```python
# 단순한 ZMP 패턴 예시
def generate_zmp_trajectory(step_time, step_width):
    zmp_trajectory = []
    
    # 이중 지지 단계
    zmp_x = interpolate(left_foot_x, right_foot_x, ratio=0.5)
    
    # 단일 지지 단계  
    zmp_x = support_foot_x
    
    return zmp_trajectory
```

#### 가변 ZMP (Variable ZMP)
- **목적**: 에너지 효율성 향상
- **방법**: 허용 ZMP 영역 내에서 최적 경로 탐색
- **제약**: 지지 영역 내부 유지

### 2. ZMP 추적 제어

#### 피드백 제어
```
CoM 보정 = Kp × (ZMPdesired - ZMPactual)
여기서 Kp는 비례 게인
```

#### 퍼지 로직 제어
- **입력**: ZMP 오차, ZMP 변화율
- **출력**: CoM 위치 보정량
- **장점**: 비선형성에 강건

### 3. ZMP 기반 보행 패턴 생성

#### Cart-Table Model
```
질량중심 방정식:
ẍ = (g/h) × (x - xZMP)

여기서:
- x: 질량중심 위치
- h: 질량중심 높이
- g: 중력 가속도
```

#### LIPM (Linear Inverted Pendulum Model)
- **가정**: 로봇을 단순한 역진자로 모델링
- **장점**: 해석적 해 존재, 실시간 계산 가능
- **단점**: 상체 움직임과 다리 질량 무시

## 실제 구현

### 1. ZMP 측정

#### Force/Torque Sensor
```cpp
// 6축 F/T 센서를 이용한 ZMP 계산
Vector3 calculateZMP(Vector3 force, Vector3 moment) {
    double x_zmp = -moment.y / force.z;
    double y_zmp = moment.x / force.z;
    return Vector3(x_zmp, y_zmp, 0);
}
```

#### 압력 센서 배열
- **분포 측정**: 발바닥 여러 지점의 압력
- **ZMP 계산**: 압력 분포로부터 무게중심 계산
- **해상도**: 센서 개수에 따른 정밀도

### 2. ZMP 제어기 설계

#### PID 제어
```cpp
class ZMPController {
private:
    double kp, ki, kd;
    double integral_error = 0;
    double previous_error = 0;
    
public:
    double control(double zmp_error, double dt) {
        integral_error += zmp_error * dt;
        double derivative = (zmp_error - previous_error) / dt;
        
        double output = kp * zmp_error + 
                       ki * integral_error + 
                       kd * derivative;
        
        previous_error = zmp_error;
        return output;
    }
};
```

#### 상태공간 제어
```
상태: x = [CoM_position, CoM_velocity, ZMP_position]
제어입력: u = CoM_acceleration
출력: y = ZMP_position

상태방정식: ẋ = Ax + Bu
출력방정식: y = Cx
```

## 고급 ZMP 기법

### 1. 예측 ZMP 제어

#### Model Predictive Control (MPC)
```
목적함수:
J = Σ(||ZMP_predicted - ZMP_reference||² + R||u||²)

제약조건:
- ZMP ∈ Support Polygon
- |CoM_acceleration| ≤ a_max
```

#### 미래 지지 영역 고려
- **다단계 예측**: 여러 스텝 앞의 지지 영역 예측
- **최적화**: 전체 보행 주기에 대한 최적 ZMP 궤적

### 2. 적응적 ZMP 제어

#### 지형 적응
- **경사면**: 중력 방향 변화 고려
- **불규칙 지형**: 발 접촉 모델 실시간 업데이트
- **계단**: 높이 변화에 따른 ZMP 재계산

#### 외란 대응
- **밀림**: 급격한 ZMP 변화 대응
- **미끄러짐**: 마찰 계수 변화 고려
- **바람**: 외부 힘의 영향 보상

### 3. ZMP 안정성 마진

#### 안정성 지표
```
안정성 마진 = min(distance(ZMP, boundary_of_support_polygon))
- 양수: 안정
- 0: 임계
- 음수: 불안정
```

#### 동적 안정성
- **ZMP 속도**: ZMP 이동 속도 제한
- **ZMP 가속도**: 급격한 ZMP 변화 방지
- **예측 안정성**: 미래 ZMP 궤적의 안정성 평가

## 한계 및 확장

### ZMP 기법의 한계

#### 보수적 접근
- **정적 안정**: 동적 효과 무시
- **에너지 비효율**: 과도한 안전 마진
- **속도 제한**: 빠른 보행에 부적합

#### 가정의 한계
- **강체 가정**: 유연한 발바닥 무시
- **점 접촉**: 실제 면 접촉과 차이
- **마찰 무한**: 미끄러짐 가능성 무시

### 확장 기법

#### Capture Point
- **정의**: 현재 상태에서 정지할 수 있는 발 위치
- **동적 안정성**: ZMP보다 동적 특성 반영
- **수식**: CP = CoM + CoM_velocity / ω₀

#### 중심압력점 (Center of Pressure, CoP)
- **차이점**: 실제 측정되는 압력 중심
- **관계**: ZMP는 이론적, CoP는 실측값
- **활용**: ZMP와 CoP 차이로 시스템 상태 평가

## 실험 및 검증

### 성능 지표
- **ZMP 추적 오차**: RMS 오차, 최대 오차
- **안정성 마진**: 평균 마진, 최소 마진
- **에너지 효율**: 단위 거리당 에너지 소비
- **보행 속도**: 최대 안정 보행 속도

### 실험 프로토콜
1. **정지 상태**: ZMP 위치 정확도 측정
2. **저속 보행**: ZMP 추적 성능 평가
3. **고속 보행**: 안정성 한계 측정
4. **외란 테스트**: 밀기, 미끄러짐 대응
5. **지형 테스트**: 경사, 계단, 불규칙 지형

### 시뮬레이션 검증
- **물리 엔진**: 정확한 접촉 모델링
- **센서 모델**: 노이즈, 지연, 오차 모델
- **Monte Carlo**: 다양한 조건에서 통계적 검증

## 관련 연구

### 초기 연구 (1970-1990)
- **Vukobratović**: ZMP 개념 최초 제안
- **기본 이론**: 정적 안정성 기준 확립

### 발전기 (1990-2010)  
- **Honda**: ASIMO의 ZMP 기반 보행
- **실용화**: 실제 휴머노이드에 적용

### 현대 연구 (2010-현재)
- **최적화**: MPC 기반 ZMP 제어
- **학습**: 강화학습과 ZMP 결합
- **동적 보행**: ZMP 한계 극복 연구

## 코드 예제

### 기본 ZMP 계산
```python
import numpy as np

def calculate_zmp(masses, positions, accelerations):
    """
    질량, 위치, 가속도로부터 ZMP 계산
    """
    g = 9.81  # 중력가속도
    
    numerator_x = 0
    numerator_y = 0  
    denominator = 0
    
    for m, pos, acc in zip(masses, positions, accelerations):
        force_z = m * (g + acc[2])  # 수직력
        moment_x = m * (acc[1] * pos[2] - (g + acc[2]) * pos[1])
        moment_y = m * ((g + acc[2]) * pos[0] - acc[0] * pos[2])
        
        numerator_x += moment_y
        numerator_y += moment_x
        denominator += force_z
    
    zmp_x = numerator_x / denominator
    zmp_y = numerator_y / denominator
    
    return np.array([zmp_x, zmp_y])
```

### ZMP 추적 제어기
```python
class ZMPTracker:
    def __init__(self, kp=100, ki=10, kd=5):
        self.kp = kp
        self.ki = ki  
        self.kd = kd
        self.integral_error = np.zeros(2)
        self.previous_error = np.zeros(2)
        
    def control(self, zmp_desired, zmp_actual, dt):
        error = zmp_desired - zmp_actual
        
        # PID 제어
        self.integral_error += error * dt
        derivative_error = (error - self.previous_error) / dt
        
        com_correction = (self.kp * error + 
                         self.ki * self.integral_error +
                         self.kd * derivative_error)
        
        self.previous_error = error.copy()
        return com_correction
```

## 연결 문서

### 관련 기술
- **상위**: [[휴머노이드 연구기술]]
- **보행**: [[이족보행]], [[보행패턴생성]]
- **제어**: [[전신제어기법]], [[궤적최적화]]

### 확장 개념
- **동역학**: [[라그랑지안방법]], [[뉴턴오일러방법]]
- **최적화**: [[MPC]], [[비선형최적화]]
- **센서**: [[힘토크센서]], [[IMU센서]]

### 응용 분야
- **휴머노이드**: [[휴머노이드]], [[ASIMO]], [[Atlas]]
- **보행로봇**: [[이족로봇]], [[보행제어]]

## 태그

#ZMP #이족보행 #동적안정성 #보행제어 #질량중심 #지지다각형 #휴머노이드 #로봇제어