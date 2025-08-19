# ANSYS

> 상위: [[구조해석]]

세계에서 가장 널리 사용되는 범용 유한요소해석 소프트웨어입니다.

## 📚 주요 레퍼런스

### 공식 문서
- **ANSYS Mechanical User's Guide** - 기본 사용법과 해석 절차
- **ANSYS Element Reference** - 요소 라이브러리와 이론
- **ANSYS Theory Reference** - 수치해석 이론과 구현
- **ANSYS Contact Technology Guide** - 접촉 해석 전문 가이드

### 실무 교재
- **Moaveni** - "Finite Element Analysis: Theory and Application with ANSYS" (4th ed.)
- **Kurowski** - "Engineering Analysis with ANSYS Software" (2nd ed.)
- **Nakasone et al.** - "Engineering Analysis with ANSYS Software"

## 🏢 ANSYS 제품군 개요

### ANSYS Workbench 플랫폼
```
통합 환경:
- Project Schematic: 해석 플로우 관리
- DesignModeler: 3D 모델링 도구
- Mechanical: 구조해석 모듈
- Meshing: 메시 생성 도구

플랫폼 특징:
- Drag & Drop 인터페이스
- 양방향 CAD 연동
- 매개변수 연구
- 최적화 통합
```

### 주요 해석 모듈
```
구조해석:
- Mechanical: 기본 구조해석
- LS-DYNA: 명시적 동적해석
- Explicit Dynamics: 충돌/폭발

유체해석:
- Fluent: 범용 CFD
- CFX: 터보기계 특화

멀티피직스:
- Thermal: 열해석
- Electromagnetic: 전자기해석
- Coupled Field: 연성해석
```

## 🔧 ANSYS Mechanical 기능

### 해석 유형
```
정적 해석:
- Static Structural: 선형/비선형 정적
- Buckling: 좌굴 해석
- Contact: 접촉 해석

동적 해석:
- Modal: 모달 해석
- Harmonic Response: 조화응답
- Transient: 과도응답
- Random Vibration: 랜덤 진동

특수 해석:
- Fatigue: 피로해석
- Fracture: 파괴역학
- Topology Optimization: 위상최적화
```

### 요소 라이브러리
```
구조 요소:
- SOLID185/186/187: 3D 솔리드 요소
- SHELL181/281: 쉘 요소
- BEAM188/189: 보 요소
- LINK180: 트러스 요소

특수 요소:
- CONTA174/TARGE170: 접촉 요소
- COMBIN14: 스프링/댐퍼 요소
- MASS21: 집중질량 요소
- FLUID220: 유체-구조 연성

고급 요소:
- SOLID226/227: 압전 요소
- PLANE223: 전기-열-구조 연성
- SHELL208: 층상 복합재 쉘
```

### 재료 모델
```
선형 재료:
- 등방성: E, ν, ρ
- 직교이방성: 복합재료
- 이방성: 일반적 이방성

비선형 재료:
- 소성: von Mises, Hill, Drucker-Prager
- 크리프: Norton, Time Hardening
- 하이퍼탄성: Neo-Hookean, Mooney-Rivlin
- 점탄성: Prony Series, Maxwell

재료 데이터:
- Engineering Data: 내장 재료 라이브러리
- 온도 의존 물성
- 변형률 속도 의존성
```

## 📊 해석 설정과 제어

### 메시 생성
```
메시 방법:
- Patch Conforming: 기본 메시
- Patch Independent: 균일한 크기
- MultiZone: 육면체 위주
- Hex Dominant: 육면체 선호

메시 제어:
- Sizing: 전역/국부 크기 제어
- Refinement: 적응적 세분화
- Inflation: 경계층 메시
- Contact Sizing: 접촉면 메시

메시 품질:
- Element Quality: 요소 품질 지수
- Aspect Ratio: 종횡비
- Skewness: 기울어짐
- Orthogonal Quality: 직교성
```

### 하중과 경계조건
```
하중 유형:
- Force: 집중하중
- Pressure: 압력하중
- Remote Force: 원격하중
- Bearing Load: 베어링 하중

경계조건:
- Fixed Support: 완전고정
- Simply Supported: 단순지지
- Displacement: 강제변위
- Remote Displacement: 원격변위

고급 하중:
- Bolt Pretension: 볼트 예긴장
- Joint Load: 관절하중
- Fluid Pressure: 유체압력
- Thermal Condition: 열하중
```

### 해석 설정
```
Solver 선택:
- Direct Solver: 직접법 (작은 모델)
- Iterative Solver: 반복법 (큰 모델)
- Distributed Solver: 분산 처리

수렴 제어:
- Force Convergence: 힘 수렴 기준
- Displacement Convergence: 변위 수렴
- Line Search: 수렴성 개선
- Adaptive Time Stepping: 적응적 시간간격

출력 제어:
- Result File: 결과 파일 크기
- Solution Information: 수렴 정보
- Restart: 재시작 옵션
```

## 🎯 비선형 해석

### 재료 비선형
```
소성 해석:
- Rate Independent Plasticity
- Rate Dependent Plasticity
- Kinematic Hardening
- Mixed Hardening

크리프 해석:
- Primary/Secondary/Tertiary Creep
- Norton Law
- Time/Strain Hardening

하이퍼탄성:
- Rubber/Foam 재료
- 대변형 해석
- 압축 불가능성
```

### 기하 비선형
```
대변형 옵션:
- Large Deflection: ON
- Updated Lagrangian
- Total Lagrangian

스핀 소프닝:
- Stress Stiffening: 응력 강성
- 좌굴 후 거동
- 불안정 해석

Arc-Length Method:
- 극한점 통과
- 스냅스루 현상
- 좌굴 후 경로
```

### 접촉 해석
```
접촉 유형:
- Bonded: 완전 결합
- No Separation: 분리 없음
- Frictionless: 마찰 없음
- Frictional: 마찰 접촉
- Rough: 완전 마찰

접촉 알고리즘:
- Pure Penalty: 페널티법
- Augmented Lagrange: 증강 라그랑지안
- Normal Lagrange: 라그랑지 승수법
- MPC: 다점 제약

접촉 제어:
- Pinball Region: 접촉 감지 영역
- Interface Treatment: 접촉면 처리
- Formulation: 공식화 방법
- Detection Method: 감지 방법
```

## 🔨 동적 해석

### 모달 해석
```
추출 방법:
- Block Lanczos: 일반적 방법
- Subspace: 부공간 반복
- Unsymmetric: 비대칭 행렬
- Damped: 감쇠 모달 해석

모드 선택:
- Number of Modes: 모드 수
- Max/Min Frequency: 주파수 범위
- Shift Point: 이동점

출력:
- Natural Frequency: 고유진동수
- Mode Shape: 모드형상
- Participation Factor: 참여계수
- Effective Mass: 유효질량
```

### 조화응답 해석
```
해석 방법:
- Full Method: 전체 방법
- Mode Superposition: 모드 중첩
- MSUP: 모드 중첩 (간소화)

주파수 설정:
- Frequency Range: 주파수 범위
- Solution Intervals: 해석 간격
- Cluster/Spread: 간격 조절

감쇠 모델:
- Modal Damping: 모달 감쇠
- Rayleigh Damping: 비례 감쇠
- Material Damping: 재료 감쇠

출력:
- Amplitude: 진폭 응답
- Phase: 위상 응답
- Real/Imaginary: 실수/허수부
```

### 과도응답 해석
```
시간 적분:
- Newmark: Newmark-β 방법
- HHT: α-방법
- Generalized-α: 일반화 α

시간 설정:
- End Time: 종료 시간
- Time Step: 시간 간격
- Auto Time Stepping: 자동 조절

하중 정의:
- Tabular Data: 표 형태 데이터
- Function: 수학 함수
- Import: 외부 파일 불러오기

출력:
- Animation: 애니메이션
- Time History: 시간 이력
- Maximum Values: 최대값
```

## 🛠️ 특수 해석 기능

### 피로 해석
```
피로 툴:
- Fatigue Tool: 기본 피로 해석
- nCode DesignLife: 고급 피로
- Safe Technology: 다축 피로

피로 이론:
- Stress Life: 응력-수명
- Strain Life: 변형률-수명
- Crack Growth: 균열 전파

하중 이력:
- Constant Amplitude: 일정 진폭
- Variable Amplitude: 변동 진폭
- PSD Loading: 확률 밀도 함수

결과:
- Life Contour: 수명 분포
- Damage: 손상도
- Safety Factor: 안전계수
```

### 파괴역학 해석
```
기능:
- Pre-meshed Crack: 기정의 균열
- Arbitrary Crack: 임의 균열
- Semi-Elliptical: 반타원 균열

해석 방법:
- VCCT: 가상 균열 폐쇄법
- J-Integral: J-적분 계산
- Stress Intensity: 응력확대계수

결과:
- K-factor: 응력확대계수
- J-Integral: J-적분값
- T-Stress: T-응력
- Energy Release Rate: 에너지 방출률
```

### 최적화
```
최적화 유형:
- Parameter Optimization: 매개변수 최적화
- Topology Optimization: 위상 최적화
- Shape Optimization: 형상 최적화

목적함수:
- Mass Minimization: 질량 최소화
- Compliance Minimization: 컴플라이언스 최소화
- Stress Minimization: 응력 최소화
- Frequency Maximization: 주파수 최대화

제약조건:
- Stress Constraint: 응력 제약
- Displacement Constraint: 변위 제약
- Volume Constraint: 체적 제약
- Manufacturing Constraint: 제조 제약
```

## 💻 실무 활용 팁

### 모델링 모범 사례
```
형상 준비:
- 불필요한 피처 제거
- 대칭성 활용
- 적절한 단순화
- CAD 호환성 확인

메시 전략:
- 응력집중부 세밀화
- 접촉면 메시 정합
- 종횡비 < 20 유지
- 품질 확인 필수

재료 정의:
- 온도 의존성 고려
- 방향성 재료 주의
- 비선형 모델 검증
```

### 해석 검증
```
수렴성 확인:
- 메시 수렴성 연구
- 시간 간격 수렴성
- 접촉 매개변수 수렴성

결과 검증:
- 평형조건 확인
- 단위계 일관성
- 물리적 타당성
- 실험/이론해 비교

오류 진단:
- 수렴 이력 확인
- 경고 메시지 검토
- 특이점 응력 제외
- 에너지 확인
```

### 성능 최적화
```
하드웨어:
- 충분한 RAM (32GB+)
- 다중 코어 CPU
- SSD 사용 권장
- GPU 가속 (일부 기능)

소프트웨어 설정:
- Solver 선택 최적화
- 병렬 처리 활용
- 메모리 관리
- 결과 파일 크기 제어

모델 최적화:
- 대칭 활용
- 부구조 기법
- 적응적 메시
- 접촉면 최소화
```

## ⚠️ 주의사항과 한계

### 일반적 주의사항
```
모델링 한계:
- 과도한 단순화 주의
- 하중 전달 경로 확인
- 경계조건 현실성
- 재료 모델 적합성

수치적 이슈:
- 조건수 문제
- 특이점 응력
- 시계열 해석 누적 오차
- 접촉 수렴성

결과 해석:
- 절점 vs 적분점 응력
- 평균화 효과
- 국부 vs 전역 응답
- 안전계수 적용
```

### 굳이 안 해도 되는 것
```
과도한 정밀화:
✗ 불필요한 고밀도 메시
✗ 의미 없는 비선형 해석
✗ 과도한 접촉 모델링

효율적 접근:
- 필요 정확도에 맞는 모델
- 검증된 해석 절차 활용
- 단계적 복잡도 증가
```
