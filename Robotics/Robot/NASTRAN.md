# NASTRAN

> 상위: [[구조해석]]

항공우주 분야의 표준으로 자리잡은 고성능 유한요소해석 소프트웨어입니다.

## 📚 주요 레퍼런스

### 공식 문서
- **MSC Nastran User's Guide** - 기본 사용법과 해석 절차
- **MSC Nastran Quick Reference Guide** - 명령어 빠른 참조
- **MSC Nastran Linear Static Analysis User's Guide** - 선형 정적해석
- **MSC Nastran Dynamic Analysis User's Guide** - 동적해석 전문

### 실무 교재
- **Siemens NX Nastran** - NX 통합 환경 가이드
- **Femap with NX Nastran** - 전후처리 도구
- **NASA Structural Analysis Manual** - NASA 구조해석 기준

## 🏛️ NASTRAN의 역사와 특징

### 개발 배경
```
역사:
1960년대 NASA 주도 개발
항공우주 구조해석 목적
대규모 선형 해석 특화

특징:
- 매우 안정적이고 검증된 솔버
- 대규모 문제 처리 능력
- 정확한 수치해석 알고리즘
- 항공우주 산업 표준

버전별 발전:
- MSC Nastran: 상용화 버전
- NX Nastran: Siemens 통합
- NEi Nastran: 중소기업용
- OpenBEM: 오픈소스 호환
```

### 핵심 강점
```
선형 해석:
- 매우 빠른 해석 속도
- 정확한 수치해
- 대규모 모델 처리
- 메모리 효율성

동적 해석:
- 모달 해석 전문성
- 주파수응답 해석
- 과도응답 해석
- 랜덤 진동 해석

최적화:
- 구조 최적화
- 크기 최적화
- 형상 최적화
- 위상 최적화
```

## 🔧 NASTRAN 해석 유형

### SOL 시퀀스 (Solution Sequences)
```
정적 해석:
- SOL 101: Linear Static
- SOL 106: Nonlinear Static
- SOL 400: Nonlinear Static/Transient

동적 해석:
- SOL 103: Normal Modes
- SOL 108: Frequency Response
- SOL 109: Transient Response
- SOL 111: Modal Frequency Response
- SOL 112: Modal Transient Response

좌굴/최적화:
- SOL 105: Buckling
- SOL 200: Design Optimization
- SOL 400: Advanced Nonlinear

특수 해석:
- SOL 114: Cyclic Symmetry
- SOL 128: Nonlinear Transient
- SOL 129: Nonlinear Transient (Arc-length)
- SOL 401: Implicit Nonlinear
- SOL 402: Implicit Nonlinear Transient
```

### 입력 파일 구조
```
Bulk Data 형식:
- Fixed Format: 고정 필드 형식
- Free Format: 자유 형식
- Large Format: 확장 형식

주요 섹션:
- Executive Control: 해석 제어
- Case Control: 해석 조건
- Bulk Data: 모델 데이터

예제 구조:
ID PROJECT,1
SOL 101
CEND
TITLE = Linear Static Analysis
LOAD = 100
SPC = 200
BEGIN BULK
...
ENDDATA
```

## 📊 NASTRAN 요소와 재료

### 요소 라이브러리
```
1차원 요소:
- CROD: 막대 요소
- CBAR: 보 요소
- CBEAM: 일반 보 요소
- CELAS: 스프링 요소

2차원 요소:
- CTRIA3: 3절점 삼각형
- CQUAD4: 4절점 사각형 쉘
- CQUAD8: 8절점 사각형 쉘
- CSHEAR: 전단 패널

3차원 요소:
- CTETRA: 사면체 요소
- CHEXA: 육면체 요소
- CPENTA: 오면체 요소
- CPYRAM: 피라미드 요소

특수 요소:
- RBE2: 강체 요소
- RBE3: 분산 커플링
- MPC: 다점 제약
- CBUSH: 부시 요소
```

### 재료 모델
```
선형 재료:
- MAT1: 등방성 재료
- MAT2: 이방성 재료 (2D)
- MAT8: 직교이방성 (쉘)
- MAT9: 이방성 재료 (3D)

온도 의존:
- MATT1: 온도 의존 MAT1
- MATT2: 온도 의존 MAT2
- TABLEM1: 재료 테이블

복합재료:
- MAT8: 단방향 복합재
- PCOMP: 복합재 적층
- PCOMPG: 복합재 적층 (일반)

비선형 재료:
- MATS1: 소성 재료
- MATEP: 탄소성 재료
- MATHE: 탄소성 재료 (확장)
```

## 🔨 NASTRAN 고급 기능

### 부구조 해석 (Superelement)
```
Craig-Bampton 방법:
- 고정 경계면 모드
- 제약 모드
- 축약된 강성/질량 행렬

장점:
- 대규모 모델 효율화
- 모델 재사용성
- 병렬 계산 가능
- 기밀성 보장

응용:
- 항공기 날개+동체
- 자동차 차체 조립
- 선박 구조
- 복잡한 조립체
```

### 접촉 해석
```
접촉 요소:
- CGAP: 간극 요소
- CBUSH: 부시/댐퍼
- Contact Bodies: 접촉체

접촉 알고리즘:
- Penalty Method: 페널티법
- Augmented Lagrange: 증강 라그랑지안
- Direct Constraint: 직접 제약

한계:
NASTRAN은 일반적으로 선형 해석 위주
복잡한 접촉은 타 소프트웨어 권장
```

### 동적 해석 특수 기능
```
모달 해석:
- 다양한 고유값 추출법
- Lanczos, Subspace Iteration
- Inverse Power, Givens
- 복소 고유값 해석

주파수 응답:
- Modal Method: 빠른 해석
- Direct Method: 정확한 해석
- Residual Vector: 정적 보정

랜덤 진동:
- PSD Analysis: 파워 스펙트럴 밀도
- Response PSD: 응답 스펙트럼
- RMS Response: 실효값 응답
```

## 🎯 항공우주 특화 기능

### 공력탄성학 (Aeroelasticity)
```
해석 유형:
- Static Aeroelastic: 정적 공력탄성
- Flutter Analysis: 플러터 해석
- Dynamic Aeroelastic: 동적 공력탄성
- Gust Response: 돌풍 응답

SOL 시퀀스:
- SOL 144: Static Aeroelastic
- SOL 145: Flutter Analysis
- SOL 146: Dynamic Aeroelastic

공력 모델:
- Doublet Lattice Method
- Piston Theory
- Vortex Lattice Method
```

### 복합재료 해석
```
층상 복합재:
- PCOMP: 적층 정의
- Failure Criteria: 파손 기준
- Ply Stress: 층별 응력
- Reserve Factor: 여유계수

해석 기능:
- First Ply Failure: 첫 층 파손
- Progressive Failure: 점진적 파손
- Interlaminar Stress: 층간응력
- Free Edge Effect: 자유단 효과

적용:
- 항공기 날개
- 동체 패널
- 제어면
- 복합재 구조
```

### 열해석과 연성
```
열해석:
- SOL 153: Heat Transfer
- Steady State: 정상상태
- Transient: 과도상태

열-구조 연성:
- Sequential Coupling: 순차 연성
- Temperature Loading: 온도 하중
- Thermal Stress: 열응력

응용:
- 고온 부품 해석
- 열팽창 응력
- 온도 구배 효과
```

## 💻 NASTRAN 최적화

### 설계 최적화 (SOL 200)
```
최적화 유형:
- Size Optimization: 크기 최적화
- Shape Optimization: 형상 최적화
- Topology Optimization: 위상 최적화
- Topometry Optimization: 기하 최적화

설계 변수:
- DESVAR: 설계 변수 정의
- DVPREL1: 물성 관계
- DVMREL1: 재료 관계
- DVGRID: 격자점 관계

제약조건:
- DCONSTR: 응력/변위 제약
- DCONADD: 제약 조합
- Frequency Constraint: 주파수 제약
- Buckling Constraint: 좌굴 제약

목적함수:
- Weight: 중량 최소화
- Compliance: 컴플라이언스 최소화
- Frequency: 주파수 최대화
- User Defined: 사용자 정의
```

### 다분야 최적화 (MDO)
```
연성 최적화:
- Aero-Structure: 공력-구조
- Thermo-Structure: 열-구조
- Multi-Objective: 다목적

최적화 알고리즘:
- Gradient Based: 기울기 기반
- Genetic Algorithm: 유전 알고리즘
- Response Surface: 응답면 기법
```

## 🛠️ 실무 활용 가이드

### 모델링 모범 사례
```
요소 선택:
- 1차 요소: 일반적 사용
- 2차 요소: 정밀한 응력 필요시
- 적절한 요소 크기
- 종횡비 제한

모델 검증:
- GPFORCE: 격자점 하중 확인
- SPCFORCE: 구속력 확인
- WEIGHT: 질량 확인
- Modal Assurance: 모드 검증

품질 확인:
- CCHECK: 연결성 확인
- Element Quality Check
- Free Body Check
- Mass Properties Check
```

### 성능 최적화
```
메모리 관리:
- PARAM,MAXRATIO: 메모리 비율
- SYSTEM(144): 스크래치 용량
- Buffsize Control: 버퍼 크기

병렬 처리:
- SMP: 공유 메모리 병렬
- DMP: 분산 메모리 병렬
- GPU Acceleration: GPU 가속

솔버 선택:
- SPARSE: 희소 행렬 솔버
- ACMS: 자동 컴포넌트 모드
- AMLS: 자동 다단계 부구조
```

### 문제 해결
```
일반적 오류:
- Singular Matrix: 특이 행렬
- Negative Diagonal: 음의 대각선
- Large Displacement: 대변위 경고
- Memory Exceeded: 메모리 초과

디버깅:
- DIAG Option: 진단 출력
- PARAM,POST: 후처리 정보
- F06 File: 결과 파일 분석
- LOG File: 로그 파일 확인

해결 방법:
- Model Checking
- Constraint Review
- Element Quality
- Load Path Analysis
```

## ⚠️ 장단점과 한계

### 장점
```
신뢰성:
- 50년 이상 검증
- 항공우주 표준
- 정확한 수치해
- 안정적 수렴성

성능:
- 대규모 모델 처리
- 빠른 선형 해석
- 효율적 메모리 사용
- 고성능 컴퓨팅 지원

기능:
- 포괄적 동적 해석
- 공력탄성학
- 구조 최적화
- 부구조 해석
```

### 한계
```
비선형 해석:
- 제한적 비선형 기능
- 복잡한 접촉 제한
- 대변형 해석 한계
- 재료 비선형성 제한

사용성:
- 텍스트 기반 입력
- 학습 곡선 가파름
- 전후처리 도구 필요
- 현대적 GUI 부족

비용:
- 높은 라이센스 비용
- 전문 교육 필요
- 하드웨어 요구사항
```

### 굳이 안 해도 되는 것
```
과도한 정밀화:
✗ 간단한 문제에 NASTRAN 사용
✗ 비선형이 주인 문제
✗ 소규모 모델 해석

적절한 사용:
- 대규모 선형 해석
- 동적 특성 중요
- 항공우주 분야
- 검증된 결과 필요
```

## 🎓 추천 학습 자료

### 공식 교육 과정
**Siemens NX Nastran 교육**
- [Siemens Xcelerator Academy](https://training.plm.automation.siemens.com/) - 공식 교육 플랫폼
  - NXNAS111: Introduction to Finite Element Analysis with NX
  - NXNAS121: Introduction to Dynamic Analysis with NX
  - NXNAS321: Superelement Analysis with NX
  - NXNAS312: Introduction to DMAP in NX Nastran

**MSC Nastran 교육**
- MSC Software Training Center - 공식 MSC Nastran 과정
- 항공우주 산업 특화 교육

### 온라인 강의 플랫폼
**Udemy 과정**
- [Siemens Femap Nastran Course](https://www.udemy.com/course/siemens-nx-nastran-learn-engineering-simulations/) - 기초부터 고급까지

**전문 교육기관**
- Simcenter 3D Pre/Post 통합 과정
- Femap with NX Nastran 전문 교육

### 참고 교재
**실무 가이드북**
- **Koh, Jaecheol** - "Siemens NX 10 Nastran: Tutorials for Beginners and Advanced Users"
- **Koh, Jaecheol** - "SIEMENS NX12 Nastran: Tutorials for Beginners and Advanced Users"

**이론서**
- **MSC Nastran User's Guide** - 공식 사용자 가이드
- **MSC Nastran Quick Reference Guide** - 명령어 빠른 참조
- **NASA Structural Analysis Manual** - NASA 구조해석 기준

### 무료 학습 자료
**공식 문서**
- [Siemens NX Nastran Documentation](https://support.industrysoftware.automation.siemens.com/general/nxn.shtml) - 공식 기술 문서
- Femap with NX Nastran Tutorial - 전후처리 도구 활용

**학생용 리소스**
- Siemens 학생용 라이센스 - 대학교 교육용
- Academic Partner Program - 교육기관 지원

### 특화 분야 교육
**항공우주 특화**
- Aeroelasticity 전문 과정
- 복합재료 해석 교육
- 구조 최적화 과정

**고급 사용자**
- DMAP 프로그래밍
- Superelement 기법
- 사용자 정의 확장

## 🔗 관련 문서

### NASTRAN 환경
- **Femap** - 전후처리 도구
- **NX CAE** - Siemens 통합 환경
- **Patran** - MSC 전후처리

### 해석 분야
- [[선형정적해석]] - NASTRAN 주력 분야
- [[모달해석]] - 고유진동 해석
- [[주파수응답해석]] - 동적 응답

### 특화 분야
- [[공력탄성학]] - 항공기 특수 해석
- [[복합재료해석]] - 층상 복합재
- [[구조최적화]] - 설계 최적화

### 비교 분석
- [[ANSYS]] - 범용 FEA
- [[Abaqus]] - 비선형 전문
- [[항공우주해석]] - 분야별 특화