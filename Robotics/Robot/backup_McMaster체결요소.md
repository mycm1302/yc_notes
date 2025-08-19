# McMaster 체결요소

> 상위: [[기계요소]]
> 
> 레퍼런스: **Shigley's Mechanical Engineering Design** (Budynas & Nisbett) Chapter 8, **Norton's Machine Design** Chapter 15

로봇 및 기계설계에 필수적인 체결요소들의 McMaster-Carr 조달 가이드입니다. 이론적 배경은 Shigley와 Norton 교재를 기반으로 하며, 실무 조달은 McMaster-Carr의 124년 검증된 제품군을 활용합니다.

## 📋 체결요소 분류

### 1. 나사 및 볼트 (Screws & Bolts)
> 참고: [Shigley Chapter 8.2 - Thread Geometry and Nomenclature](https://www.mheducation.com/highered/product/Shigleys-Mechanical-Engineering-Design-Nisbett.html)

#### Machine Screws (기계나사)
- **용도**: 기계 부품의 정밀 체결
- **특징**: 정확한 공차, 높은 강도
- **McMaster 장점**: 다양한 재질(스테인리스, 합금강, 황동) 즉시 조달

#### Socket Head Cap Screws (소켓 헤드 캡 스크류)
- **용도**: 공간 제약이 있는 고강도 체결
- **특징**: 육각 소켓, 높은 체결력
- **설계 고려사항**: Allen key 접근 공간 확보 필수

#### Hex Bolts (육각 볼트)
- **용도**: 구조물 체결, 높은 토크 적용
- **특징**: 외부 육각 헤드, 렌치 체결
- **재질**: ASTM A307, A325, A490 등급별 선택


#### Self-Tapping Screws (셀프 태핑 스크류)
- **용도**: 플라스틱, 얇은 금속판 체결
- **특징**: 자체 나사산 생성
- **장점**: 미리 나사산 가공 불필요

#### Threaded Rods & Studs (나사봉 및 스터드)
- **용도**: 장거리 체결, 조정 가능한 연결
- **규격**: DIN 975, ASTM A36/A193 준수
- **활용**: 프레임 연결, 조정 기구

### 2. 너트 및 와셔 (Nuts & Washers)
> 참고: [Norton Chapter 15.4 - Types of Screw Fasteners](https://www.pearsonhighered.com/norton)

#### Hex Nuts (육각 너트)
- **표준형**: 일반적인 체결용
- **플랜지형**: 와셔 기능 내장
- **잠금형**: 진동 방지 기능

#### Lock Nuts (잠금 너트)
- **나일론 인서트형**: 재사용 가능, 중간 강도
- **전금속형**: 고온 환경, 높은 신뢰성
- **프리베일링형**: 정밀 토크 제어

#### Washers (와셔)
- **평와셔**: 하중 분산, 표면 보호
- **스프링와셔**: 진동 방지, 체결력 유지
- **톱니와셔**: 회전 방지, 높은 마찰

### 3. 인서트 및 스페이서 (Inserts & Spacers)

#### Helical Inserts (헬리컬 인서트)
- **용도**: 알루미늄, 플라스틱 강화
- **브랜드**: HeliCoil, Keensert
- **장점**: 내마모성, 정밀도 향상

#### Threaded Inserts (나사 인서트)
- **열압착형**: 플라스틱용
- **압입형**: 금속용
- **몰딩형**: 사출성형 동시 삽입

#### Spacers & Standoffs (스페이서 및 스탠드오프)
- **원형 스페이서**: 정확한 간격 유지
- **육각 스탠드오프**: 회로기판 장착
- **조정형**: 높이 조절 가능


### 4. 핀류 (Pins)
> 참고: [Shigley Chapter 8.5 - Pins and Dowels](https://www.mheducation.com/highered/product/Shigleys-Mechanical-Engineering-Design-Nisbett.html)

#### Dowel Pins (다웰핀)
- **정밀 위치결정**: ±0.0002" 공차
- **재질**: 경화강, 스테인리스
- **용도**: 부품 얼라인먼트, 회전 방지

#### Spring Pins (스프링핀)
- **롤핀**: 원통형, 높은 유지력
- **스파이럴핀**: 나선형, 쉬운 삽입
- **장점**: 구멍 공차 관대, 재사용 가능

#### Clevis Pins (클리비스핀)
- **용도**: 관절 연결, 회전 허용
- **특징**: 헤드와 코터핀 홀
- **응용**: 실린더 연결, 링크 기구

#### Cotter Pins (코터핀)
- **기능**: 다른 핀이나 너트 고정
- **형태**: 반원형, 쉬운 삽입/제거
- **재질**: 스테인리스, 아연도금강

### 5. 특수 체결요소 (Specialty Fastening)

#### Key Stock (키재)
- **평키**: 표준 DIN 6885, JIS B 1301
- **각키**: 정사각형 단면
- **용도**: 축과 허브 토크 전달

#### Retaining Rings (리테이닝링)
- **내부용**: 축 홈 장착
- **외부용**: 구멍 홈 장착
- **나선형**: 연속 조정 가능

#### Cable Ties (케이블타이)
- **나일론**: 일반 용도
- **스테인리스**: 고온/부식 환경
- **재사용형**: 릴리즈 탭 내장

#### Magnets (자석)
- **네오디뮴**: 초강력, 소형화
- **세라믹**: 경제적, 내열성
- **조립용**: 나사 장착 구멍

## 🔧 설계 고려사항

### 체결력 계산
> 참고: [Shigley Chapter 8.6 - Bolt Preload](https://www.mheducation.com/highered/product/Shigleys-Mechanical-Engineering-Design-Nisbett.html)

```
예압력 Fi = 0.75 × 입증응력 × 인장응력단면적
최대체결토크 = K × d × Fi
K: 토크계수 (일반적으로 0.2)
d: 볼트 공칭지름
```

### 재질 선택 기준
- **스테인리스 316**: 부식 환경, 식품 등급
- **합금강 12.9급**: 고강도 요구
- **황동**: 전기 전도성, 비자성
- **나일론**: 절연, 경량

### 표면처리 선택
- **아연도금**: 일반 방청
- **크롬도금**: 내마모성
- **양극산화**: 알루미늄 경화
- **흑색산화**: 미관, 경미한 방청

## 💡 McMaster-Carr 활용 팁

### 신속 조달 전략
1. **표준 규격 우선**: DIN, JIS, ASTM 표준품
2. **재고 확인**: 당일 배송 표시 제품
3. **대량 할인**: 25개, 100개 단위 구매
4. **기술 데이터**: 상세 CAD 도면 제공

### 품질 보증
- **인증서**: 재질 증명서 요청 가능
- **추적성**: 로트 번호 관리
- **반품 정책**: 30일 무조건 반품
- **기술 지원**: 전화/이메일 상담

---

## 📚 관련 참고자료

### 표준 및 규격
- **ASTM F568**: 볼트, 스크류, 스터드 규격
- **DIN 912**: 소켓 헤드 캡 스크류
- **JIS B 1176**: 육각 소켓 머리 볼트
- **ISO 4762**: 육각 소켓 머리 캡 스크류

### 설계 참고서
- [Budynas, R.G., Nisbett, J.K. "Shigley's Mechanical Engineering Design" 11th Ed.](https://www.mheducation.com/highered/product/Shigleys-Mechanical-Engineering-Design-Nisbett.html)
- [Norton, R.L. "Machine Design: An Integrated Approach" 6th Ed.](https://www.pearsonhighered.com/norton)
- **Machinery's Handbook 31st Edition**: 체결구 규격 종합
- **McMaster-Carr Catalog #130**: 최신 제품 및 기술 데이터

### 온라인 리소스
- [McMaster-Carr 기술 자료실](https://www.mcmaster.com)
- [ASTM International 표준](https://www.astm.org)
- [DIN 독일 표준연구소](https://www.din.de)
- [JIS 일본산업표준](https://www.jisc.go.jp)

