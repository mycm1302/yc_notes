# 방수 설계 (Waterproof Design)

> 상위: [[설계]]
> 
> 레퍼런스: **[IEC 60529](https://www.iec.ch/ip-ratings)** - IP 등급 국제 표준, **[Parker O-Ring Handbook](https://www.fiveflute.com/guide/how-to-design-waterproof-products/)** - 씰링 설계 실무

액체 침입으로부터 전자 장비와 기계 부품을 보호하는 설계 기법입니다. IP(Ingress Protection) 등급 체계를 기반으로 하며, O-링, 가스켓, 하우징 설계 등 다양한 기법을 사용합니다.

## 💧 IP 등급 체계

### 액체 보호 등급 (두 번째 숫자)
**[IEC 60529](https://en.wikipedia.org/wiki/IP_code) 기준 액체 침입 보호 분류**

- **IPX0**: 보호 없음
- **IPX1**: 수직 낙하수 보호 (분당 1mm, 10분간)
- **IPX2**: 15° 기울어진 낙하수 보호
- **IPX3**: 분사수 보호 (60° 각도, 분당 0.7L, 5분간)
- **IPX4**: 모든 방향 분사수 보호 (분당 10L, 5분간)
- **IPX5**: 저압 물 분사 보호 (30kPa, 분당 12.5L, 3분간)
- **IPX6**: 강력한 물 분사 보호 (100kPa, 분당 100L, 3분간)
- **IPX7**: 일시적 침수 보호 (1m 깊이, 30분간)
- **IPX8**: 지속적 침수 보호 (1m 이상, 제조사 명시)
- **IPX9/IPX9K**: [고온 고압 세척](https://en.wikipedia.org/wiki/IP_code) (80°C, 8-10MPa)

### 중요 설계 고려사항
**[수압과 깊이의 관계](https://www.polycase.com/techtalk/ip-rated-enclosures/ultimate-guide-to-ip-water-resistance-ratings.html)**
```
압력 = ρgh (수심에 따른 압력 증가)
- 1m 수심: 약 0.1 bar (10kPa) 추가 압력
- 10m 수심: 약 1 bar (100kPa) 추가 압력  
- 100m 수심: 약 10 bar (1MPa) 추가 압력
```

## 🔧 방수 설계 기법

### O-링 씰링 설계
**[Parker O-Ring Handbook](https://www.fiveflute.com/guide/how-to-design-waterproof-products/) 기반 설계 원칙**

**홈(Groove) 설계**
```
압축률 = (홈 깊이 - O-링 선경) / O-링 선경 × 100%
- 권장 압축률: 15-25% (정적 씰링)
- 최대 압축률: 30% (손상 방지)
- 최소 압축률: 10% (씰링 성능 확보)
```

**재료 선택**
- **NBR (니트릴)**: 일반용, -40°C~120°C, 유압유 저항성
- **EPDM**: 내오존성, -50°C~150°C, 물/스팀 적합
- **실리콘**: 극한 온도, -60°C~200°C, 식품 등급 가능
- **FKM (바이톤)**: 고온/화학 저항, -20°C~200°C, 항공우주용

### 가스켓 설계
**[복잡한 면-면 밀봉](https://www.fictiv.com/articles/nothing-gets-in-waterproof-enclosure-design-101-and-ip68) 적용**

**설계 원칙**
- **압축 두께**: 가스켓 두께의 20-30%
- **접촉 폭**: 최소 3mm, 권장 5-8mm
- **표면 조도**: Ra 1.6μm 이하 (밀봉면)
- **재료**: 실리콘, 네오프렌, EPDM, 폴리우레탄

### 하우징 설계
**[방수 인클로저 설계 원칙](https://jiga.io/injection-molding/designing-waterproof-enclosure-a-guide/)**

**구조적 고려사항**
- **리브 설계**: 가스켓 안착용 홈 가공
- **체결 방식**: 균등한 압축력 분산 (최소 4점 체결)
- **드레인**: 응축수 배출 경로 설계
- **케이블 입구**: IP 등급 커넥터 또는 케이블 글랜드 사용

## 🛠️ 재료 및 부품

### 방수 커넥터
**IP 등급별 커넥터 선택**
- **IP65**: 실외 설치, 분사수 보호
- **IP67**: 일시 침수 환경, 1m/30분
- **IP68**: 지속 침수 환경, 수중 장비
- **IP69K**: 고압 세척 환경, 식품 가공

### 케이블 글랜드
**[씰링 메커니즘](https://blog.epectec.com/mastering-ip-ratings-guide-to-waterproof-and-dustproof-designs)**
- **압축 글랜드**: 케이블 외피 압축 밀봉
- **분할 글랜드**: 다중 케이블 개별 밀봉
- **스트레인 릴리프**: 케이블 손상 방지

## ✅ 테스트 및 검증

### IP 테스트 방법
**[IEC 60529 테스트 절차](https://www.boulderes.com/resource-library/making-things-waterproof-ip-ratings-explained)**

**IPX7 테스트 (1m 침수)**
```
테스트 셋업:
- 투명 튜브 1.4-1.5m 높이
- 일정 수압 1.4-1.5 psi (약 10kPa)
- 테스트 시간: 30분
- 수온: 실온 (15-35°C)
```

**검증 방법**
- **수분 감지지**: 색상 변화로 침수 확인
- **전기적 테스트**: 절연 저항 측정
- **기능 테스트**: 정상 동작 확인

---

## 🔗 연결 분야

### 관련 설계 기법
- **[[방진 설계]]**: 고체 입자 보호와 복합 적용
- **[[내식 설계]]**: 수분으로 인한 부식 방지
- **[[구조설계]]**: 하우징 강도 및 밀봉 구조

### 응용 분야
- **[[수중로봇]]**: 심해 방수 설계, 고압 환경 대응
- **[[실외 장비]]**: 기상 조건 대응, 장기 신뢰성
- **[[산업 장비]]**: 세척/소독 환경, 화학 저항성

### 관련 표준
- **IEC 60529**: IP 등급 국제 표준
- **ISO 20653**: 자동차용 IP 등급 (IP69K 포함)
- **NEMA 250**: 북미 등급 체계 (IP65≈NEMA4)

#방수설계 #IP등급 #씰링 #O링 #가스켓 #하우징설계 #방수커넥터
