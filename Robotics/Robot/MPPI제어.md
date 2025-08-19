# Model Predictive Path Integral Control (MPPI 제어)

## 1. 개요 및 배경

Model Predictive Path Integral (MPPI) Control은 확률적 최적 제어를 위한 샘플링 기반 모델 예측 제어 알고리즘입니다. 2015년 Georgia Tech의 연구진에 의해 처음 개발되어, 비선형 시스템의 복잡한 제어 문제를 해결하기 위한 혁신적인 접근법을 제시했습니다[^1][^2].

### 1.1 기본 원리

MPPI의 핵심 아이디어는 다음과 같습니다:
1. **확률적 샘플링**: 제어 입력을 가우시안 분포에서 수천 개의 궤적으로 샘플링
2. **경로 적분**: 통계물리학의 경로 적분 공식을 사용하여 최적 제어 계산
3. **가중 평균**: 각 궤적의 비용에 따라 가중치를 부여하여 최적 제어 결정
4. **후퇴 지평선**: 첫 번째 제어 입력만 적용하고 다음 스텝에서 재계산

이 방법은 기존 MPC와 달리 미분이 불가능한 비용 함수나 복잡한 비선형 동역학에도 적용 가능합니다.

### 1.2 로봇 제어에서의 필요성

로봇 제어에서 MPPI가 중요한 이유:
- **미분 불필요**: 동역학이나 비용 함수의 미분을 계산할 필요 없음
- **비선형성 대응**: 복잡한 비선형 동역학을 직접 처리 가능
- **장애물 회피**: 불연속적인 장애물 회피 비용 함수 처리 가능
- **불확실성 처리**: 확률적 접근법으로 모델 불확실성과 외란 고려
- **실시간 처리**: GPU 병렬 연산으로 실시간 제어 가능

### 1.3 역사적 발전

- **2015년**: 첫 번째 MPPI 논문 발표 (arXiv:1509.01149)[^1]
- **2016년**: ICRA에서 AutoRally 플랫폼을 통한 실험 결과 발표[^3]
- **2017년**: 비-아핀(non-affine) 동역학에 대한 확장[^4]
- **2018년**: IEEE TRO에 이론적 기초 논문 게재[^2]
- **2021년 이후**: Tube-MPPI, Robust-MPPI, Tsallis-MPPI 등 다양한 변형 개발[^5]

### 1.4 기존 MPC와의 차이점

| 특성 | 전통적 MPC | MPPI |
|------|------------|------|
| 최적화 방법 | 그래디언트 기반 | 샘플링 기반 |
| 미분 요구 | 필요 | 불필요 |
| 비선형성 | 근사화 필요 | 직접 처리 |
| 불연속성 | 처리 어려움 | 자연스럽게 처리 |
| 계산 복잡도 | 낮음 | 높음 (병렬화 가능) |

## 2. 수학적 기초

### 2.1 확률적 최적 제어 문제

MPPI는 다음과 같은 연속시간 확률적 제어 시스템을 고려합니다:

$$dx_t = f(x_t, u_t) dt + \sigma dW_t$$

여기서:
- $x_t \in \mathbb{R}^n$: 시스템 상태
- $u_t \in \mathbb{R}^m$: 제어 입력
- $f(x_t, u_t)$: 드리프트 함수
- $\sigma$: 확산 계수 행렬
- $dW_t$: 브라운 운동

### 2.2 비용 함수

최적화할 비용 함수는 다음과 같이 정의됩니다:

$$J = \mathbb{E}\left[\phi(x_T) + \int_0^T \left(q(x_t) + \frac{1}{2} u_t^T R u_t\right) dt\right]$$

여기서:
- $\phi(x_T)$: 종단 비용
- $q(x_t)$: 상태 의존 러닝 비용
- $R$: 제어 가중 행렬
- $T$: 예측 지평선

### 2.3 경로 적분 공식

MPPI의 핵심인 경로 적분 공식은 다음과 같습니다:

$$u_t^* = \frac{\int u_t e^{-\frac{1}{\lambda}S(x_t, u_t)} \mathcal{D}u}{\int e^{-\frac{1}{\lambda}S(x_t, u_t)} \mathcal{D}u}$$

여기서:
- $S(x_t, u_t)$: 비용-대-이동(cost-to-go) 함수
- $\lambda$: 온도 매개변수
- $\mathcal{D}u$: 경로 측도

### 2.4 이산시간 근사

실제 구현에서는 이산시간 근사를 사용합니다:

$$x_{k+1} = x_k + f(x_k, u_k)\Delta t + \sqrt{\Delta t}\sigma \omega_k$$

여기서 $\omega_k \sim \mathcal{N}(0, I)$는 가우시안 백색 잡음입니다.

## 3. MPPI 알고리즘

### 3.1 기본 알고리즘

MPPI의 기본 알고리즘은 다음과 같습니다:

1. **초기화**: 명목 제어 시퀀스 $\{u_0, u_1, ..., u_{N-1}\}$ 설정
2. **샘플링**: K개의 제어 섭동 $\{\delta u^{(i)}\}_{i=1}^K$ 생성
3. **롤아웃**: 각 샘플에 대해 순방향 시뮬레이션 수행
4. **비용 계산**: 각 궤적의 총 비용 $J^{(i)}$ 계산
5. **가중치 계산**: $w^{(i)} = \exp(-\frac{1}{\lambda}J^{(i)})$
6. **제어 업데이트**: $u_t = \bar{u}_t + \frac{\sum_{i=1}^K w^{(i)} \delta u_t^{(i)}}{\sum_{i=1}^K w^{(i)}}$
7. **적용**: 첫 번째 제어 입력 적용 후 다음 스텝으로

### 3.2 중요도 샘플링

MPPI는 일반화된 중요도 샘플링을 사용하여 성능을 향상시킵니다[^2]:

$$\delta u_t^{(i)} \sim \mathcal{N}(0, \Sigma_t)$$

여기서 $\Sigma_t$는 시간에 따라 변할 수 있는 공분산 행렬입니다.

### 3.3 수렴성과 온도 매개변수

온도 매개변수 $\lambda$는 탐색과 활용의 균형을 조절합니다:
- $\lambda \to 0$: 그리디 선택 (최고 샘플만 선택)
- $\lambda \to \infty$: 균등 평균 (모든 샘플 동등 가중)

## 4. 구현 및 최적화

### 4.1 GPU 병렬화

MPPI의 계산 집약적 특성상 GPU 병렬화가 필수적입니다[^2]:
- **전역 메모리**: 궤적 데이터 저장
- **공유 메모리**: 블록 내 데이터 공유
- **레지스터 메모리**: 스레드별 임시 변수

### 4.2 계산 복잡도

- **시간 복잡도**: O(K × N × n), K: 샘플 수, N: 지평선, n: 상태 차원
- **공간 복잡도**: O(K × N × m), m: 제어 차원
- **병렬화 효율성**: 샘플 간 독립적 계산으로 높은 병렬성

### 4.3 실시간 고려사항

실시간 구현을 위한 주요 고려사항:
- **샘플 수 조정**: 정확도와 계산 시간의 트레이드오프
- **지평선 길이**: 예측 능력과 계산 부하의 균형
- **비동기 처리**: 제어 적용과 다음 계산의 병렬 처리
## 5. MPPI 변형들

### 5.1 Tube-MPPI

Tube-MPPI는 두 계층 최적화 구조를 가집니다[^5]:
- **상위 계층**: 명목 동역학을 사용하는 MPPI
- **하위 계층**: 궤적 추종을 위한 DDP 제어기

이 구조는 모델 불확실성에 대한 강인성을 제공합니다.

### 5.2 Robust-MPPI

Robust-MPPI는 Tube-MPPI의 한계를 극복하기 위해 개발되었습니다[^5]:
- 동역학 표현의 증강을 통한 강인성 향상
- 모델링되지 않은 동역학과 환경 외란에 대한 대응

### 5.3 Tsallis-MPPI

Tsallis-MPPI는 비확장 정보 이론적 측도를 사용합니다[^5]:
- Cross Entropy에서 MPPI까지의 연속체 제공
- 다중 모드 및 변분-스타인 정책 매개화에서 성능 향상

### 5.4 Shield-MPPI

안전이 중요한 응용을 위한 Shield-MPPI[^6]:
- 제어 배리어 함수(Control Barrier Functions) 통합
- 예측되지 않은 외란에 대한 강인성 제공
- 실시간 계획 달성

## 6. 로봇 응용 분야

### 6.1 자율 주행

MPPI는 자율 주행에서 다음과 같이 활용됩니다:
- **경로 계획**: 동적 장애물 회피
- **차선 변경**: 복잡한 교통 상황에서의 안전한 기동
- **주차**: 제한된 공간에서의 정밀 제어

**성과 사례**:
- GT-AutoRally: 오프로드 환경에서 고속 주행[^3]
- 평균 속도 1.68m/s, 에너지 효율성 53% 향상

### 6.2 다족 로봇

4족 로봇에서의 MPPI 적용[^7]:
- **지형 적응**: 불규칙한 지형에서의 안정한 보행
- **장애물 회피**: 복잡한 3D 환경 탐색
- **에너지 최적화**: 보행 패턴 최적화

**ANYmal 로봇 성과**:
- 파쿠르 동작 수행
- 사다리 등반 90% 성공률

### 6.3 무인 항공기 (UAV)

UAV 제어에서의 MPPI 응용[^8]:
- **민첩한 기동**: 44 km/h 속도, 20 m/s² 가속도
- **장애물 회피**: 실시간 경로 재계획
- **에너지 효율**: 배터리 수명 최적화

### 6.4 매니퓰레이터

로봇 암에서의 MPPI 활용:
- **궤적 최적화**: 부드러운 팔 움직임
- **충돌 회피**: 동적 환경에서의 안전한 조작
- **힘 제어**: 접촉 작업에서의 정밀 제어

## 7. ROS2에서의 MPPI 구현

### 7.1 Nav2 MPPI Controller

ROS2 Navigation2 스택에서 MPPI는 로컬 제어기로 구현되어 있습니다[^9]:

```yaml
controller_server:
  ros__parameters:
    controller_plugins: ["FollowPath"]
    FollowPath:
      plugin: "nav2_mppi_controller/MPPIController"
      
      # MPPI 매개변수
      time_steps: 56
      model_dt: 0.05
      batch_size: 2000
      vx_std: 0.2
      vy_std: 0.2
      wz_std: 0.4
      
      # 비용 함수 가중치
      critics: ["ConstraintCritic", "ObstacleCritic", "GoalCritic", "GoalAngleCritic", "PathAlignCritic", "PathFollowCritic", "PathAngleCritic", "PreferForwardCritic"]
      
      # 온도 매개변수
      temperature: 0.3
```

### 7.2 주요 설정 매개변수

- **time_steps**: 예측 지평선 길이
- **model_dt**: 시뮬레이션 시간 스텝
- **batch_size**: 샘플 궤적 수
- **temperature**: 탐색/활용 균형 조절
- **critics**: 사용할 비용 함수들

### 7.3 커스텀 크리틱 구현

사용자 정의 비용 함수 작성 예제:

```cpp
class CustomCritic : public mppi::critics::CriticFunction
{
public:
  void score(CriticData & data) override
  {
    // 비용 계산 로직
    for (size_t i = 0; i < data.costs.size(); ++i) {
      data.costs[i] += calculateCustomCost(data.trajectories[i]);
    }
  }
};
```

## 8. 장점과 한계

### 8.1 주요 장점

**이론적 장점**:
- 미분 불필요: 비연속적, 비미분 가능한 함수 처리
- 일반성: 임의의 동역학과 비용 함수 적용 가능
- 확률적 접근: 불확실성과 외란 자연스럽게 처리
- 병렬화: GPU를 통한 고성능 계산

**실용적 장점**:
- 구현 용이성: 복잡한 미분 계산 불필요
- 유연성: 다양한 제약 조건과 목표 함수 통합 가능
- 실시간성: 적절한 샘플링으로 실시간 제어 가능

### 8.2 주요 한계

**계산적 한계**:
- 높은 계산 비용: 수천 개의 시뮬레이션 필요
- GPU 의존성: 실시간 성능을 위해 GPU 필수
- 메모리 사용량: 대량의 궤적 데이터 저장

**이론적 한계**:
- 수렴 보장 없음: 지역 최적해에 수렴할 수 있음
- 차원 저주: 고차원 제어 공간에서 성능 저하
- 조정 매개변수: 온도, 샘플 수 등 많은 튜닝 필요

## 9. 성능 개선 기법

### 9.1 스마트 샘플링

- **적응적 공분산**: 성능에 따른 샘플링 분산 조정
- **중요도 샘플링**: 좋은 영역에 더 많은 샘플 집중
- **멀티모달 샘플링**: 다중 모드 탐색

### 9.2 워밍업 전략

- **이전 해 활용**: 이전 시간 스텝의 최적해로 초기화
- **계층적 계획**: 거친 계획 후 세밀한 최적화
- **학습 기반 초기화**: 신경망을 통한 초기 추정

### 9.3 하이브리드 접근법

- **MPPI + DDP**: MPPI로 초기화 후 DDP로 세밀 조정
- **MPPI + 학습**: 강화학습과의 결합
- **MPPI + 전역 계획**: 전역 경로 계획과의 통합

## 10. 최신 연구 동향

### 10.1 학습과의 융합

- **신경망 동역학**: 학습된 모델을 MPPI에 통합
- **정책 증류**: MPPI 궤적으로부터 신경망 정책 학습
- **메타 학습**: 다양한 환경에서의 MPPI 매개변수 학습

### 10.2 다중 에이전트 MPPI

- **분산 MPPI**: 다중 로봇 협조 제어
- **게임 이론적 MPPI**: 경쟁적 환경에서의 최적 제어
- **합의 기반 MPPI**: 팀 목표 달성을 위한 협력

### 10.3 안전성 강화

- **제약 처리**: 하드 제약 조건 보장
- **불확실성 정량화**: 신뢰 구간을 가진 제어
- **검증 가능한 MPPI**: 형식적 안전성 보장

## 11. 실습 및 구현 가이드

### 11.1 기본 MPPI 구현

Python을 사용한 간단한 MPPI 구현 예제:

```python
import numpy as np

class SimpleMPPI:
    def __init__(self, dynamics, cost_fn, horizon, num_samples):
        self.dynamics = dynamics
        self.cost_fn = cost_fn
        self.horizon = horizon
        self.num_samples = num_samples
        self.temperature = 1.0
        
    def solve(self, x0, u_nominal):
        # 제어 섭동 샘플링
        noise = np.random.randn(self.num_samples, self.horizon, self.control_dim)
        u_samples = u_nominal + noise * self.noise_std
        
        # 비용 계산
        costs = self.rollout_costs(x0, u_samples)
        
        # 가중치 계산
        weights = np.exp(-costs / self.temperature)
        weights /= np.sum(weights)
        
        # 최적 제어 계산
        u_optimal = np.sum(weights[:, None, None] * u_samples, axis=0)
        
        return u_optimal[0]  # 첫 번째 제어만 반환
```

### 11.2 튜닝 가이드라인

**샘플 수 선택**:
- 시작: 500-1000 샘플
- 실시간 요구사항에 따라 조정
- GPU 메모리 제한 고려

**온도 매개변수**:
- 높은 값: 더 많은 탐색
- 낮은 값: 더 집중적인 활용
- 일반적 범위: 0.1-1.0

**지평선 길이**:
- 시스템 동역학 속도에 따라 결정
- 긴 지평선: 더 나은 예측, 높은 계산 비용
- 일반적 범위: 20-100 스텝

## 12. 참고문헌

[^1]: Williams, G., Wagener, N., Goldfain, B., Drews, P., Rehg, J. M., Boots, B., & Theodorou, E. A. (2015). Model predictive path integral control using covariance variable importance sampling. *arXiv preprint arXiv:1509.01149*. https://arxiv.org/abs/1509.01149

[^2]: Williams, G., Drews, P., Goldfain, B., Rehg, J. M., & Theodorou, E. A. (2017). Model predictive path integral control: From theory to parallel computation. *Journal of Guidance, Control, and Dynamics*, 40(2), 344-357. https://arc.aiaa.org/doi/10.2514/1.G001921

[^3]: Williams, G., Goldfain, B., Drews, P., Rehg, J. M., & Theodorou, E. A. (2016). Aggressive driving with model predictive path integral control. *2016 IEEE International Conference on Robotics and Automation (ICRA)*. https://sites.gatech.edu/acds/mppi/

[^4]: Nagabandi, A., Konidaris, G., & Theodorou, E. (2017). Model predictive path integral control for non-affine dynamics. *2017 IEEE International Conference on Robotics and Automation (ICRA)*.

[^5]: Autonomous Control and Decision Systems Laboratory. (n.d.). Model Predictive Path Integral (MPPI) control. Georgia Institute of Technology. https://sites.gatech.edu/acds/mppi/

[^6]: Yin, J., Zhang, Z., & Theodorou, E. A. (2023). Shield model predictive path integral: A computationally efficient robust MPC approach using control barrier functions. *arXiv preprint arXiv:2302.11719*. https://arxiv.org/abs/2302.11719

[^7]: Hutter, M., et al. (2024). ANYmal parkour: Learning agile navigation for quadrupedal robots. *Science Robotics*.

[^8]: Minarik, M., Lantos, B., & Theodorou, E. A. (2024). Model predictive path integral control for agile unmanned aerial vehicles. *arXiv preprint arXiv:2407.09812*. https://arxiv.org/abs/2407.09812

[^9]: Macenski, S., et al. (2023). The Nav2 project. *IEEE Robotics & Automation Magazine*.

[^10]: Cheng, C. A., et al. (2024). Recent advances in path integral control for trajectory optimization: An overview in theoretical and algorithmic perspectives. *Annual Reviews in Control*, 57, 100950. https://www.sciencedirect.com/science/article/abs/pii/S1367578823000950

---

*이 문서는 MPPI 제어에 대한 포괄적인 개요를 제공합니다. 더 자세한 수학적 유도와 구현 세부사항은 참고문헌을 참조하세요.*