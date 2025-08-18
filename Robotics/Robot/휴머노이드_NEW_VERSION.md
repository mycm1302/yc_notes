# 휴머노이드 로보틱스

> 상위: [[분야별 로봇]]

## 목차

1. [개념 및 개요](#1-개념-및-개요)
2. [수학적 기초](#2-수학적-기초)
3. [기구학 (Kinematics)](#3-기구학-kinematics)
4. [Zero-Moment Point와 동역학](#4-zero-moment-point와-동역학)
5. [이족 보행 제어](#5-이족-보행-제어)
6. [전신 운동 생성](#6-전신-운동-생성)
7. [센싱 및 인지](#7-센싱-및-인지)
8. [최신 연구 동향](#8-최신-연구-동향)

---

## 1. 개념 및 개요

### 1.1 기본 정의

휴머노이드 로봇(Humanoid Robot)은 인간과 유사한 형태와 동작을 가지는 로봇으로, 복잡한 기구학적 구조와 동역학적 특성을 가진 고자유도 시스템입니다. 30개 이상의 관절을 가지며, 이족 보행과 전신 제어를 통해 인간과 유사한 작업을 수행할 수 있습니다.

### 1.2 주요 특징

#### 기구학적 특성
- **높은 자유도**: 30+ DOF (Degree of Freedom)
- **복잡한 기구학**: 다중 체인 구조 (머리, 몸통, 양팔, 양다리)
- **중복 자유도**: 대부분의 작업에 필요한 것보다 많은 관절
- **인간 유사 구조**: 어깨, 팔꿈치, 손목, 고관절, 무릎, 발목

#### 동역학적 특성
- **비선형 동역학**: 고차원, 비선형, 비볼록 시스템
- **결합 동역학**: 관절 간 상호작용
- **동적 불안정성**: 이족 보행으로 인한 본질적 불안정성
- **중력 의존성**: 항상 중력의 영향을 받는 시스템

#### 제어적 특성
- **다중 태스크**: 균형, 보행, 조작을 동시에 수행
- **계층적 제어**: 우선순위 기반 태스크 할당
- **실시간 제어**: 수십 Hz~수 kHz의 제어 주기
- **적응성**: 환경 변화와 외란에 대한 대응

### 1.3 연구 역사와 현황

#### 초기 연구 (1970-1990)
- **1970년대**: Vukobratović의 ZMP 개념 제안
- **기본 이론**: 정적 안정성 기준 확립
- **이론적 토대**: 이족 보행의 수학적 기반 마련

#### 발전기 (1990-2010)
- **Honda ASIMO**: 최초의 실용적 휴머노이드
- **Sony QRIO**: 소형 엔터테인먼트 로봇
- **실용화**: ZMP 기반 보행 제어의 실제 적용

#### 현대 연구 (2010-현재)
- **Boston Dynamics Atlas**: 동적 보행과 운동성
- **AI 융합**: 강화학습과 로봇 제어 결합
- **상용화**: 서비스 로봇, 의료 보조 로봇

### 1.4 주요 도전과제

#### 기술적 도전
- **동적 안정성**: 이족 보행의 본질적 불안정성
- **계산 복잡성**: 실시간 최적화와 제어
- **센서 융합**: 다양한 센서 정보의 통합
- **에너지 효율**: 장시간 동작을 위한 배터리 관리

#### 응용 분야
- **서비스 로봇**: 가정, 병원, 호텔에서의 서비스
- **재난 구조**: 위험 지역에서의 인명 구조
- **산업 응용**: 제조업에서의 협업 로봇
- **연구 플랫폼**: 인간 운동 분석 및 의학 연구

---

## 2. 수학적 기초

### 2.1 선형대수 및 기하학

#### 벡터와 행렬
```
벡터: v = [v₁, v₂, v₃]ᵀ
행렬: M = [m₁₁ m₁₂ m₁₃]
          [m₂₁ m₂₂ m₂₃]
          [m₃₁ m₃₂ m₃₃]
```

#### 좌표계 변환
- **동차 좌표**: 회전과 평행이동 통합 표현
- **변환 행렬**: 4×4 동차 변환 행렬
```
T = [R t]
    [0 1]
여기서 R: 3×3 회전행렬, t: 3×1 평행이동 벡터
```

### 2.2 회전 표현

#### 회전 행렬 (Rotation Matrix)
```
Rx(θ) = [1    0      0   ]
        [0  cos θ -sin θ]
        [0  sin θ  cos θ]

Ry(θ) = [ cos θ  0  sin θ]
        [   0    1    0  ]
        [-sin θ  0  cos θ]

Rz(θ) = [cos θ -sin θ  0]
        [sin θ  cos θ  0]
        [  0      0    1]
```

#### 오일러 각 (Euler Angles)
- **ZYX 순서**: 요(Yaw) → 피치(Pitch) → 롤(Roll)
- **특이점**: 짐벌 락 현상
- **범위**: φ ∈ [-π, π], θ ∈ [-π/2, π/2], ψ ∈ [-π, π]

#### 사원수 (Quaternion)
```
q = q₀ + q₁i + q₂j + q₃k
||q|| = q₀² + q₁² + q₂² + q₃² = 1

회전행렬 변환:
R = [1-2(q₂²+q₃²)  2(q₁q₂-q₀q₃)  2(q₁q₃+q₀q₂)]
    [2(q₁q₂+q₀q₃)  1-2(q₁²+q₃²)  2(q₂q₃-q₀q₁)]
    [2(q₁q₃-q₀q₂)  2(q₂q₃+q₀q₁)  1-2(q₁²+q₂²)]
```

### 2.3 미분방정식과 동역학

#### 라그랑지안 역학
```
L = T - V (운동에너지 - 위치에너지)

오일러-라그랑주 방정식:
d/dt(∂L/∂q̇ᵢ) - ∂L/∂qᵢ = τᵢ

여기서:
- T: 운동에너지
- V: 위치에너지  
- qᵢ: 일반화된 좌표
- τᵢ: 일반화된 힘/토크
```

#### 뉴턴-오일러 방법
```
선형 운동: F = ma
각운동: τ = Iω̇ + ω × Iω

여기서:
- F: 힘, m: 질량, a: 가속도
- τ: 토크, I: 관성텐서, ω: 각속도
```

---

## 3. 기구학 (Kinematics)

### 3.1 순기구학 (Forward Kinematics)

#### 정의와 목적
순기구학은 관절 각도가 주어졌을 때 엔드 이펙터의 위치와 방향을 계산하는 기법입니다.

```
x = f(q)
여기서:
- q: 관절 각도 벡터 [q₁, q₂, ..., qₙ]ᵀ
- x: 엔드 이펙터 위치/방향 [x, y, z, roll, pitch, yaw]ᵀ
```

#### DH Parameters
Denavit-Hartenberg 매개변수를 사용한 체계적 접근:

| 관절 | aᵢ₋₁ | αᵢ₋₁ | dᵢ | θᵢ |
|------|------|------|----|----|
| 1    | 0    | 0    | d₁ | θ₁ |
| 2    | a₁   | α₁   | d₂ | θ₂ |
| ...  | ...  | ...  | ...| ...|

```
변환 행렬:
Tᵢ = Rot(z,θᵢ) × Trans(z,dᵢ) × Trans(x,aᵢ) × Rot(x,αᵢ)

전체 변환:
T₀ⁿ = T₁ × T₂ × ... × Tₙ
```

### 3.2 역기구학 (Inverse Kinematics)

#### 문제 정의
주어진 엔드 이펙터 목표 위치/방향에서 필요한 관절 각도를 계산:

```
q = f⁻¹(x)
```

#### 해석적 방법

**2링크 로봇팔 예시:**
```python
def inverse_kinematics_2link(x, y, L1, L2):
    # 목표점까지의 거리
    r = sqrt(x² + y²)
    
    # 코사인 법칙
    cos_θ2 = (r² - L1² - L2²) / (2×L1×L2)
    
    if |cos_θ2| > 1:
        return "해가 없음"
    
    # 두 가지 해 (elbow up/down)
    θ2 = ±arccos(cos_θ2)
    θ1 = atan2(y,x) ∓ arccos((L1² + r² - L2²)/(2×L1×r))
    
    return (θ1, θ2)
```

#### 수치적 방법

**뉴턴-랩슨 방법:**
```
반복식: qₖ₊₁ = qₖ + J⁻¹(qₖ)[x_target - f(qₖ)]

여기서:
- J: 자코비안 행렬
- α: 스텝 크기 (0 < α ≤ 1)
```

### 3.3 자코비안 (Jacobian)

#### 정의
자코비안은 관절 속도와 엔드 이펙터 속도 사이의 관계를 나타내는 행렬입니다.

```
J = ∂f/∂q = [∂x/∂q₁  ∂x/∂q₂  ...  ∂x/∂qₙ]

속도 관계: ẋ = J(q) × q̇
```

#### 기하학적 자코비안
```python
def geometric_jacobian(q, joint_positions, joint_axes):
    """기하학적 자코비안 계산"""
    J = zeros(6, len(q))
    
    for i in range(len(q)):
        if joint_type[i] == 'revolute':
            # 회전 관절
            J[0:3, i] = cross(joint_axes[i], end_effector_pos - joint_positions[i])
            J[3:6, i] = joint_axes[i]
        elif joint_type[i] == 'prismatic':
            # 이동 관절
            J[0:3, i] = joint_axes[i]
            J[3:6, i] = zeros(3)
    
    return J
```

#### 특이점 (Singularity)
- **발생 조건**: det(J) = 0
- **문제점**: 역자코비안이 존재하지 않음
- **해결책**: 감쇠 최소제곱법 (Damped Least Squares)

```
J_damped = J^T(JJ^T + λI)⁻¹
여기서 λ: 감쇠 상수
```

### 3.4 중복 자유도 (Redundancy)

#### Null Space 투영
7-DOF 팔으로 6-DOF 태스크 수행 시:

```python
def redundant_ik(target_pose, current_q, secondary_task):
    J = compute_jacobian(current_q)
    J_pinv = pinv(J)
    
    # 주 태스크
    error = target_pose - forward_kinematics(current_q)
    dq_primary = J_pinv @ error
    
    # 부차 태스크 (null space에서)
    N = eye(len(current_q)) - J_pinv @ J
    dq_secondary = N @ secondary_task_gradient
    
    return dq_primary + dq_secondary
```

---

## 4. Zero-Moment Point와 동역학

### 4.1 ZMP 정의와 개념

#### 수학적 정의
ZMP(Zero Moment Point)는 지면과의 접촉점에서 수평 방향 모멘트의 합이 0이 되는 점입니다.

```
ZMP 조건: Σ M_horizontal = 0

좌표 계산:
x_zmp = Σ(m_i × g × x_i - m_i × ẍ_i × z_i) / Σ(m_i × g)
y_zmp = Σ(m_i × g × y_i - m_i × ÿ_i × z_i) / Σ(m_i × g)

여기서:
- m_i: i번째 링크 질량
- g: 중력 가속도  
- (x_i, y_i, z_i): i번째 링크 위치
- (ẍ_i, ÿ_i): i번째 링크 수평 가속도
```

#### 물리적 의미
- **안정 조건**: ZMP ∈ Support Polygon
- **불안정**: ZMP가 지지 영역을 벗어남
- **임계점**: ZMP가 지지 영역 경계에 위치

### 4.2 ZMP 기반 보행 제어

#### Cart-Table Model
```
단순화된 모델:
ẍ_com = (g/h) × (x_com - x_zmp)

여기서:
- x_com: 질량중심 위치
- h: 질량중심 높이 (상수)
- x_zmp: ZMP 위치 (제어 입력)
```

#### Linear Inverted Pendulum Model (LIPM)
```python
class LIPM:
    def __init__(self, height, gravity=9.81):
        self.h = height
        self.g = gravity
        self.omega = sqrt(gravity / height)
    
    def predict_com(self, x0, v0, zmp, time):
        """질량중심 위치 예측"""
        exp_term = exp(self.omega * time)
        
        x = (x0 - zmp) * exp_term + (v0/self.omega + zmp - x0) / exp_term + zmp
        v = (x0 - zmp) * self.omega * exp_term - (v0/self.omega + zmp - x0) * self.omega / exp_term
        
        return x, v
```

### 4.3 동역학 방정식

#### 라그랑지안 접근
```
휴머노이드의 운동방정식:
M(q)q̈ + C(q,q̇)q̇ + G(q) = τ + J^T F_ext

여기서:
- M(q): 관성 행렬 (n×n)
- C(q,q̇): 코리올리/원심력 행렬 (n×n)  
- G(q): 중력 벡터 (n×1)
- τ: 관절 토크 (n×1)
- J^T: 자코비안 전치
- F_ext: 외력 (6×1)
```

#### 뉴턴-오일러 접근
순방향-역방향 재귀 알고리즘:

```python
def newton_euler_dynamics(q, qd, qdd, f_ext):
    """뉴턴-오일러 동역학 계산"""
    n = len(q)
    
    # 순방향 재귀 (속도, 가속도 전파)
    for i in range(n):
        # 링크 i의 속도와 가속도 계산
        v[i] = update_velocity(v[i-1], q[i], qd[i])
        a[i] = update_acceleration(a[i-1], q[i], qd[i], qdd[i])
    
    # 역방향 재귀 (힘, 토크 계산)
    for i in range(n-1, -1, -1):
        # 링크 i에 작용하는 힘과 모멘트
        f[i], tau[i] = compute_link_forces(m[i], I[i], a[i], f[i+1])
    
    return tau
```

### 4.4 ZMP 추적 제어

#### PID 제어기
```python
class ZMPController:
    def __init__(self, kp=100, ki=10, kd=5):
        self.kp, self.ki, self.kd = kp, ki, kd
        self.error_integral = 0
        self.prev_error = 0
        
    def control(self, zmp_desired, zmp_actual, dt):
        error = zmp_desired - zmp_actual
        
        self.error_integral += error * dt
        error_derivative = (error - self.prev_error) / dt
        
        com_correction = (self.kp * error + 
                         self.ki * self.error_integral +
                         self.kd * error_derivative)
        
        self.prev_error = error
        return com_correction
```

#### 모델 예측 제어 (MPC)
```
목적함수:
J = Σ(||zmp_pred[k] - zmp_ref[k]||² + R||u[k]||²)

제약조건:
- zmp ∈ Support Polygon
- |com_acceleration| ≤ a_max
- 관절 한계: q_min ≤ q ≤ q_max
```

---

## 5. 이족 보행 제어

### 5.1 보행 패턴 생성

#### 보행 주기 분석
```
보행 주기 = 이중 지지 + 단일 지지 + 이중 지지 + 단일 지지

이중 지지 (Double Support): 양발이 지면에 접촉
단일 지지 (Single Support): 한 발만 지면에 접촉
```

#### 궤적 계획
```python
class GaitPlanner:
    def __init__(self, step_length, step_height, step_time):
        self.step_length = step_length
        self.step_height = step_height  
        self.step_time = step_time
        
    def generate_foot_trajectory(self, start_pos, end_pos, phase):
        """발 궤적 생성 (3차 스플라인)"""
        if phase < 0.5:  # 들어올리기
            height = self.step_height * sin(pi * phase * 2)
            x = start_pos[0] + (end_pos[0] - start_pos[0]) * phase * 2
        else:  # 내려놓기
            height = self.step_height * sin(pi * (phase - 0.5) * 2)
            x = start_pos[0] + (end_pos[0] - start_pos[0]) * (phase * 2 - 1)
            
        return [x, start_pos[1], height]
```

### 5.2 안정성 분석

#### Capture Point
동적 안정성을 위한 확장된 개념:

```
Capture Point: ξ = x_com + ẋ_com/ω₀

여기서:
- ω₀ = √(g/h): 고유 진동수
- 현재 상태에서 안정하게 정지할 수 있는 발 위치
```

#### 안정성 마진
```python
def stability_margin(zmp, support_polygon):
    """안정성 마진 계산"""
    distances = []
    
    for edge in support_polygon.edges:
        dist = point_to_line_distance(zmp, edge)
        distances.append(dist)
    
    return min(distances)  # 경계까지의 최소 거리
```

### 5.3 동적 보행

#### 자연 동역학 활용
```
Passive Dynamic Walking의 원리를 휴머노이드에 적용:
- 중력과 관성을 이용한 에너지 효율적 보행
- 최소한의 제어 입력으로 안정적 보행
- 언덕 보행에서 영감을 얻은 제어 기법
```

#### Limit Cycle Walking
```python
class LimitCycleController:
    def __init__(self, desired_speed):
        self.v_desired = desired_speed
        self.step_adjustment_gain = 0.1
        
    def compute_step_length(self, current_speed, terrain_slope):
        """스텝 길이 적응적 조절"""
        speed_error = self.v_desired - current_speed
        
        # 기본 스텝 길이 + 속도 보정 + 지형 보정
        step_length = (self.nominal_step + 
                      self.step_adjustment_gain * speed_error +
                      self.slope_compensation(terrain_slope))
        
        return step_length
```

---

## 6. 전신 운동 생성

### 6.1 다중 태스크 제어

#### 계층적 제어 (Hierarchical Control)
```
우선순위 기반 태스크 할당:
1순위: 균형 유지 (ZMP 제약)
2순위: 발 위치 제어 (보행)
3순위: 상체 태스크 (물체 조작)

수학적 표현:
q̇ = J₁⁺ẋ₁ + N₁J₂⁺ẋ₂ + N₁N₂J₃⁺ẋ₃

여기서:
- J_i⁺: i번째 태스크의 의사역행렬
- N_i: i번째 태스크의 null space 투영자
```

#### 구현 예시
```python
class HierarchicalController:
    def compute_joint_velocities(self, tasks):
        q_dot = zeros(self.n_joints)
        N = eye(self.n_joints)  # 누적 null space
        
        for task in sorted(tasks, key=lambda t: t.priority):
            J = task.compute_jacobian(self.current_q)
            
            # 현재 null space에서의 자코비안
            J_projected = J @ N
            
            if rank(J_projected) > 0:
                J_pinv = pinv(J_projected)
                
                # 태스크 오차
                error = task.compute_error(self.current_q)
                
                # 이 태스크의 기여분
                q_dot += N @ J_pinv @ error
                
                # null space 업데이트
                N = N @ (eye(self.n_joints) - J_pinv @ J_projected)
        
        return q_dot
```

### 6.2 운동 계획

#### 궤적 최적화
```
최적화 문제:
minimize: ∫[τᵀQτ + ẍᵀRẍ]dt

제약조건:
- 동역학 방정식: M(q)q̈ + C(q,q̇)q̇ + G(q) = τ
- 관절 한계: q_min ≤ q ≤ q_max
- ZMP 제약: zmp ∈ Support Polygon
- 충돌 회피: d(q) ≥ d_safe
```

#### 실시간 궤적 수정
```python
class TrajectoryOptimizer:
    def __init__(self, horizon_length, dt):
        self.N = horizon_length
        self.dt = dt
        
    def optimize_trajectory(self, current_state, target_states):
        """호라이즌 내 궤적 최적화"""
        def objective(x):
            # x = [q₀, q₁, ..., q_N, τ₀, τ₁, ..., τ_{N-1}]
            cost = 0
            
            for i in range(self.N):
                q_i = x[i*self.n_q:(i+1)*self.n_q]
                
                if i < self.N-1:
                    tau_i = x[self.N*self.n_q + i*self.n_q:(self.N*self.n_q + (i+1)*self.n_q)]
                    cost += self.energy_cost(tau_i)
                
                cost += self.tracking_cost(q_i, target_states[i])
            
            return cost
        
        # 제약조건과 함께 최적화
        result = minimize(objective, initial_guess, 
                         constraints=self.constraints,
                         bounds=self.bounds)
        
        return result.x
```

### 6.3 제약 조건 처리

#### 물리적 제약
```python
class ConstraintHandler:
    def __init__(self, robot_model):
        self.robot = robot_model
        
    def zmp_constraint(self, q, qd, qdd):
        """ZMP 제약 검사"""
        zmp = self.compute_zmp(q, qd, qdd)
        support_polygon = self.get_support_polygon()
        
        return self.point_in_polygon(zmp, support_polygon)
    
    def joint_limit_constraint(self, q):
        """관절 한계 제약"""
        return all(self.q_min <= q) and all(q <= self.q_max)
    
    def collision_constraint(self, q):
        """충돌 회피 제약"""
        min_distance = self.compute_minimum_distance(q)
        return min_distance >= self.safety_margin
```

---

## 7. 센싱 및 인지

### 7.1 센서 시스템

#### 관성 측정 장치 (IMU)
```
센서 구성:
- 3축 가속도계: 선형 가속도 측정
- 3축 자이로스코프: 각속도 측정  
- 3축 자력계: 지자기 방향 측정

센서 융합:
- 상보 필터: 고주파 자이로 + 저주파 가속도계
- 칼만 필터: 확률적 센서 융합
```

#### 시각 센서
```python
class VisionSystem:
    def __init__(self):
        self.rgb_camera = Camera(resolution=(640, 480))
        self.depth_camera = DepthCamera()
        
    def detect_obstacles(self, rgb_image, depth_image):
        """장애물 감지"""
        # 영상 전처리
        edges = cv2.Canny(rgb_image, 50, 150)
        
        # 깊이 정보와 결합
        obstacles = []
        for contour in cv2.findContours(edges):
            depth = np.mean(depth_image[contour_mask])
            obstacles.append({
                'position': self.pixel_to_world(contour.center, depth),
                'size': contour.area
            })
        
        return obstacles
```

### 7.2 상태 추정

#### 확장 칼만 필터 (EKF)
```python
class ExtendedKalmanFilter:
    def __init__(self, dim_state, dim_observation):
        self.n = dim_state
        self.m = dim_observation
        
        # 상태: [위치, 속도, 자세, 각속도]
        self.x = zeros(self.n)  # 상태 벡터
        self.P = eye(self.n)    # 공분산 행렬
        
    def predict(self, u, dt):
        """예측 단계"""
        # 상태 전이 모델
        self.x = self.state_transition(self.x, u, dt)
        
        # 자코비안 계산
        F = self.compute_jacobian_f(self.x, u, dt)
        
        # 공분산 예측
        self.P = F @ self.P @ F.T + self.Q
        
    def update(self, z):
        """업데이트 단계"""
        # 관측 모델
        h = self.observation_model(self.x)
        
        # 자코비안 계산
        H = self.compute_jacobian_h(self.x)
        
        # 칼만 게인
        S = H @ self.P @ H.T + self.R
        K = self.P @ H.T @ inv(S)
        
        # 상태 업데이트
        self.x = self.x + K @ (z - h)
        self.P = (eye(self.n) - K @ H) @ self.P
```

#### 파티클 필터
```python
class ParticleFilter:
    def __init__(self, n_particles, state_dim):
        self.N = n_particles
        self.particles = random.normal(size=(n_particles, state_dim))
        self.weights = ones(n_particles) / n_particles
        
    def predict(self, motion_model, control_input):
        """예측 단계"""
        for i in range(self.N):
            self.particles[i] = motion_model(self.particles[i], control_input)
            
    def update(self, observation, observation_model):
        """업데이트 단계"""
        for i in range(self.N):
            likelihood = observation_model(self.particles[i], observation)
            self.weights[i] *= likelihood
            
        # 정규화
        self.weights /= sum(self.weights)
        
        # 리샘플링
        if self.effective_sample_size() < self.N / 2:
            self.resample()
```

### 7.3 SLAM (동시 위치추정 및 지도작성)

#### 시각적 SLAM
```python
class VisualSLAM:
    def __init__(self):
        self.keyframes = []
        self.landmarks = []
        self.pose_graph = Graph()
        
    def process_frame(self, image):
        """프레임 처리"""
        # 특징점 추출
        keypoints = self.extract_features(image)
        
        # 특징점 매칭
        matches = self.match_features(keypoints, self.current_keypoints)
        
        # 모션 추정
        R, t = self.estimate_motion(matches)
        
        # 번들 조정
        if len(self.keyframes) % 10 == 0:
            self.bundle_adjustment()
        
        # 루프 클로져 감지
        loop_closure = self.detect_loop_closure(image)
        if loop_closure:
            self.optimize_pose_graph()
            
        return self.current_pose
```

---

## 8. 최신 연구 동향

### 8.1 AI 융합 기술

#### 강화학습 기반 제어
```python
class HumanoidRLAgent:
    def __init__(self, state_dim, action_dim):
        self.policy_network = PolicyNetwork(state_dim, action_dim)
        self.value_network = ValueNetwork(state_dim)
        
    def get_action(self, state):
        """정책에 따른 행동 선택"""
        mean, std = self.policy_network(state)
        action = normal(mean, std)
        return action
        
    def update(self, trajectory):
        """경험으로부터 학습"""
        # PPO 업데이트
        advantages = self.compute_advantages(trajectory)
        
        for epoch in range(self.update_epochs):
            policy_loss = self.compute_policy_loss(trajectory, advantages)
            value_loss = self.compute_value_loss(trajectory)
            
            total_loss = policy_loss + value_loss
            total_loss.backward()
            self.optimizer.step()
```

#### 모방 학습
```
데이터 소스:
- 인간 모션 캡처 데이터
- 전문가 시연 데이터
- 시뮬레이션 데이터

학습 방법:
- Behavior Cloning: 지도학습 방식
- DAgger: 대화형 모방학습
- GAIL: 생성적 적대 모방학습
```

### 8.2 2024-2025 기술 동향

#### 멀티모달 센싱
```
센서 융합 아키텍처:
- Vision-Language Models
- Audio-Visual Processing  
- Tactile-Visual Integration
- IMU-Vision-LiDAR Fusion
```

#### 엣지 AI 최적화
```python
class EdgeOptimizedController:
    def __init__(self):
        # 경량화된 신경망
        self.lightweight_network = MobileNetV3(num_classes=action_dim)
        
        # 양자화 적용
        self.quantized_model = torch.quantization.quantize_dynamic(
            self.lightweight_network, {torch.nn.Linear}, dtype=torch.qint8
        )
        
    def real_time_inference(self, sensor_data):
        """실시간 추론 (< 10ms)"""
        with torch.no_grad():
            action = self.quantized_model(sensor_data)
        return action
```

### 8.3 미래 연구 방향

#### 사회적 로봇
- **Human-Robot Interaction**: 자연스러운 상호작용
- **감정 인식**: 표정과 음성 분석
- **개인화**: 사용자별 적응적 행동

#### 자율 학습
- **Self-Supervised Learning**: 라벨 없는 데이터 활용
- **Meta-Learning**: 빠른 새 태스크 학습
- **Continual Learning**: 지속적 학습과 망각 방지

#### 안전성과 신뢰성
- **Formal Verification**: 형식적 안전성 검증
- **Robust Control**: 불확실성에 강건한 제어
- **Explainable AI**: 설명 가능한 의사결정

---

## 참고문헌

### 주요 교재
1. Kajita, S., et al. "Introduction to Humanoid Robotics" (2014)
2. Siciliano, B., et al. "Robotics: Modelling, Planning and Control" (2009)
3. Spong, M., et al. "Robot Modeling and Control" (2005)

### 핵심 논문
1. Vukobratović, M. "Zero-Moment Point - Thirty Five Years of its Life" (2004)
2. Pratt, J., et al. "Capture Point: A Step toward Humanoid Push Recovery" (2006)
3. Englsberger, J., et al. "Three-Dimensional Bipedal Walking Control Using Divergent Component of Motion" (2015)

### 최신 연구
1. Peng, X.B., et al. "DeepMimic: Example-Guided Deep Reinforcement Learning of Physics-Based Character Skills" (2018)
2. Kumar, A., et al. "RMA: Rapid Motor Adaptation for Legged Robots" (2021)
3. Li, Z., et al. "Learning Quadrupedal Locomotion over Challenging Terrain" (2020)

---

## 부록

### 주요 기호 정리
- **q**: 관절 각도 벡터
- **x**: 엔드 이펙터 위치/방향
- **J**: 자코비안 행렬
- **ZMP**: Zero Moment Point
- **CoM**: Center of Mass
- **τ**: 관절 토크
- **M(q)**: 관성 행렬
- **C(q,q̇)**: 코리올리 행렬
- **G(q)**: 중력 벡터

### 좌표계 규약
- **World Frame**: 고정 기준 좌표계
- **Base Frame**: 로봇 몸통 중심
- **End-Effector Frame**: 손목/발목 좌표계
- **Joint Frame**: 각 관절별 로컬 좌표계