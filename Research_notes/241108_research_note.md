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
    - 안정성 확보
    - 문서화
    - 유지보수 계획
- 이동 성능 (Navigation)
- 파지 능력 (Grasping)
- 충돌 회피 (Collision Avoidance)

## 실험 설계 시 고려사항
### 1. 환경 설정
- 조명 조건
- 온도/습도
- 바닥 상태
- 외부 진동

### 2. 데이터 수집
- 센서 데이터 동기화
- 샘플링 주파수
- 데이터 저장 형식
- 노이즈 필터링

### 3. 평가 기준
- 반복 실험 횟수
- 통계적 유의성
- 오차 허용 범위
- 실패 케이스 분류

### 4. 안전 고려사항
- 비상 정지 시스템
- 작업 공간 제한
- 속도/힘 제한
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
- 이력서, 자소서

## 기타
- 컨택트 모델

</details>

# 241119 한국기계연구원 세미나
## 정애누나 세미나
Exoskeletone, 외골격 로봇
Hard/ soft werable type



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