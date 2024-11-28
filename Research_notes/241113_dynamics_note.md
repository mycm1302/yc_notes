# 241113 Dynamics 논문 계획서
---

## 연구 개요 (Research Overview)
### 연구 배경 (Background)
<!-- 이 연구를 시작하게 된 배경과 동기를 설명 -->
- 현재 분야의 중요성과 관심도
- 실무/학계에서 겪고 있는 어려움
- 연구의 필요성

### 연구 목적 (Research Objectives)
<!-- 이 연구를 통해 달성하고자 하는 구체적인 목표 -->
- 동역학적 모델링 제시 - 이 로봇을 수학적 해석 가능하도록
- 동역학적 분석 제시 - 동특성 분석을 통해 이 로봇이 잘 동작하는지 검증
  - 워크스페이스 영역 분석
  - 영역에 따른 속도
  - 관절, 앵커 포인트 부하
  - 싱귤러리티
  - 접지 포인트 수 변화에 따른 로드 변화
  - 안정성
  - 경사 각도에 따른 로드 등 변화
  - GRF
  - 발의 충격량
- 기구적 특성은 이 논문에서는 수치적 분석을 위주로.

### 핵심 개념 (Key Concepts)
<!-- 연구에서 다루는 주요 개념과 용어 정의 -->
- 핵심 용어 정의
- 주요 변수 설명
- 연구 범위 설정
 
## 연구 프로세스
### 문제 정의
#### 기존 연구 조사
##### 로봇 동역학
<!-- 선행 연구들의 주요 발견, 방법론을 체계적으로 검토하여 현재 학문적 위치 파악 -->
- 케이블 베이스의 로봇은 별로 없었다.
- 경사를 다닐 수 있는 큰 Sprawling type은 TITAN-XI가 유일

##### 로봇 동역학 분석
- ㅇ
  
##### CDPR
###### 로프
- 케이블 구동 병렬 로봇(CDPR)에서 케이블을 모델링할 때 유한 요소법을 사용하여 질량과 응력에 따라 달라지는 강성을 설명하고 고조파적 자극 하에서 주파수 및 시간 영역 응답을 포착.[^Ferravante2019]
- CDPR에 사용되는 폴리머 케이블은 비선형 신장, 히스테리시스, 크립, 단기 및 장기 회복과 같은 복잡한 동적 특성을 가지고 있으며, 이는 통합 비선형 동적 모델을 사용하여 분석할 수 있습니다.[^Choi2018]
- 케이블 구동 병렬 로봇(CDPR)에서 제안된 장력 공식은 다양한 크기의 엔드 이펙터의 동적 반응에 영향을 미칩니다.[^Baklouti2017]
- 케이블 질량과 탄성은 케이블 구동 병렬 로봇(CDPR)의 정적 및 동적 강성에 상당한 영향을 미칩니다.[^Yuan2014]
- 강철, Dyneema®, 아라미드 등 CDPR에 사용되는 케이블 유형의 진동에 따른 물성치들을 측정 및 분석했고, 적층 제조 응용 분야에서 높은 강성을 갖도록 선택해야 한다는 결론을 내렸다.
- 케이블 구동 병렬 로봇의 최적 케이블 장력은 외부 가속도와 엔드 이펙터에 의해 제한되는 이동 범위에 따라 달라집니다.[^Kieu2021]
- 제안된 레이저 기반 직접 케이블 길이 측정 센서는 케이블 길이의 편차를 감지하고 자동 홈 포즈 재보정을 가능하게 함으로써 케이블 구동 병렬 로봇의 정확도를 향상시킵니다.[^Martin2021]
- 케이블 구동 병렬 로봇을 위한 비전 기반 제어는 정확도와 안정성을 개선하고, 탑재량 대 중량 비율 측면에서 기존 병렬 로봇보다 우수한 성능을 발휘합니다.[^Zake2019]

###### 쉬브


##### 로프 역학
- 로프의 다양한 특성들은 이미 분석되었고, 책으로도 정리되었다.[^rope_handbook]

##### 어센더 로봇
- 우리 연구들
---

#### 기존 연구 한계점 파악
<!-- 기존 연구들의 미흡한 점(gap)과 추가 연구가 필요한 영역을 식별 -->
- 4족 휠레그에 로프라이딩 
  - 전체의 최적 제어를 위해서는 전체 모델이 필수
  - 로프, 휠 타이어 등의 요소들은 종합된 로봇에서 새로운 영향을 줄 가능성이 커서 기존 방식대로 가면 한계가 있을 가능성이 큼
- 로프 연구 한계
- CDPR, 어센더 연구 한계
  - 로프의 특성이 로봇에 영향을 주는데, 이전 연구에서는 바디와 지면에만 영향을 줌. 4족부분에 주는 영향에 대한 해석은 불가
  - 로프 2개로 된건 거의 없음 - CDPR은 로프 3개 이상이 대부분
  - 휠, 풋 접촉으로 인한 영향이 고려되는 연구는 거의 없음 - 대부분 그냥 공중 or 캐스터 접촉
  - 일반 CDPR과 달리 몸체와 지면간의 다리에서 16자유도 발생
- 통합을 통해 발생되는 새로운 문제점과 이를 해결할 연구 필요.
  - 위치 오차 발생
  - 끼임 문제 발생

#### 해결할 문제 명확화
<!-- 연구를 통해 해결하고자 하는 구체적인 문제와 목표를 명확히 설정 -->
- 로프 기반에 외벽을 대상으로 한 다이나믹스 모델 프레임워크 제시
- 요소들이 전체 거동에 주는 영향 파악
- 
- 통합을 통해 파악되는 새로운 문제점 제시 및 분석
---

### 가설 수립
#### 문제 해결 방향 제시
<!-- 파악된 문제를 해결하기 위한 핵심 아이디어와 접근 방식을 제안 -->
#### 이론적 근거 준비
<!-- 제안된 해결 방안의 타당성을 뒷받침할 이론적 배경과 논리를 구축 -->
#### 검증 가능한 형태로 구체화
<!-- 이론적 가설을 측정 가능한 형태의 구체적인 연구 가설로 변환 -->

### 방법론 설계
#### 실험 설계
<!-- 가설을 검증하기 위한 구체적인 실험 절차와 조건을 설계 -->
- 로프와 멀티 시스템의 특성상 완벽한 시뮬레이션 불가능이기 때문에 실험 필수.
- 
#### 검증 방법 결정
<!-- 데이터 수집 및 분석에 사용될 구체적인 방법과 도구를 선정 -->
#### 평가 지표 선정
<!-- 연구 결과의 성공 여부를 판단할 수 있는 객관적인 평가 기준 설정 -->

---

### 실험 및 검증
#### 데이터 수집
<!-- 설계된 실험 계획에 따라 필요한 데이터를 체계적으로 수집 -->
#### 통계적 분석
<!-- 수집된 데이터를 통계적 기법을 활용하여 객관적으로 분석 -->
#### 가설 검증
<!-- 분석 결과를 바탕으로 초기 설정한 연구 가설의 타당성을 검증 -->

---

### 결론 도출
#### 이론적 기여도 정리
<!-- 연구 결과가 학문 분야에 제공하는 새로운 통찰과 가치를 정리 -->
#### 한계점 명시
<!-- 연구의 제한사항과 보완이 필요한 부분을 객관적으로 제시 -->
#### 후속 연구 제안
<!-- 현재 연구를 바탕으로 향후 진행될 수 있는 추가 연구 방향 제시 -->

### Appendix
#### 미사용 파트


<!--
<img src="./example.png" width="300" height="200" alt="이미지 설명">
<img src="./example.png" alt="이미지 설명">
-->


[^LX]: https://www.b2bzincatalog.com/digital/catalog/specin/
[^rope_handbook]: C. Leech, The modelling and analysis of the mechanics of ropes. Springer, 2014.
[^Ferravante2019]: Ferravante, V., Riva, E., Taghavi, M., Braghin, F., & Bock, T. (2019). Dynamic analysis of high precision construction cable-driven parallel robots. Mechanism and Machine Theory. https://doi.org/10.1016/J.MECHMACHTHEORY.2019.01.023. 25 cited.

[^Choi2018]: Choi, S., & Park, K. (2018). Integrated and nonlinear dynamic model of a polymer cable for low-speed cable-driven parallel robots. Microsystem Technologies, 24, 4677-4687. https://doi.org/10.1007/S00542-018-3820-7. 12 cited

[^Baklouti2017]: Baklouti, S., Courteille, E., Caro, S., & Dkhil, M. (2017). Dynamic and Oscillatory Motions of Cable-Driven Parallel Robots Based on a Nonlinear Cable Tension Model. Journal of Mechanisms and Robotics, 9, 061014. https://doi.org/10.1115/1.4038068. 25 cited

[^Yuan2014]: Yuan, H., Courteille, E., & Deblaise, D. (2015). Static and dynamic stiffness analyses of cable-driven parallel robots with non-negligible cable mass and elasticity. Mechanism and Machine Theory, 85, 64-81. https://doi.org/10.1016/J.MECHMACHTHEORY.2014.10.010.

[^Gueners2021]:Gueners, D., Bouzgarrou, B., & Chanal, H. (2021). Cable Behavior Influence on Cable-Driven Parallel Robots Vibrations: Experimental Characterization and Simulation. Journal of Mechanisms and Robotics, 1-54. https://doi.org/10.1115/1.4049978. 6 cited.

[^Zarebidoki2021]: Zarebidoki, M., Dhupia, J., & Xu, W. (2022). A Review of Cable-Driven Parallel Robots: Typical Configurations, Analysis Techniques, and Control Methods. IEEE Robotics & Automation Magazine, 29, 89-106. https://doi.org/10.1109/MRA.2021.3138387. 8 cited

[^Kieu2021]: Kieu, V., & Huang, S. (2021). Dynamic and Wrench-Feasible Workspace Analysis of a Cable-Driven Parallel Robot Considering a Nonlinear Cable Tension Model. Applied Sciences. https://doi.org/10.3390/app12010244.

[^Baklouti2019]:Baklouti, S., Courteille, E., Lemoine, P., & Caro, S. (2019). Vibration reduction of Cable-Driven Parallel Robots through elasto-dynamic model-based control. Mechanism and Machine Theory. https://doi.org/10.1016/J.MECHMACHTHEORY.2019.05.001.15 cited.

[^Martin2021]: Martin, C., Fabritius, M., Stoll, J., & Pott, A. (2021). A Laser-Based Direct Cable Length Measurement Sensor for CDPRs. Robotics, 10, 60. https://doi.org/10.3390/ROBOTICS10020060. 4 cited

[^Zake2019]: Zake, Z., Chaumette, F., Pedemonte, N., & Caro, S. (2019). Vision-Based Control and Stability Analysis of a Cable-Driven Parallel Robot. IEEE Robotics and Automation Letters, 4, 1029-1036. https://doi.org/10.1109/LRA.2019.2893611. 39 cited