# PCB 설계 실무

> 상위: [[전자회로설계]]

실제 PCB 설계에서 필요한 실무적 가이드라인과 체크리스트를 정리한 문서입니다. 이론적 배경보다는 현장에서 바로 적용할 수 있는 구체적인 수치와 규칙들을 중심으로 구성되었습니다.

## ⚡ 도선 설계 기준

### 기본 가이드라인
- **1mm당 1A**: 1oz 구리, 외층, 10°C 온도 상승 기준의 대략적 가이드라인
- **정확한 계산**: IPC-2152/2221 표준 사용 권장
- **안전 마진**: 계산값의 20-30% 여유 확보

### IPC-2152 기반 도선 넓이 표 (1oz 구리, 외층, 10°C 온도상승)

| 전류 (A) | 도선 넓이 (mils) | 도선 넓이 (mm) |
|----------|------------------|----------------|
| 0.5      | 10               | 0.25           |
| 1.0      | 25               | 0.64           |
| 2.0      | 50               | 1.27           |
| 3.0      | 75               | 1.91           |
| 5.0      | 125              | 3.18           |
| 10.0     | 250              | 6.35           |
| 20.0     | 500              | 12.7           |

**출처**: [IPC-2152 표준](https://www.protoexpress.com/blog/how-to-optimize-your-pcb-trace-using-ipc-2152-standard/), [Altium IPC-2221 Calculator](https://resources.altium.com/p/ipc-2221-calculator-pcb-trace-current-and-heating)

### 내층 vs 외층 고려사항
- **내층**: 열 방출이 어려워 약 20-30% 더 넓은 도선 필요
- **외층**: 공기 대류로 냉각 효과 좋음
- **구리 두께**: 2oz 구리는 1oz 대비 약 40% 좁은 도선으로 동일 전류 처리 가능

**출처**: [IPC-2152 vs IPC-2221 비교](https://resources.system-analysis.cadence.com/blog/ipc-2152-vs-ipc-2221-which-to-use-for-pcb-thermal-analysis), [Circuit Digest Copper Weight](https://circuitdigest.com/calculators/pcb-trace-width-calculator)
### 전압 강하 계산
```
전압 강하 (V) = 전류 (A) × 저항 (Ω)
저항 (Ω) = ρ × L / A
```
- **ρ (구리 비저항)**: 1.7 × 10^-8 Ω·m
- **L (길이)**: 도선 길이 (m)  
- **A (단면적)**: 도선 넓이 × 구리 두께 (m²)

### 고전류 설계 기법
- **구리 부어(Pour) 활용**: 넓은 면적으로 전류 분산
- **비아 스티칭**: 다층간 열전도 개선, λ/20 ~ λ/10 간격
- **써멀 비아**: 고전력 부품 하부에 배치
- **직선 배선**: 급격한 각도 변화 최소화

**출처**: [RunTime 고전류 PCB 설계](https://runtimerec.com/optimizing-pcb-trace-width-and-spacing-for-high-current-applications/), [Sierra Circuits 고속 설계](https://www.protoexpress.com/blog/best-layout-tips-for-high-speed-and-high-current-pcb-traces/)

## 🔧 레이아웃 가이드라인

### 부품 배치 원칙
1. **신호 흐름 순서**: 입력 → 처리 → 출력 순으로 배치
2. **기능별 그룹화**: 아날로그/디지털/전원부 분리
3. **고주파 부품**: 크리스털, 클럭 소스 최대한 가까이 배치
4. **전원 관리**: DC-DC 컨버터 주변 여유 공간 확보

### 최소 설계 규칙 (일반적 PCB 제조업체 기준)
- **최소 도선 폭**: 4mil (0.1mm)
- **최소 도선 간격**: 4mil (0.1mm)  
- **최소 비아 크기**: 8mil (0.2mm)
- **최소 비아 드릴**: 6mil (0.15mm)
- **최소 패드 크기**: 12mil (0.3mm)

**출처**: [911EDA PCB 설계 규칙](https://www.911eda.com/articles/pcb-design-rules-essential/), [Stack Exchange 표준 규격](https://electronics.stackexchange.com/questions/5403/standard-pcb-trace-widths)

### 고밀도 설계 (HDI PCB)
- **마이크로비아**: 2-3mil 드릴 가능
- **최소 도선 폭**: 2.5mil까지 가능
- **제조 비용**: 일반 PCB 대비 2-3배 증가

**출처**: [SF Circuits HDI 가이드](https://www.sfcircuits.com/pcb-school/pcb-trace-widths), [JLCPCB HDI 설계](https://jlcpcb.com/blog/comprehensive-layer-stack-up-design-for-high-speed-controlled-impedance-pcbs)
## 📡 EMI/EMC 설계 규칙

### 그라운드 플레인 설계
1. **연속된 그라운드 플레인**: 분할 금지, 신호 반사 최소화
2. **그라운드 스티칭**: 다층 PCB에서 층간 연결, 200mil 이하 간격
3. **PCB 가장자리 도금**: 내부 층 방사 차단
4. **구리 면적 최대화**: 방열 및 EMI 억제 효과

**출처**: [Academy of EMC 설계 가이드](https://www.academyofemc.com/emc-design-guidelines), [Sierra Circuits EMI 팁](https://www.protoexpress.com/blog/7-pcb-design-tips-solve-emi-emc-issues/)

### 고속 신호 설계
- **차동 페어 임피던스**: USB 90Ω, Ethernet 100Ω
- **단일 신호 임피던스**: 일반적으로 50Ω 또는 75Ω
- **길이 매칭**: 차동 페어 내 스큐 5mil 이하
- **가드 트레이스**: 민감한 신호 주변에 그라운드 트레이스 배치

**출처**: [EinfoChips 고속 설계](https://www.einfochips.com/blog/high-speed-pcb-design-layer-stack-up-material-selection-and-via-types/), [Cadence 임피던스 제어](https://resources.pcb.cadence.com/blog/controlled-impedance-vs-controlled-stackup-design)

### EMI 억제 기법
1. **분리 설계**: 아날로그/디지털/전원부 물리적 분리
2. **디커플링 커패시터 배치**:
   - 0.1μF: 각 IC의 전원핀 가까이 (<5mm)
   - 10μF: 그룹당 하나씩 배치
   - 100μF: 전원 입력부에 배치
3. **클럭 신호 처리**:
   - 최단 거리 배선
   - 그라운드 플레인 위에 배치
   - 다른 신호와 최소 20mil 이상 분리

### 전원 무결성 설계
- **전원 플레인 분할**: 각 전압별 독립 영역
- **전원-그라운드 간격**: 최대한 좁게 (4-6mil)
- **리턴 패스 관리**: 신호마다 명확한 리턴 경로 확보
- **평면 분할 금지**: 신호가 평면 분할선을 지나지 않도록 설계
## ✅ 실무 체크리스트

### 설계 완료 전 필수 확인사항
- [ ] **전류 용량**: 모든 전원 라인 도선 넓이 검증
- [ ] **DRC 통과**: Design Rule Check 오류 0개
- [ ] **임피던스 매칭**: 고속 신호 임피던스 사양 만족
- [ ] **열 관리**: 고전력 부품 열 방출 경로 확보
- [ ] **제조 가능성**: 선택한 제조업체 규격 내 설계

### 비아 설계 체크리스트
- [ ] **비아 크기**: 최소 drill/pad 비율 1:2 이상
- [ ] **써멀 릴리프**: 그라운드 연결 비아에 적용
- [ ] **비아 수**: 고전류 경로에 충분한 개수 배치
- [ ] **길이 고려**: aspect ratio 10:1 이하 권장

### EMC 대응 체크리스트
- [ ] **커넥터 배치**: 한쪽 모서리에 집중 배치
- [ ] **크리스털 차폐**: 고주파 발진자 주변 그라운드 가드
- [ ] **전원 필터링**: 입력단 EMI 필터 배치
- [ ] **케이블 고려**: I/O 케이블의 공통모드 노이즈 대책

## 🏭 제조 고려사항

### 비용 최적화 팁
1. **표준 두께**: 1.6mm PCB 가장 경제적
2. **구리 두께**: 1oz가 기본, 2oz부터 추가 비용
3. **층수 최소화**: 4층으로 대부분 요구사항 만족 가능
4. **비아 종류**: 스루홀 비아가 가장 저렴

### 제조업체별 차이점
- **저가 업체**: 6/6mil 설계 규칙, 기본적 표면처리
- **중급 업체**: 4/4mil 설계 규칙, HASL/OSP 선택 가능  
- **고급 업체**: 2/2mil 설계 규칙, ENIG 표면처리 가능

**출처**: [Advanced PCB 제조업체 비교](https://www.advancedpcb.com/en-us/tools/trace-width-calculator/), [JPE PCB 특성](https://www.jpe-innovations.com/precision-point/pcb-design-typical-layout-characteristics/)

### 표면처리 선택 가이드
- **HASL**: 범용, 저비용, 평탄도 부족
- **OSP**: 경제적, 평탄하지만 보관 기간 짧음
- **ENIG**: 고급, 평탄하고 신뢰성 높지만 비용 증가
- **ImAg**: 고주파 특성 우수, 고급 애플리케이션용

**출처**: [Wevolver PCB 설계 가이드](https://www.wevolver.com/article/trace-width-vs-current-in-pcb-design), [Rush PCB EMI/EMC 전략](https://rushpcb.com/pcb-design-strategies-for-emi-emc-compliance/)
## 🤖 로봇 시스템 특화 고려사항

### 모터 드라이버 PCB 설계
- **대전류 경로**: 30A급 모터컨트롤러는 200mil (5mm) 이상 도선
- **스위칭 노이즈**: MOSFET 스위칭부 주변 구리 부어 활용
- **게이트 드라이버**: 고속 스위칭을 위한 짧은 배선 (<10mm)
- **방열 설계**: 파워 MOSFET 하부 써멀 비아 다수 배치

**출처**: [RunTime 고전류 애플리케이션](https://runtimerec.com/optimizing-pcb-trace-width-and-spacing-for-high-current-applications/), [Sierra Circuits 고전류 설계](https://www.protoexpress.com/blog/best-layout-tips-for-high-speed-and-high-current-pcb-traces/)

### 센서 인터페이스 설계
- **아날로그 신호**: 디지털부와 물리적 분리, 별도 그라운드
- **엔코더 신호**: 차동 배선으로 노이즈 내성 향상
- **전원 분리**: 센서용 클린 전원 별도 공급
- **차폐**: 민감한 센서는 그라운드 가드링 적용

**출처**: [Altium EMI 대응](https://resources.altium.com/p/emi-and-emc-compliance-101-pcb-designers), [Compliance Engineering EMC](https://www.compeng.com.au/pcb-design-guidelines-tips-for-emi-and-emc/)

### 통신 인터페이스 설계
- **CAN 버스**: 120Ω 종단저항, 차동 임피던스 120Ω 
- **I2C/SPI**: 풀업 저항 근처 배치, 짧은 배선
- **UART**: 크로스토크 방지를 위한 적절한 간격
- **USB**: 90Ω 차동 임피던스, 최대 10cm 이내 배선

**출처**: [EinfoChips 고속 인터페이스](https://www.einfochips.com/blog/high-speed-pcb-design-layer-stack-up-material-selection-and-via-types/), [JLCPCB 임피던스 설계](https://jlcpcb.com/blog/comprehensive-layer-stack-up-design-for-high-speed-controlled-impedance-pcbs)

### 전원 시스템 설계
- **배터리 관리**: 대전류 충/방전 경로 분리
- **DC-DC 컨버터**: 입력/출력 커패시터 최대한 가까이 배치
- **전원 시퀀싱**: 다양한 전압 레벨의 파워온 순서 고려
- **역전압 보호**: 다이오드 또는 MOSFET 기반 보호회로

## 🔗 참고 자료

### 표준 규격
- **IPC-2152**: 도선 전류 용량 계산 표준 (최신)
- **IPC-2221**: PCB 설계 일반 표준 (기존)
- **IPC-2141**: 임피던스 제어 설계 가이드
- **CISPR 32**: EMI 방출 기준
- **IEC 61000-4-3**: EMI 내성 시험 기준

### 온라인 계산 도구
- **[Saturn PCB Toolkit](https://saturnpcb.com/saturn-pcb-toolkit/)**: 임피던스/도선폭 계산 (무료)
- **[Advanced PCB Trace Calculator](https://www.advancedpcb.com/en-us/tools/trace-width-calculator/)**: IPC-2221 기반 (무료)
- **[JLCPCB Impedance Calculator](https://jlcpcb.com/pcb-impedance-calculator)**: 임피던스 계산 (무료)
- **[Circuit Digest Calculator](https://circuitdigest.com/calculators/pcb-trace-width-calculator)**: 종합 PCB 계산기 (무료)

### 제조업체 DRC 파일
- **[JLCPCB](https://jlcpcb.com/help/article/BGA-Design-Guidelines---PCB-Layout-Recommendations-for-BGA-packages)**: KiCad, Altium 규칙 파일 제공
- **[Sierra Circuits](https://www.protoexpress.com/kb/bga-fanout/)**: Altium, Cadence 지원
- **[Advanced Circuits](https://www.advancedpcb.com/en-us/tools/trace-width-calculator/)**: Eagle, KiCad 규칙 파일
- 대부분의 PCB 제조업체에서 CAD별 DRC 규칙 파일 제공

---

*이 문서는 실무 경험과 업계 표준을 바탕으로 정리되었습니다. 구체적인 프로젝트에서는 해당 시스템의 요구사항에 맞게 조정하여 사용하시기 바랍니다.*
## 🔌 비아 설계 실무

### 비아 전류 용량 계산
- **기본 공식**: IPC-2221 기준 A = (I/(k×ΔT^b))^(1/c)
- **표준 비아**: 12mil 드릴, 24mil 패드로 약 1.5A 처리 가능
- **고전류용**: 20mil 드릴, 40mil 패드로 약 4A 처리 가능
- **다중 비아**: 병렬 연결 시 개별 용량의 80% 합산

**출처**: [Sierra Circuits Via Calculator](https://www.protoexpress.com/blog/how-to-optimize-your-pcb-trace-using-ipc-2152-standard/), [IPC-2221 Via Current](https://rigidflexpcb.org/ipc-2221-calculator-for-pcb-trace-current-and-heating/)

### 비아 종류별 특성과 비용
- **스루홀 비아**: 가장 저렴, 모든 층 관통, 최소 8mil 드릴
- **블라인드 비아**: 표면-내층 연결, 비용 +30%, HDI 필수
- **마이크로비아**: 레이저 드릴, 2-3mil 가능, 비용 +100-200%
- **비아인패드**: BGA 필수, 구리/에폭시 충전, 비용 +50%

**출처**: [ESCATEC Via Guide](https://www.escatec.com/blog/which-pcb-vias-to-choose-how-they-affect-bga-fanout-and-layer-count), [Altium BGA Design](https://resources.altium.com/p/which-bga-pad-and-fanout-strategy-right-your-pcb)

### 써멀 비아 설계
- **배치 간격**: λ/20 ~ λ/10 (파장의 1/20 ~ 1/10)
- **고전력 IC 하부**: 최소 20개 이상 배치
- **크기**: 8-12mil 드릴이 효과적, 너무 크면 오히려 비효율
- **패턴**: 벌집 패턴이 가장 효과적

**출처**: [EMC FastPass Via Stitching](https://emcfastpass.com/the-roadmap-to-low-emi-pcb-design-9-essential-steps/), [Sierra Circuits Thermal Design](https://www.protoexpress.com/blog/best-layout-tips-for-high-speed-and-high-current-pcb-traces/)

## 🏗️ 적층 구조(Stackup) 설계 실무

### 층수별 설계 가이드라인
- **2층**: 단순 회로용, EMI 문제 많음, 비용 최저
- **4층**: 일반적 선택, Sig-GND-PWR-Sig 구조 권장
- **6층**: 고속 설계용, 여러 전원 레일 분리 가능
- **8층 이상**: 복잡한 시스템, BGA 고밀도 설계 필수

**출처**: [Andwin Stackup Guide](https://www.andwinpcb.com/pcb-stackup-design-and-impedance-calculation-a-comprehensive-guide/), [EMA Stackup Guidelines](https://www.ema-eda.com/ema-resources/blog/pcb-stackup-design-guidelines-emd)

### 비용 최적화 적층 설계
- **짝수 층**: 2,4,6,8층이 제조 표준, 홀수층은 추가비용
- **대칭성 유지**: 상하 대칭 구조로 뒤틀림 방지 필수
- **표준 유전체**: FR-4가 가장 경제적, 고주파용은 Rogers 추가비용

**출처**: [Zuken Stackup Rules](https://www.zuken.com/us/blog/pcb-stack-up-design-rules/), [JLCPCB Impedance Calculator](https://jlcpcb.com/pcb-impedance-calculator)

### 임피던스 제어 실무 수치
- **50Ω 단일선**: 가장 표준적, 대부분 고속 신호용
- **100Ω 차동**: USB, Ethernet 등 일반적 차동 신호
- **90Ω 차동**: USB 2.0 전용
- **120Ω 차동**: CAN 버스 전용

**출처**: [Cadence Controlled Impedance](https://resources.pcb.cadence.com/blog/controlled-impedance-vs-controlled-stackup-design), [EinfoChips High-Speed Design](https://www.einfochips.com/blog/high-speed-pcb-design-layer-stack-up-material-selection-and-via-types/)

## 📱 BGA 설계 실무

### BGA 팬아웃 방식별 선택 기준
- **Dog-bone 팬아웃**: 0.8mm 피치 이상, 비용 저렴
- **Via-in-pad**: 0.5mm 이하 피치, 추가 공정비용 발생
- **마이크로비아**: 0.4mm 이하 피치, HDI 필수

**출처**: [Sierra Circuits BGA Fanout](https://www.protoexpress.com/kb/bga-fanout/), [Altium BGA Design Rules](https://resources.altium.com/p/design-rules-to-fanout-a-large-bga)

### BGA 층수 계산 경험 공식
```
필요 층수 = 총 신호 핀수 / (4면 × 면당 라우팅 가능 신호수)
```
- **0.8mm 피치**: 면당 약 6-8개 신호 라우팅 가능
- **0.5mm 피치**: 면당 약 3-4개 신호 라우팅 가능
- **0.4mm 피치**: 면당 약 1-2개 신호 라우팅 가능

**출처**: [Viasion BGA Guidelines](https://www.viasion.com/blog/bga-pcb-design-guidelines-and-layout-practices/), [Charley Yap BGA Design](https://resources.altium.com/p/how-to-successfully-design-a-bga)

### BGA 써멀 관리 실무
- **써멀 패드**: 중앙 써멀 패드 주변에 20개 이상 써멀 비아
- **구리 부어**: BGA 하부 전체 구리 부어로 열 분산
- **에어플로우**: 고전력 BGA 주변 3mm 이상 여유공간 확보

**출처**: [KSG BGA Thermal](https://www.ksg-pcb.com/en/what-is-important-for-the-bga-fanout-of-sbu-superstructures/), [JLCPCB BGA Guidelines](https://jlcpcb.com/help/article/BGA-Design-Guidelines---PCB-Layout-Recommendations-for-BGA-packages)
## 💰 제조 비용 최적화 실무

### 제조업체별 능력과 비용 차이
- **저가 업체 (중국)**: 6/6mil 규칙, HASL 표면처리, 2-4층 경제적
- **중급 업체**: 4/4mil 규칙, OSP/ENIG 선택, HDI 레벨1 가능
- **고급 업체**: 2/2mil 규칙, 임의 표면처리, HDI 레벨3까지

**출처**: [Advanced PCB Trace Calculator](https://www.advancedpcb.com/en-us/tools/trace-width-calculator/), [Circuit Digest PCB Calculator](https://circuitdigest.com/calculators/pcb-trace-width-calculator)

### 비용 급증 요인들
- **3mil 이하 도선**: 제조비용 2-3배 증가
- **HDI 기술**: 마이크로비아 사용시 50-200% 비용 증가  
- **비표준 두께**: 1.6mm 외 두께는 20-30% 추가비용
- **특수 표면처리**: ENIG는 HASL 대비 30-50% 추가

**출처**: [SF Circuits PCB Cost](https://www.sfcircuits.com/pcb-school/pcb-trace-widths), [RunTime Recruitment Cost Analysis](https://runtimerec.com/optimizing-pcb-trace-width-and-spacing-for-high-current-applications/)

### DRC 규칙 최적화
```
표준 4층 PCB 권장 DRC:
- 최소 도선폭: 4mil (0.1mm)
- 최소 간격: 4mil (0.1mm)  
- 최소 비아: 8mil 드릴/20mil 패드
- 최소 환형고리: 2mil
```

**출처**: [911EDA PCB Rules](https://www.911eda.com/articles/pcb-design-rules-essential/), [Stack Exchange PCB Standards](https://electronics.stackexchange.com/questions/5403/standard-pcb-trace-widths)

## 🔍 고급 설계 기법

### 고속 신호 완전성 실무
- **길이 매칭**: 차동 페어 내 5mil 이하 스큐 유지
- **가드 트레이스**: 민감한 신호 양옆에 그라운드 배치
- **45도 배선**: 직각 배선 금지, 신호 반사 최소화
- **티어드롭**: 도선-패드 연결부 강화, 신뢰성 향상

**출처**: [Academy of EMC Guidelines](https://www.academyofemc.com/emc-design-guidelines), [Sierra Circuits EMI Tips](https://www.protoexpress.com/blog/7-pcb-design-tips-solve-emi-emc-issues/)

### 전원 무결성(PI) 설계
- **플레인 커패시턴스**: 전원-그라운드 간격 4-6mil로 최대화
- **디커플링 배치**: 0.1μF는 5mm 이내, 10μF는 그룹당 배치
- **전압 강하 한계**: 공급전압의 5% 이내 유지
- **리플 억제**: 1% 이내 목표

**출처**: [Cadence EMI Guidelines](https://resources.pcb.cadence.com/blog/2021-the-best-pcb-design-guidelines-for-reduced-emi), [Compliance Engineering EMC](https://www.compeng.com.au/pcb-design-guidelines-tips-for-emi-and-emc/)

### EMI/EMC 실무 체크포인트
- **80-20 규칙**: 회로 면적의 80%는 그라운드 플레인 유지
- **크리스털 차폐**: 20MHz 이상 크리스털 주변 그라운드 가드링
- **커넥터 배치**: 한쪽 모서리 집중, 안테나 효과 최소화
- **케이블 필터**: 보드 입구에 페라이트 비드 또는 공통모드 초크

**출처**: [EMC FastPass Design Guide](https://emcfastpass.com/the-roadmap-to-low-emi-pcb-design-9-essential-steps/), [Astrodyne EMC Guidelines](https://www.astrodynetdi.com/blog/emc-design-guidelines)

## 🧮 온라인 계산 도구 (출처별)

### 무료 계산 도구
- **[Saturn PCB Toolkit](https://saturnpcb.com/saturn-pcb-toolkit/)**: 임피던스, 도선폭, 비아 계산
- **[Advanced PCB Trace Calculator](https://www.advancedpcb.com/en-us/tools/trace-width-calculator/)**: IPC-2221 기반 도선폭
- **[JLCPCB Impedance Calculator](https://jlcpcb.com/pcb-impedance-calculator)**: 무료 임피던스 계산
- **[Sierra Circuits Via Calculator](https://www.protoexpress.com/blog/how-to-optimize-your-pcb-trace-using-ipc-2152-standard/)**: 비아 전류 용량

### 상용 도구 내장 기능
- **Altium Designer**: 통합 임피던스/DRC 엔진
- **Cadence Allegro**: 3D 전자기장 해석 내장
- **Mentor Graphics**: 열해석 시뮬레이션 지원
- **KiCad**: 무료, 기본적 DRC/임피던스 지원

**출처**: [Altium Stackup Design](https://resources.altium.com/p/pcb-stackup-impedance-calculator), [Cadence RF Guide](https://resources.pcb.cadence.com/blog/2023-rf-pcb-stackup-guide)

## 📚 업계 표준 문서 (완전한 출처)

### 필수 IPC 표준
- **[IPC-2152](https://resources.system-analysis.cadence.com/blog/ipc-2152-vs-ipc-2221-which-to-use-for-pcb-thermal-analysis)**: 최신 도선 전류 용량 표준 (2009년~)
- **[IPC-2221B](https://rigidflexpcb.org/ipc-2221-calculator-for-pcb-trace-current-and-heating/)**: PCB 설계 일반 표준 (보수적 계산)
- **IPC-6012**: 강성 PCB 제조 표준
- **IPC-7351**: 컴포넌트 랜드 패턴 표준

### EMC 관련 표준
- **CISPR 32**: 전자기 방출 한계값
- **IEC 61000-4-3**: 전자기 내성 시험
- **FCC Part 15**: 미국 EMI 규정
- **EN 55032**: 유럽 EMC 표준

**출처**: [Altium IPC Standards](https://resources.altium.com/p/using-an-ipc-2221-calculator-for-high-voltage-design), [PCB Online EMC Standards](https://www.pcbonline.com/blog/emc-pcb-design.html)

---

*본 문서의 모든 내용은 신뢰할 수 있는 업계 출처에서 수집되었으며, 각 항목마다 원본 링크가 제공됩니다. 구체적인 프로젝트 적용 시에는 해당 제조업체의 최신 규격을 확인하시기 바랍니다.*