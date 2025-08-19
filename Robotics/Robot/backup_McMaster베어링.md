# McMaster 베어링

> 상위: [[기계요소]]
> 
> 레퍼런스: **Harris & Kotzalas "Rolling Bearing Analysis"**, **Hamrock "Essential Concepts of Bearing Technology"**, **Shigley's Mechanical Engineering Design** Chapter 11

로봇 및 기계설계에서 회전 운동을 지지하는 핵심 기계요소인 베어링의 McMaster-Carr 조달 가이드입니다. 이론적 배경은 Harris와 Hamrock의 권위 있는 베어링 이론서를 기반으로 합니다.

## 📋 베어링 분류

### 1. 볼베어링 (Ball Bearings)
> 참고: [Harris & Kotzalas "Rolling Bearing Analysis" 5th Ed.](https://www.taylorfrancis.com/books/mono/10.1201/9781420006599/essential-concepts-bearing-technology-michael-kotzalas-tedric-harris)

#### Deep Groove Ball Bearings (깊은 홈 볼베어링)
- **용도**: 일반적인 회전 지지, 경·중하중
- **특징**: 높은 속도, 저소음, 양방향 하중
- **규격**: ISO 15:2017, DIN 625 준수
- **McMaster 장점**: SKF, NTN, NSK 정품 즉시 조달

#### Angular Contact Ball Bearings (앵귤러 볼베어링)
- **용도**: 고속 스핀들, 축방향 하중 지배적
- **접촉각**: 15°, 25°, 40° 선택
- **조합**: 등배치(DB), 면대면(DF), 연속(DT)
- **응용**: 공작기계 주축, 로봇 관절

#### Self-Aligning Ball Bearings (자동조심 볼베어링)
- **특징**: 축 기울어짐 보상 (±1.5°~3°)
- **구조**: 구면 외륜 궤도
- **용도**: 긴 축, 하우징 변형 허용


### 2. 롤러베어링 (Roller Bearings)
> 참고: [Hamrock "Ball Bearing Lubrication" Wiley, 1981](https://link.springer.com/content/pdf/10.1007/978-1-4612-5276-4.pdf)

#### Cylindrical Roller Bearings (원통 롤러베어링)
- **용도**: 고하중, 높은 강성
- **형태**: NU, NJ, NUP, N 시리즈
- **특징**: 축방향 변위 허용
- **응용**: 기어박스, 대형 모터

#### Tapered Roller Bearings (테이퍼 롤러베어링)
- **용도**: 복합하중 (반지름+축방향)
- **조정**: 백래시 및 예압 조정 가능
- **특징**: 높은 축방향 강성
- **응용**: 자동차 허브, 산업용 감속기

#### Needle Roller Bearings (니들 롤러베어링)
- **특징**: 소형, 높은 하중 용량
- **형태**: 내륜 유/무, 케이지형
- **용도**: 공간 제약, 고하중
- **응용**: 유니버설 조인트, 컨넥팅 로드

### 3. 추력베어링 (Thrust Bearings)

#### Ball Thrust Bearings (볼 추력베어링)
- **용도**: 축방향 하중 전용
- **형태**: 단방향, 양방향
- **특징**: 낮은 축방향 공간
- **제한**: 고속 부적합

#### Cylindrical Roller Thrust Bearings (원통 롤러 추력베어링)
- **용도**: 대형 축방향 하중
- **특징**: 높은 강성, 정밀도
- **응용**: 크레인 선회부, 터빈

### 4. 리니어베어링 (Linear Bearings)
> 참고: [Schmid, Hamrock & Jacobson "Fundamentals of Machine Elements" 3rd Ed.](https://www.routledge.com/Fundamentals-of-Machine-Elements/Schmid-Hamrock-Jacobson/p/book/9781439891322)

#### Linear Ball Bearings (리니어 볼베어링)
- **용도**: 직선 운동 지지
- **형태**: 개방형, 밀폐형
- **특징**: 저마찰, 긴 수명
- **규격**: ISO 12164 표준

#### Linear Rod Ends (리니어 로드엔드)
- **용도**: 실린더 연결, 링크 기구
- **종류**: 수나사, 암나사
- **재질**: 스테인리스, 크롬강

### 5. Pillow Block Bearings (베어링 하우징)

#### Cast Iron Pillow Blocks (주철 필로우 블록)
- **용도**: 일반 산업용
- **특징**: 경제적, 견고함
- **윤활**: 그리스 급유, 급유구

#### Stainless Steel Units (스테인리스 유닛)
- **용도**: 식품, 화학공업
- **특징**: 내부식성, 위생적
- **밀봉**: 식품등급 씰

#### Flange Mount Bearings (플랜지 마운트)
- **장착**: 수직면 볼트 체결
- **용도**: 벽면 장착, 브래킷 설치
- **조정**: 편심 칼라 위치 조정


## 🔧 베어링 설계 이론

### 동정격하중 계산
> 참고: [Harris "Rolling Bearing Analysis" Chapter 4](https://www.taylorfrancis.com/books/mono/10.1201/9781420006599/essential-concepts-bearing-technology-michael-kotzalas-tedric-harris)

```
L₁₀ = (C/P)ⁿ
L₁₀: 수명 (백만 회전)
C: 동정격하중 (N)
P: 등가동하중 (N)
n: 3 (볼베어링), 10/3 (롤러베어링)
```

#### 등가동하중 계산
```
P = X·Fr + Y·Fa
Fr: 반지름 하중
Fa: 축방향 하중
X, Y: 하중계수 (베어링별 상이)
```

### 정격수명 수정계수
> 참고: [ISO 281:2007 베어링 수명 계산](https://www.iso.org/standard/38102.html)

```
L₁₀ₘ = a₁·aISO·L₁₀
a₁: 신뢰도 계수
aISO: 윤활 및 오염 계수
```

### 축과 하우징 공차
> 참고: [Shigley Chapter 11.8 - Bearing Mounting](https://www.mheducation.com/highered/product/Shigleys-Mechanical-Engineering-Design-Nisbett.html)

#### 축 공차 (IT6~IT7)
- **회전 내륜**: k6, m6 끼워맞춤
- **고정 내륜**: h6, j6 끼워맞춤
- **경하중**: g6, h6 끼워맞춤

#### 하우징 공차 (IT7~IT8)
- **회전 외륜**: P7, N7 끼워맞춤
- **고정 외륜**: H7, J7 끼워맞춤
- **조정 필요**: K7, M7 끼워맞춤

## 💡 베어링 선정 기준

### 하중 특성별 선택
1. **순수 반지름하중**: 깊은 홈 볼베어링
2. **순수 축방향하중**: 추력베어링
3. **복합하중**: 앵귤러 베어링, 테이퍼 롤러
4. **고하중**: 롤러베어링
5. **고속**: 볼베어링 (dmn 한계 고려)

### 환경 조건별 선택
- **부식 환경**: 스테인리스 베어링
- **고온**: 특수 윤활제, 세라믹 볼
- **청정실**: 밀폐형, 특수 그리스
- **진동**: 예압 베어링, 강성 향상

### 정밀도 등급 (ISO 492)
- **P0 (보통)**: 일반 기계
- **P6 (고급)**: 공작기계
- **P5 (정밀)**: 고속 스핀들
- **P4 (초정밀)**: 측정기, 항공우주

## 🛠️ McMaster-Carr 활용 전략

### 베어링 브랜드별 특징
- **SKF**: 전세계 1위, 종합 품질
- **NTN**: 자동차 전문, 고품질
- **NSK**: 정밀베어링 강세
- **Timken**: 테이퍼 롤러 전문
- **INA**: 니들베어링, 리니어 전문

### 신속 조달 팁
1. **표준 사이즈**: 6000, 6200 시리즈 우선
2. **재고 확인**: 당일 배송 표시 확인
3. **대체품**: 호환 베어링 추천 활용
4. **기술 데이터**: CAD 모델 다운로드

### 윤활 및 밀봉
- **개방형**: 별도 윤활 시스템 필요
- **ZZ (차폐)**: 경하중, 청정 환경
- **2RS (고무씰)**: 일반 환경, 그리스 봉입
- **고온용**: 특수 그리스, 세라믹 코팅

## 🔧 설치 및 유지보수

### 설치 주의사항
1. **가열 설치**: 80-100°C 오일욕 가열
2. **압입 도구**: 내륜과 외륜 분리 압입
3. **청정도**: 설치 전 청정실 수준 관리
4. **정렬**: 동심도 및 직각도 점검

### 윤활 관리
- **그리스량**: 베어링 공간의 1/3~1/2
- **교체주기**: 운전시간 또는 온도 기준
- **호환성**: 기존 그리스와 혼용 금지
- **온도**: 윤활제 사용온도 범위 준수

---

## 📚 관련 참고자료

### 베어링 이론서
- [Harris, T.A., Kotzalas, M.N. "Rolling Bearing Analysis" 5th Ed.](https://www.taylorfrancis.com/books/mono/10.1201/9781420006599/essential-concepts-bearing-technology-michael-kotzalas-tedric-harris)
- [Hamrock, B.J. "Ball Bearing Lubrication" Wiley, 1981](https://onlinelibrary.wiley.com/doi/book/10.1002/9781118053843)
- **Gupta, P.K. "Advanced Dynamics of Rolling Elements" Springer**
- **Palmgren, A. "Ball and Roller Bearing Engineering" 3rd Ed.**

### 국제 표준
- **ISO 281**: 베어링 동정격하중 및 수명 계산
- **ISO 492**: 베어링 치수 및 회전 정밀도
- **ISO 15312**: 롤러베어링 동정격하중
- **DIN 625**: 깊은 홈 볼베어링 치수

### 제조사 기술자료
- [SKF General Catalog](https://www.skf.com)
- [NTN Technical Handbook](https://www.ntnglobal.com)
- [NSK Motion & Control](https://www.nsk.com)
- [Timken Engineering Manual](https://www.timken.com)

### 온라인 계산 도구
- SKF Bearing Calculator: 수명 및 하중 계산
- NTN Technical Calculator: 베어링 선정
- McMaster-Carr CAD 라이브러리: 3D 모델 다운로드

