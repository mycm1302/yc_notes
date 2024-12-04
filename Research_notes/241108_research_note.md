# 241108 기존 로봇 조사
<details>
- 고스트 로보틱스 vision60
  - 카탈로그 분석 - 방수 방진 인증말고는 의외로 인증 요소는 업었음.
- Warehouse robot [^case1]
- X-57 Maxwell Overview (NASA) [^X57]
- ARGOS (Active Response Gravity Offload System) (NASA) [^ARGOS]
- Atlas Centaur (Convair) [^AtlasCentaur]

- Tesla Model 3 파워트레인
  - Tesla의 특허 공개 정책으로 많은 기술 자료 확인 가능
  - 모터 설계, 배터리 관리 시스템 등
  - 실제 분해 분석 보고서들도 공개됨
  - 배터리 팩 설계
  - 모터 및 인버터 구조
  - 열관리 시스템
  - 제어 소프트웨어 아키텍처

- SpaceX Falcon 9
  - 발사체 설계의 주요 개념들 공개
  - 엔진 테스트 데이터
  - 착륙 시스템 개발 과정

### 1. NASA JPL의 Ingenuity
공개 수준: 매우 높음
- 접근 가능한 자료:
  * GitHub의 비행 소프트웨어 코드
  * NASA Technical Reports Server(NTRS)의 상세 설계 문서
  * 극한 환경 대응 설계 문서
  * 실제 화성 비행 데이터와 분석
  * 로터 시스템 상세 설계
- 접근 경로:
  * nasa.gov
  * GitHub NASA 저장소
  * 공개 학술 논문
  * NASA 기술 보고서

### 2. MIT Cheetah Robot
공개 수준: 매우 높음
- 접근 가능한 자료:
  * 전체 제어 소프트웨어 (GitHub)
  * 동역학 모델링 상세 문서
  * 실시간 제어 알고리즘
  * 하드웨어 설계 문서
  * 실험 데이터셋
- 접근 경로:
  * MIT 연구실 웹사이트
  * GitHub 저장소
  * 학술 논문
  * ROS 위키

### 3. SpaceX Falcon 9
공개 수준: 높음
- 접근 가능한 자료:
  * NASA 계약 관련 기술 문서
  * 엔진 시스템 기본 설계
  * 발사/착륙 시스템 설계
  * 비행 데이터
  * 안전성 분석 보고서
- 접근 경로:
  * NASA 기술 보고서
  * SpaceX 웹사이트
  * FAA 인증 문서
  * 공개 학술 자료

### 4. Boston Dynamics Spot
공개 수준: 높음
- 접근 가능한 자료:
  * SDK 완전 문서화
  * API 레퍼런스
  * 하드웨어 인터페이스 사양
  * 응용 개발 가이드
  * 예제 코드
- 접근 경로:
  * 공식 개발자 포털
  * GitHub 저장소
  * 기술 문서 사이트
  * 개발자 포럼

### 5. Solar Impulse 2
공개 수준: 높음
- 접근 가능한 자료:
  * 태양전지 시스템 설계
  * 에너지 관리 시스템
  * 구조 설계 문서
  * 비행 데이터
  * 개발 과정 문서
- 접근 경로:
  * 공식 프로젝트 사이트
  * 기술 논문
  * 교육 자료
  * 연구 보고서

### 6. Virgin Hyperloop
공개 수준: 상당함
- 접근 가능한 자료:
  * 기본 시스템 설계
  * 안전성 검증 절차
  * 테스트 트랙 데이터
  * 특허 문서
- 접근 경로:
  * 공식 기술 문서
  * 특허 데이터베이스
  * 규제 제출 문서
  * 연구 논문

### 7. Formula E Gen2
공개 수준: 상당함
- 접근 가능한 자료:
  * 파워트레인 스펙
  * 배터리 시스템 설계
  * 공기역학 데이터
  * 경주 데이터
- 접근 경로:
  * FIA 기술 규정
  * 팀 기술 문서
  * 경기 데이터
  * 연구 논문

### 8. Google Wing
공개 수준: 상당함
- 접근 가능한 자료:
  * 드론 제어 시스템
  * 안전 인증 문서
  * API 문서
  * 운영 데이터
- 접근 경로:
  * 개발자 포털
  * FAA 인증 문서
  * 기술 블로그
  * GitHub 저장소

## 제한적 기술 자료 공개 프로젝트
(이하 프로젝트들은 기본적인 마케팅 자료나 특허 정도만 공개)

- VoltAero Cassio
- Pipistrel Velis Electro
- HondaJet Elite
- Joby Aviation S4
- XCOR Lynx
- Archer Midnight
- Eviation Alice
- Beta Technologies ALIA
- Embraer E2
- Aurora D8
- Universal Hydrogen
- Heart Aerospace ES-19
- Blue Origin New Shepard
- Lilium Jet
- Rimac Nevera
- Hyundai NEXO
- Sono Sion
- Aptera
- ABB YuMi
- Nuro R2
- Voith Inline Thruster
- Volocopter 2X

## 분석 기준
각 프로젝트는 다음 기준으로 평가:
1. 공식 기술 문서 공개 수준
2. 소스 코드/SDK 공개 여부
3. 특허 이상의 상세 설계 공개
4. 실제 운용/테스트 데이터 접근성
5. 개발 과정 문서화 수준

## 자료 접근 방법
1. 공식 문서/웹사이트
2. 학술 데이터베이스
3. 특허 검색
4. 정부/규제기관 문서
5. 오픈소스 저장소
6. 개발자 포럼/커뮤니티

## 주요 발견사항
1. 정부/연구기관 프로젝트가 가장 상세한 기술 공개
2. 안전 인증이 필요한 프로젝트들의 검증 데이터 접근 가능
3. 오픈소스 정책 프로젝트들의 개발자 문서 충실
4. 대부분의 상업 프로젝트는 제한적 공개
5. 교육/연구 목적 프로젝트의 높은 공개성


- Sample Data Sources
  - 기업 기술 블로그
  - 특허 데이터베이스
  - 학술 논문
  - 인증 문서
  - 개발자 컨퍼런스 발표 자료
  - 기업 발표 자료
  - 기술 컨퍼런스 발표
  - 개발자 블로그

- 참고 레퍼런스
  - 항공 학회 논문
    - AIAA (American Institute of Aeronautics and Astronautics)
    - SAE Aerospace 기술 논문들
  - 인증 관련 문서
    - FAA와 EASA의 인증 보고서
    - 특히 새로운 기술 적용시의 특별 조건들(Special Conditions) 참고
  - 특허 문서
    - Google Patents나 특허청 데이터베이스
    - 상세한 기술 구현 방식 확인 가능
  - 제작사 기술 문서
    - 정비 매뉴얼
    - 시스템 설명서
    - 부품 카탈로그

</details>

# 241108 안전공학 관련 정리
<details>
- ​RAMS 분석
  - 신뢰도(Reliability), 가용도(Availability), 정비도(Maintainability), 안전도(Safety)
  - 
</details>

# 241108 건축물 표면 정리
<details>
- 결국 건축물 외벽도 사용품 공사하는 만큼 주요 리스트업 후 카탈로그 참조하는것이 좋을것.
- LX 하우시스[^LX]
- MECE 전략[^MECE]
- 적합한 실험 횟수는 몇번인가: 30번
  - 중심극한정리(Central limit theorem)가 표본크기 30이상에서 성립하기 때문[^CLT]
    - 동일한 확률분포를 가진 독립 확률 변수 n개의 평균의 분포는 n이 적당히 크다면 정규분포에 가까워진다는 정리

- 물리 성질 관련 핸드북 정리
  - ASM Metals Handbook[^ASM]
  - 

- safety critical system[^SCS]
  - The Safety Critical Systems Handbook

- Swiss cheese model[^SCM]

- Tribology   마찰 공학, 윤활 공학	
- robotics handbook에 5. Mechanical Properties 부분 참고.
- Model Predictive Impedance Control
- AGMA 110.03
- Github Engineering Blogs
  - https://github.com/kilimchoi/engineering-blogs
  - https://github.com/crispgm/awesome-engineering-blogs
  - https://github.com/sumodirjo/engineering-blogs
  - https://github.com/androiddevnotes/awesome-google-engineering-blogs
  - https://github.com/exajobs/engineering-blogs-collection
</details>

# 241108 로봇 물리적 특성 정리
<details>
- 버클링
- 응력집중
- 충격
- cavitation 공동현상
- 용착 , Scoring 스코링
- 피팅(Pitting)
- 경마모 영역 (응착 마모), 400N보다 클 경우 가혹마모(severe wear) 영역 (박리 마모)
- 프레팅
- 윤활공학
</details>

# 241108 건물 외벽 정리
<details>
## 주요 건축 요소
- 강화 콘크리트 (철근콘크리트)
  - 고강도 콘크리트와 철근의 조합으로 뛰어난 내구성 제공
  - 압축강도가 높고 시공이 용이함
- 구조용 강재 (Steel)
  - 높은 인장강도와 유연성
  - 철골조 또는 철골철근콘크리트조(SRC)에 사용
  - 가벼우면서도 강한 구조 가능
</details>

## 외벽 시스템
<details>
- 커튼월 (Curtain Wall)
  - 알루미늄 프레임에 유리를 끼운 구조
  - 건물 하중을 지지하지 않는 비내력벽
  - 종류
    - 스틱시스템: 현장에서 조립
    - 유닛시스템: 공장 제작 후 설치

- 외장 마감재
  - 알루미늄 복합패널
  - 금속 패널
  - 석재 클래딩
  - 강화유리
  
## 특수 기술요소:

- 내진 설계
  - 제진장치
  - 면진시스템
  - 튜닝매스댐퍼(TMD)

- 에너지 효율
  - 로이유리(Low-E Glass) 사용
  - 이중외피 시스템
  - 단열재 강화

- 외벽 유지관리
  - BMU(Building Maintenance Unit) 시스템
  - 곤돌라 설치
  - 청소용 트랙 시스템

## 주요 커튼월/외장재 제조사
- LG하우시스
- KCC
- 한화L&C
- 현대알루미늄
- POSCO A&C
  
## 외벽 성능
- 단열성능 (열관류율)
- 기밀성
- 수밀성
- 내풍압성
- 차음성능
- 내화성능
</details>

# 241108 콘크리트 파손 관련 정리
<details>
## 콘크리트 파손 종류
- 콘크리트가 떨어져 나오는 현상을 '박리' 또는 '박락'이라고 합니다.
  - 박리(剝離): 콘크리트 표면이 벗겨지거나 떨어져 나가는 현상
  - 박락(剝落): 콘크리트가 덩어리째 떨어져 나가는 현상
  - 원인
    - 동결융해 작용
    - 철근의 부식 팽창
    - 콘크리트의 노화
    - 시공 불량
    - 외부 충격
## 콘크리트 박리에 대한 AI 모델링
- 물리적 특성 모델링
  - 탄성계수, 밀도, 포아송비 등 콘크리트의 기본 물성치 고려
  - 응력파 전달 속도 계산
  - Boussinesq 해법 기반 응력 분포 계산
- 충격 분석
  - 충격량-운동량 원리를 이용한 충격력 계산
  - 깊이별 응력 분포 분석
  - 박리 발생 가능성 평가
- 시각화
  - 응력 분포 그래프 생성
  - 임계 강도 표시
  - 거리에 따른 응력 변화 분석
## 콘크리트 박리 관련 수식 (AI)
### 충격력(F) 계산
  > F = m(v₁ - v₂)/Δt
    - m: 충돌체 질량, v₁: 충돌 전 속도, v₂: 충돌 후 속도, Δt: 충돌 시간
  - 적용: 외부 충격에 의한 초기 충격력 산정

### 응력파 전파 속도(c)
  > c = √(E/ρ)
    - E: 탄성계수, ρ: 밀도
  - 적용: 콘크리트 내부 응력파 전파 속도 계산

### Boussinesq 해법
  > 수직응력: σz = (3P/2π) * (z³/(r² + z²)^(5/2))
  > 전단응력: τrz = (3P/2π) * (rz²/(r² + z²)^(5/2))
    - P: 집중하중, r: 하중점으로부터 수평거리, z: 깊이
  - 적용: 깊이와 거리에 따른 응력 분포 계산

### 동적 하중 응력
  > σd = DIF × σs
    - σd: 동적응력, DIF: Dynamic Increase Factor, σs: 정적응력
  - 적용: 변형률 속도 효과를 고려한 동적 응력 산정

### Hertz 접촉이론
  > σmax = (1/π) * √(3FE/4R²)
    - F: 충격력, E: 탄성계수, R: 접촉 반경, 
  - 적용: 접촉면에서의 최대 응력 계산

### 박리 판정 조건
  > σt > ft
    - σt: 인장응력, ft: 콘크리트 인장강도
  - 적용: 박리 발생 여부 판정

### 응력파 반사계수(R)
  > R = (Z₂ - Z₁)/(Z₂ + Z₁)
    - Z = ρc: 음향임피던스, ρ: 밀도, c: 응력파 속도
  - 적용: 경계면에서의 응력파 반사 특성 분석

### 균열 진전 에너지
  > G = K²/E
    - G: 에너지 해방률, K: 응력확대계수, E: 탄성계수
  - 적용: 균열 진전에 필요한 에너지 계산

### 주요 고려사항
  - 동적 하중의 특성
  - 응력파의 전파와 반사
  - 재료의 물성
  - 기하학적 조건

# 재료 손상 관련 레퍼런스
- 재료강도학 (Material Strength) 시리즈
  - 기계적 성질, 파괴역학, 피로, 마모 등 기본 개념부터 심화 내용까지 체계적으로 다룹니다

- 트라이볼로지 개론 (Introduction to Tribology)
  - 마찰, 마모, 윤활 등 트라이볼로지 전반을 다룹니다
  - 실제 산업 현장의 사례도 포함되어 있습니다


- 금속재료학 (Metallurgical Engineering)
  - 금속 재료의 부식, 피로, 크리프 등 다양한 손상 메커니즘을 설명합니다
  - 미세구조와 물성의 관계도 자세히 다룹니다

- 재료공학의 이해와 응용 시리즈 (ASM Handbook)
  - 약 3,000페이지 분량의 대작
  - 재료의 물성, 시험방법, 손상분석, 열처리, 표면처리 등 모든 내용을 망라
  - 특히 Volume 11 "Failure Analysis and Prevention"이 손상 메커니즘을 상세히 다룹니다

- Engineering Materials: Mechanical Behavior and Testing
  - 1,500페이지 이상의 종합 교재
  - 재료의 기계적 성질부터 각종 시험법, 손상 사례까지 포함
  - 풍부한 도표와 사진으로 이해하기 쉽게 구성

- 재료손상과 수명평가 (Materials Damage and Life Assessment)
  - 약 2,000페이지 분량
  - 이론적 배경부터 실제 산업현장의 사례연구까지 포함
  - 손상 진단/예측 방법론도 상세히 설명
</details>

# 241112 추가로 이용가능한 AI 조사
<details>

- https://consensus.app - 학술 논문조사 및 요약
- https://www.perplexity.ai/
- Elicit.org
  
- 초기 탐색: Perplexity, Semantic Scholar
- 심화 분석: Elicit, Consensus
- 관계 파악: Connected Papers, Iris.ai
- 추적/관리: Research Rabbit, Litmaps
- 요약/정리: Scholarcy
</details>

# 241113 연구, 개발의 정리
<details>
- 개발: 없는걸 새로 만들어보자
  - 새로운 가치창출
  - 가치 창출이 목적
    - 어떤 새로운 가치를 만들 수 있는가?
    - 사용자에게 어떤 이점을 제공하는가?
    - 실용적 효용이 초점
  
- 연구: 기존의 문제를 해결한다.
  - 기존 방식의 한계점 찾기: 구체적인 한계점이 연구의 동기
    - 기존 한계 극복이 초점
    - 문제 해결이 목적
      - "왜 이런 문제가 있는가?"
      - "어떻게 하면 이 문제를 해결할 수 있는가?"
      - 이상적인 상황에서만 작동한다.
      - 계산 비용이 너무 높다.
      - 특정 조건에서 실패한다.
    - 현장의 불만사항/한계점 수집하기
    - 기존 논문들의 Future Work 섹션 검토
    - 다른 분야의 유사한 문제 해결 방식 참고

- 연구 프로세스
  - 문제 정의
    - 기존 연구 조사
    - 한계점 파악
    - 해결할 문제 명확화
  - 가설 수립
    - 문제 해결 방향 제시
    - 이론적 근거 준비
    - 검증 가능한 형태로 구체화
  - 방법론 설계
    - 실험 설계
    - 검증 방법 결정
    - 평가 지표 선정
  - 실험 및 검증
    - 데이터 수집
    - 통계적 분석
    - 가설 검증
  - 결론 도출
    - 이론적 기여도 정리
    - 한계점 명시
    - 후속 연구 제안
- 개발 프로세스
  - 요구사항 분석
    - 시장/사용자 조사
    - 기능 명세 작성
    - 기술적 제약 파악
  - 솔루션 기획
    - 아키텍처 설계
    - 기술 스택 선정
    - 개발 범위 설정
  - 프로토타입 개발
    - 핵심 기능 구현
    - 사용성 테스트
    - 피드백 수집
  - 반복 개선
    - 기능 추가/개선
    - 성능 최적화
    - 버그 수정
  - 제품화
    - 안정성 확보
    - 문서화
    - 유지보수 계획
</details>

# 241113 로봇 연구 실험 방법론 정리
<details>

## 1. 비교 실험 (Comparative Study)
### 적용 분야
- 제어 알고리즘 성능 비교
- 로봇 동작 정확도 평가
- 에너지 효율성 분석

### 핵심 지표
- 정확도(Position/Force Error)
- 응답 속도(Response Time)
- 안정성(Stability)
- 에너지 효율(Power Consumption)
- 작업 완료 시간(Task Completion Time)

## 2. 시뮬레이션 연구 (Simulation Study)
### 적용 분야
- 위험한 작업 환경 테스트
- 극한 상황 검증
- 다양한 parameter 조합 테스트

### 주요 도구
- Gazebo
- V-REP
- Webots
- MuJoCo
- PyBullet

## 3. 파라메트릭 스터디 (Parametric Study)
### 적용 분야
- 제어기 게인 튜닝
- 기구학적 파라미터 최적화
- 센서 파라미터 조정

### 분석 요소
- 민감도 분석
- 연구 프로세스
  - 문제 정의
    - 기존 연구 조사
    - 한계점 파악
    - 해결할 문제 명확화
  - 가설 수립
    - 문제 해결 방향 제시
    - 이론적 근거 준비
    - 검증 가능한 형태로 구체화
  - 방법론 설계
    - 실험 설계
    - 검증 방법 결정
    - 평가 지표 선정
  - 실험 및 검증
    - 데이터 수집
    - 통계적 분석
    - 가설 검증
  - 결론 도출
    - 이론적 기여도 정리
    - 한계점 명시
    - 후속 연구 제안
- 개발 프로세스
  - 요구사항 분석
    - 시장/사용자 조사
    - 기능 명세 작성
    - 기술적 제약 파악
  - 솔루션 기획
    - 아키텍처 설계
    - 기술 스택 선정
    - 개발 범위 설정
  - 프로토타입 개발
    - 핵심 기능 구현
    - 사용성 테스트
    - 피드백 수집
  - 반복 개선
    - 기능 추가/개선
    - 성능 최적화
    - 버그 수정
  - 제품화
    - 안정성 확보https://github.com/jmz3/Gazebo_cable
- 사용자 보호 장치

## 문서화 필수 요소
### 1. 실험 환경
- 하드웨어 스펙
- 소프트웨어 버전
- 센서 구성
- 캘리브레이션 방법

### 2. 실험 과정
- 초기화 과정
- 실험 순서
- 데이터 처리 방법
- 예외 상황 처리

### 3. 결과 분석
- 통계 처리 방법
- 시각화 도구
- 성능 지표 계산
- 오차 분석
</details>

# 241113 아라미드 로프 내용 정리
<details>
아라미드 판매측에 로프에 대해 정리된 내용 발견 [^Aramid]
</details>

# 241113 다이나믹스에서 한계점 조사

<details>
https://consensus.app/results/?q=로봇의 동역학 해석 연구&lang=ko

Three-Dimensional Dynamic Modeling and Motion Analysis of a Fin-Actuated Robot [^dyn1]
> In summary, most studies of fin-actuated underwater robots have two limitations: 
> 1) mainly focusing on investigating 2-D motions in a horizontal plane or vertical plane, without sufficient consideration in 3-D motions, such as surfacing and spiral motions. 
> 2) Though there are a few works that have considered dynamic modeling in 3-D space [26],[27], [32]–[34], the proposed models are typically validated by limited experiments, without validation in large-scale parameter space.

> In the future work, we will input the ALLS-evaluated motion parameters into a dynamic model-based controller as references of current states of robotic fish [41], for adjusting the oscillation parameters of the robotic fish, and finally realizing flow-aided closed-loop control of its trajectory. 

A dynamical system approach to task-adaptation in physical human–robot interaction [^dyn2] (127 cited)

Data-Driven Dynamic Modeling for a Swimming Robotic Fish[^dyn3] (50 cited)

An overview of dynamic parameter identification of robots[^dyn4]

A modular approach for dynamic modeling of multisegment continuum robots[^dyn5]

Dynamic analysis of high precision construction cable-driven parallel robots [^dyn13]
CDPR에서 케이블 해석
</details>

# 241119 전체 일정 재정립
<details>

## 연구 논문
- 메커니즘 제시 논문 (AutoCon)
- Dynamics 논문
- Trajectory 논문
- Control 논문

## 개발 작업
- Dynamics node debug
- 파트별 모델링
  - 로프
- QP solver 구현
- Gazebo 구현
- Localization node
  - Visual odometry
  - Robot_localization
- State estimation node
- WBC Control node
- Trajectory make node

## 차후 준비
- Arxiv 업로드
- Github CV, 기술 문서 정리
- Github project 공개
- 이력서, 자소

## 기타
- 컨택트 모델

</details>

# 241119 한국기계연구원 세미나
<details>

## 정애누나 세미나
Exoskeletone, 외골격 로봇
Hard/ soft werable type

</details>

# 241126 노즐 대처 설계
<details>

## 로봇 청소 노즐 분실 문제
CSCAM 측에서 노즐 분실 문제 발생.
A-1 ~ A-4까지 부품을 보냈었는데 분실된것으로 보임.
금요일에 공증이라 최대한 빠른 대체 필요.

## 펌프 및 이전 사항에 대한 정리
### 펌프 Profi 195 TST
기존 구매해서 이용했던 펌프 품명 Kranzle사의 Profi 195 TST.
- 허용 압력: 30~170bar = 435.113 ~ 2465.64 psi
- 유량 7.5l/min	= 1.981 gallon/min

### 이전 노즐
- 조립형 구성
  - 고압노즐(팁) 11003-TC
    - Flat fan nozzle: 납작한 부채꼴 형태로 분사하는 노즐
  - Tip Retainer(팁 고정부) CP7890-SSP
  - UniJet Body(본체) 11430-1/4-SS-100
  - 니플(연결부) 1/4M-1/4M
    - Male-Male로 구성된 연결 부품
- 매니폴드쪽 구멍은 NPT 1/4 규격의 탭 구멍 (직경 13.7mm)
  - NPT(National Pipe Thread)

## 새 노즐 선정 - MEG 1/4" 6502
- 노즐 제품명은 종류 + NPT 규격 + 분사각 + Capacity size로 구성.
  - Ex) MEG + 1/4" + 65 + 02
- MEG 라인업
  - 고압력 대상 노즐 라인업.
  - 분사각은 0,5,15,25,40,50,65으로 구성됨.
- 기존 호환 위해 1/4인치 규격.
- 분사각 최대한 넓은 65인치.
- 압력, 유량을 데이터시트 표를 확인해서 capacity size 결정.
- 노즐의 압력은 펌프의 압력의 60~80%로 선정.
  - 60% 이하면 펌프 손상.
- MEG 외에 80, 110도 존재하나 판매처가 없음.
- 이전에도 단일 노즐이 아닌 4개의 부품으로 나눠 설계한것도 이런 이유도 있을것으로 추측됨.
- 선정한것을 빠르게 구매하는게 좋을듯

## CSCAM측 전달 메세지
> 
> 현재 공증 준비에서 대처가 필요한 노즐에 관한 정리사항입니다.​
> 
> [기술적 정리 사항]
이전 노즐의 경우 고압노즐(팁), Tip Retainer(팁 고정부), UniJet Body(본체), 니플(연결부) 4가지로 구성되어 있습니다.
금요일까지 4가지 전부 부품을 구매하는건 무리가 있을것으로 생각되어 기존 부품과 달리 단일 부품으로 대체하도록 새로 선정을 진행했습니다.
그 결과, 노즐이 결합되는 부분의 규격이 NPT 1/4"인 13.7mm 직경의 탭 홀에 맞춰 결합될 수 있는 부품을 선정했습니다.
선정한 부품은 MEG 1/4" 6502입니다.
고압 분사용 노즐 라인업인 MEG 노즐의 플랫 팬 분사 타입에 NPT 1/4인치, 분사각 65도, 허용사이즈 0.2인 노즐입니다.
이걸로 1개 구매하면 현재 분실된 노즐 대용이 가능할것으로 판단됩니다.
>
>[판매 사이트]
MEG 6502 노즐은 현재 2 판매처를 찾았습니다.
연락해서 재고 체크 후 빠르게 받아볼 수 있도록 퀵서비스나 방문배송이 가능한지 문의가 필요합니다.
​https://bestcleanmall.co.kr/goods/view?no=2027​
https://isutool.co.kr/shop/item.php?it_id=12137&NaPm=ct%3Dm3y8x3c0%7Cci%3D9e1e48e1efaafd3048e357033b20c9ccc552f485%7Ctr%3Dslsl%7Csn%3D120967%7Chk%3D3d9afc001f0278a7ed527febf564c1456fad7385​

다음날 업체 연락해보니 6503밖에 없어서 이걸로 구매

밸브 기본은 NPT인데 탭은 PT로 되어있었음. 어댑터 추가구매 (NPT-PT)
https://smartstore.naver.com/woo2441/products/5772488309
</details>

# 241127 연구 논문 접근 방법 정리

<details>

경험적으로 아래처럼 분류할 수 있다.

## 시스템 논문(System Paper) or 플랫폼 논문(Platform Paper)
- 새로운 로봇 시스템이나 플랫폼을 소개
- 다양한 실제 환경(real-world environments)에서의 성능을 검증
- 시스템의 구현 세부사항과 설계 선택을 자세히 설명
- 여러 use case들을 통해 시스템의 실용성과 확장성 시연
- Ex) Boston Dynamics의 Spot
- 단일 알고리즘이나 방법론을 깊이 파고드는 대신, 전체 시스템의 통합적인 성능과 실용성에 초점.
- 중요성
  - 실제 환경에서의 적용 가능성을 보여줌 - 산업계에 직접적인 영향
  - 여러 기술들을 어떻게 효과적으로 통합할 수 있는지 보여줌
  - 실제 구현 과정에서 발생하는 문제들과 해결책을 공유
  - 학술적 깊이는 상대적으로 얕을 수 있지만, 로보틱스 분야의 실질적인 발전을 이끄는 중요한 연구 형태. 이론과 실제의 간극을 메우는 역할.

## Improvement/Enhancement Paper (성능 개선형 연구)
- 기존 방법의 한계점을 명확히 지적하고 이를 개선하는 것이 핵심
- 보통 논문 구조가 "기존 방법 분석 → 문제점 도출 → 개선 방안 제시 → 실험적 검증" 순으로 진행됨
- A/B 테스트 형식의 비교 실험이 매우 중요하며, 통계적 유의성 검증이 필수적
- 개선 정도를 정량화하여 표현하는 것이 중요 (예: "20% 성능 향상", "30% 계산 비용 감소")
- 최근에는 작은 개선이라도 계산 효율성이나 에너지 효율성 측면의 개선을 강조하는 추세
- 예: 로봇 제어 알고리즘의 연산 속도 20% 향상, 물체 인식의 정확도 15% 개선 등

## Novel Method/Methodology Paper (새로운 방법론 제시)
- 기존 패러다임과는 다른 완전히 새로운 접근법을 제시
- 이론적 기반을 탄탄히 하는 것이 매우 중요
- 제안하는 방법의 독창성(novelty)을 명확히 부각시켜야 함
- 실험 섹션에서는 다양한 시나리오에서의 검증이 필요
- 한계점과 향후 연구 방향을 명확히 제시하는 것이 중요
- 종종 새로운 연구 분야를 여는 시발점이 되기도 함
- 알고리즘적 혁신
  - 완전히 새로운 알고리즘 제안
  - 예: 새로운 경로 계획 알고리즘, 새로운 형태의 강화학습 방법 등
- 하드웨어적 혁신
  - 새로운 메커니즘이나 구조 제안
  - 예: 새로운 형태의 그리퍼 메커니즘, 새로운 관절 구조 등
- 문제 해결 접근법의 혁신
  - 기존 문제를 해결하는 완전히 새로운 접근 방식
  - 예: 로봇 제어에 생물학적 원리를 도입하는 새로운 방식 등

## Integration/Hybrid Paper (통합 및 융합 연구)
- 각각의 요소 기술들이 어떻게 시너지를 내는지 설명하는 것이 핵심
- 시스템 아키텍처와 통합 방법론에 대한 상세한 설명이 필요
- 개별 컴포넌트 간의 인터페이스와 통신 방식을 명확히 기술
- 전체 시스템의 성능뿐만 아니라 각 구성 요소의 기여도도 분석
- 통합 과정에서 발생한 문제점들과 해결 방안을 공유하는 것이 중요
- 예: 비전-힘 제어 통합, AI와 전통적 제어 이론의 결합 등

## Application/Domain Extension Paper (응용 분야 확장)
- 기존 기술의 새로운 응용 분야에서의 적용 가능성을 보여주는 것이 목적
- 응용 분야의 특수성과 그에 따른 기술적 수정사항을 상세히 설명
- 실제 현장에서의 검증 결과가 매우 중요
- 비용 효율성이나 실용성 측면의 분석이 필수적
- 새로운 응용 분야에서의 한계점과 향후 개선 방향 제시
- 예: 산업용 로봇 기술의 의료 분야 적용

## Theoretical/Foundational Paper (이론적 분석 및 증명)
- 향후 실험적 연구의 기반이 되는 이론적 프레임워크를 제시
- 수학적 엄밀성이 가장 중요한 평가 기준
- 증명 과정을 단계별로 상세히 기술
- 이론적 결과의 실제적 의미와 활용 방안을 설명
- 시뮬레이션을 통한 이론의 검증이 보통 포함됨

## 주요 평가 지표:
### 정량적 성능 지표
속도, 정확도, 효율성 등 수치화 가능한 지표
기존 방식과의 비교 분석


### 정성적 평가
실용성, 확장성, 구현 용이성 등
실제 환경에서의 적용 가능성


## 이론적 완성도
수학적/물리적 타당성
알고리즘의 수렴성, 안정성 증명

## 연구 수행 시 고려사항:
### 명확한 문제 정의
해결하고자 하는 문제의 명확한 정의
기존 연구와의 차별점 제시

### 철저한 검증
충분한 실험 데이터 확보
다양한 조건에서의 성능 검증

### 객관적 비교
공정한 비교를 위한 실험 조건 설정
표준화된 평가 지표 사용

### 한계점 분석
제안된 방법의 한계 명시
향후 연구 방향 제시

</details>

# 241128 AI 프롬프트 내용 정리

<details>

> 연구 중인 로봇은 빌딩 옥상의 2점 고정된 1쌍의 로프를 이용한 건물 외벽 이동 메커니즘에 휠이 장착된 4족 보행 로봇이 결합된 하이브리드 시스템입니다. 상부는 로프 조작을 통해 수직 이동이 가능하며, 하부의 4족 보행 로봇은 지면에서의 장애물 극복과 이동을 담당합니다. 이 로봇 시스템에 대해 질문하겠습니다. 설명한 내용을 다시 정리하지 말고 다음 질문을 기다려주세요.
> 
</details>

# 241128 다이나믹스 분석에 관한 내용 정리

<details>

## 다이나믹스 분석의 핵심 목적
- 로봇이 안정적으로 움직일 수 있는 조건 도출
- 로봇의 성능 한계 파악
- 제어기 설계를 위한 기초 데이터 확보

## 분석에서 보여줘야 할 핵심 내용
### 안정성 검증
- 로봇이 넘어지지 않고 걸을 수 있는 조건
- 보행 시 무게중심(CoM)의 움직임이 안정 영역 안에 있는지
- ZMP(Zero Moment Point)가 지지다각형 내에 있는지
- 결과물: CoM 궤적 그래프, ZMP 위치 변화 그래프

### 관절 부하 분석
- 각 관절이 견딜 수 있는 힘의 범위
- 각 다리 관절의 토크 요구사항
- 최대 하중 조건에서도 관절이 견딜 수 있는지
- 결과물: 관절별 토크-시간 그래프, 최대 부하 조건

### 에너지 소비 분석
- 얼마나 효율적으로 움직이는지
- 보행 시 소비 전력
- 다른 로봇들과 비교했을 때 효율성
- 결과물: 속도별 에너지 소비량 그래프, Cost of Transport 비교

### 외란 대응 능력
- 예상치 못한 상황에서도 쓰러지지 않는지
- 옆에서 밀었을 때 반응
- 울퉁불퉁한 지면에서 안정성
- 결과물: 외력 인가시 자세 복원 그래프, 불규칙 지형 극복 데이터

### 이런 분석들을 통해 증명하고 싶은 것
- 당신의 로봇이 기존 로봇들보다 더 안정적인지
- 더 적은 에너지로 움직일 수 있는지
- 실제 환경에서 잘 작동할 수 있는지

즉, 다이나믹스 분석은 "이 로봇이 실제로 잘 작동할 것이다"를 수학적/실험적으로 증명하는 과정입니다. 이를 통해 로봇의 성능을 정량적으로 보여주고, 다른 로봇들과 비교할 수 있는 기준을 제시하게 됩니다.

### 활용처
- 설계 검증 - 설계한 로봇이 실제로 작동할 수 있는지 확인
  - 선택한 모터가 충분한 토크를 낼 수 있는지
  - 구조가 받는 힘을 견딜 수 있는지
  - 예시: 달리기 동작에서 다리가 받는 최대 충격력 계산
- 성능 예측
  - 로봇이 할 수 있는 것과 없는 것을 미리 파악
    - 최대 이동 속도
    - 극복 가능한 장애물 높이
    - 허용 가능한 경사도예시: 보행 속도에 따른 에너지 소비량 예측
- 차후 연구 기반
  - 제어 전략 수립 - 안정적인 움직임을 위한 기준 제시
    - 다리 궤적 설계
    - 자세 제어 방법
    - 외란 대응 전략
    - 예시: 울퉁불퉁한 지면에서 균형을 잡기 위한 제어 방법 도출
  - 최적화 방향 제시 - 성능 향상을 위해 개선해야 할 부분 파악
    - 무게 배치
    - 관절 구조
    - 센서 위치
    - 예시: 에너지 효율을 높이기 위한 다리 길이 최적화


</details>

# 241128 로봇 동적 특성 분석 정리
Anymal의 최적화 기준[^Anymal_dyn]
1. Floating base equations of motion
2. Torque limits, Friction cone and λ modulation
3. No motion at the contact points
4. Center of mass linear motion, Center of mass angular motion, Swing leg motion tracking
5. Contact force minimization


# 241130 Gazebo 케이블 모델링

<details>

관련 스레드 
https://community.gazebosim.org/t/modelling-hanging-ropes-catenaries-in-gazebo/2524

https://github.com/jmz3/Gazebo_cable

</details>


# 241130 4족로봇 동적 특성

<details>

## 1. 기구학적 분석 요소

### 다리의 기구학적 구조 및 관절 구성
- 각 다리의 자유도 (보통 3-4 DOF)
- 관절 타입 (회전, 직선운동 등)
- 워크스페이스 분석

### 순기구학/역기구학 관계
- 발끝 위치와 관절 각도간의 매핑
- 특이점 분석

### 보행 패턴에 따른 자세 분석
- 정적/동적 안정성을 위한 자세 계산
- 지지다각형(Support polygon) 분석

## 2. 동역학적 분석 요소

### 질량 특성
- 각 링크의 질량, 질량중심
- 관성 텐서

### 힘/토크 분석
- 각 관절에 작용하는 토크
- 지면 반력
- 마찰력

### 에너지 분석
- 운동에너지와 위치에너지
- 파워 소비
- 에너지 효율성

## 3. 제어 관련 분석

### 안정성 지표
- Zero Moment Point (ZMP)
- Center of Pressure (CoP)
- Center of Mass (CoM) 궤적

### 보행 위상 제어
- 스윙 페이즈/스탠스 페이즈 전환
- 듀티 팩터
- 보폭과 보행 주기

## 4. 외부 환경 상호작용

### 지면 접촉 모델링
- 충격력 분석
- 접촉면 특성

### 마찰 특성
- 정지마찰/운동마찰 계수
- 미끄럼 방지

## 5. 시스템 성능 지표

### 이동성 분석
- 최대 보행 속도
- 방향 전환 능력
- 경사면 등반 능력

### 에너지 효율
- Cost of Transport (COT)
- 전력 소비량

### 하중 지지 능력
- 최대 적재 하중
- 자세별 하중 분포

이러한 요소들은 서로 긴밀하게 연관되어 있으며, 로봇의 목적과 운용 환경에 따라 각 요소의 중요도가 달라질 수 있습니다. 예를 들어 고속 주행을 목적으로 하는 4족 로봇과 험지 극복을 목적으로 하는 4족 로봇은 서로 다른 요소에 중점을 두고 설계/분석이 이루어집니다.

</details>

# 241130 CDPR 동적 특성

<details>

## 기구학적 분석 요소
플랫폼의 위치/자세 관계식
케이블 길이와 엔드이펙터 간의 기구학적 관계
작업공간 분석 (workspace analysis)
특이점 분석 (singularity analysis)

## 케이블 특성 관련 요소
케이블 장력 분포
케이블 탄성 효과
케이블 처짐 현상 (새그, sag)
케이블 진동 특성

## 동역학 지배방정식 관련 요소

플랫폼의 운동방정식
케이블 장력과 플랫폼 운동 간의 관계식
케이블-플랫폼 연성 효과
외란에 대한 시스템 응답

## 제어 성능 관련 요소
장력 최적화 (tension optimization)
동적 안정성 (dynamic stability)
위치/힘 제어 정확도
진동 제어 성능

## 시스템 제약조건
케이블 장력 제한 조건
기구학적 구속조건
작업공간 제약조건
구동기 용량 제한

## 외부 환경 영향
중력 효과
관성력 영향
외부 하중 영향
환경적 외란 (바람 등)

## 성능 평가 지표
위치 정확도
동적 응답 특성
강성 특성
에너지 효율성

## 안전성 관련 요소
케이블 파단 위험성
시스템 안정성 여유
비상 정지 상황 대응
충돌 회피 능력

</details>


# 241203 열화상 카메라 메모

<details>
[관부가세포함] Teledyne Flir Blackfly S USB3 6.3MP Black & White Camera (C-Mount) BFS-U3-63S4M-C
https://usashop.co.kr/goods/goods_view.php?goodsNo=1000241640
https://github.com/ros-drivers/flir_camera_driver/blob/humble-release/spinnaker_camera_driver/doc/index.rst
</details>


# 241204 Dynamic anlaysis

<details>

## 에너지 효율


## 안정성


## 강건성


## 토크 분배
조인트, 어센더 모터, 앵커 포인트 텐션

## 서포트 폴리곤
일반 로봇과 달리 로프 고정점, 다리 컨택 포인트를 통합해서 서포트 폴리곤이 구성됨.
이게 서포트 폴리곤이 어디로 구성되나, 기준것과는 어떻게 다른지에 대한 증명 고찰 필요.




</details>

---
# 24120 템플릿

<details>
</details>


<!--
<img src="./example.png" width="300" height="200" alt="이미지 설명">
<img src="./example.png" alt="이미지 설명">
-->


[^LX]: https://www.b2bzincatalog.com/digital/catalog/specin/
[^MECE]: https://en.wikipedia.org/wiki/MECE_principle
[^CLT]: https://statisticsplaybook.tistory.com/67
[^ASM]: https://dl.asminternational.org/handbooks/pages/Handbooks_by_Volume
[^SCS]: https://en.wikipedia.org/wiki/Safety-critical_system
[^SCM]: https://en.wikipedia.org/wiki/Swiss_cheese_model
[^case1]: https://www.encata.net/blog/case-study-a-warehouse-robot
[^X57]: https://www.nasa.gov/nasa-missions/x-57-aircraft/
[^ARGOS]: https://ntrs.nasa.gov/citations/20120000452
[^AtlasCentaur]: https://en.wikipedia.org/wiki/Atlas-Centaur
[^Aramid]: http://www.11st.co.kr/products/6183466705/share
[^dyn1]: Zheng, Xingwen, et al. "Three-dimensional dynamic modeling and motion analysis of a fin-actuated robot." IEEE/ASME Transactions on Mechatronics 27.4 (2022): 1990-1997.
[^dyn2]: Khoramshahi, Mahdi, and Aude Billard. "A dynamical system approach to task-adaptation in physical human–robot interaction." Autonomous Robots 43 (2019): 927-946.
[^dyn3]: Yu, J., Yuan, J., Wu, Z., & Tan, M. (2016). Data-Driven Dynamic Modeling for a Swimming Robotic Fish. IEEE Transactions on Industrial Electronics, 63, 5632-5640. https://doi.org/10.1109/TIE.2016.2564338.
[^dyn4]: An overview of dynamic parameter identification of robots
[^dyn5]: A modular approach for dynamic modeling of multisegment continuum robots
[^dyn6]: An Iterative Approach for Accurate Dynamic Model Identification of Industrial Robots
[^dyn7]: Dynamic Modeling of Tendon-Driven Co-Manipulative Continuum Robots
[^dyn8]: Dynamic Analysis Tool for Legged Robots
[^dyn9]: Xu, W., Peng, J., Liang, B., & Mu, Z. (2016). Hybrid modeling and analysis method for dynamic coupling of space robots. IEEE Transactions on Aerospace and Electronic Systems, 52, 85-98. https://doi.org/10.1109/TAES.2015.140752. 106 cited.
[^dyn10]: Shah, S., Saha, S., & Dutt, J. (2012). Modular framework for dynamic modeling and analyses of legged robots. Mechanism and Machine Theory, 49, 234-255. https://doi.org/10.1016/J.MECHMACHTHEORY.2011.10.006. 41 cited.


[^dyn11]: Yuan, H., Courteille, E., & Deblaise, D. (2015). Static and dynamic stiffness analyses of cable-driven parallel robots with non-negligible cable mass and elasticity. Mechanism and Machine Theory, 85, 64-81. https://doi.org/10.1016/J.MECHMACHTHEORY.2014.10.010. 97 cited.

[^dyn12]: Azad, M., Babič, J., & Mistry, M. (2019). Effects of the weighting matrix on dynamic manipulability of robots. Autonomous Robots, 43, 1867 - 1879. https://doi.org/10.1007/s10514-018-09819-y. 18 cited.

[^dyn13]: Ferravante, V., Riva, E., Taghavi, M., Braghin, F., & Bock, T. (2019). Dynamic analysis of high precision construction cable-driven parallel robots. Mechanism and Machine Theory. https://doi.org/10.1016/J.MECHMACHTHEORY.2019.01.023. 25 cited.

[^Anymal_dyn]: Bellicoso, D., Jenelten, F., Gehring, C., & Hutter, M. (2018). Dynamic Locomotion Through Online Nonlinear Motion Optimization for Quadrupedal Robots. IEEE Robotics and Automation Letters, 3, 2261-2268. https://doi.org/10.1109/LRA.2018.2794620.