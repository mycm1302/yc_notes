# MIT Cheetah SPIne 모듈

## 개요

**SPIne**은 MIT Mini Cheetah 로봇에서 사용되는 통신 중계 모듈로, SPI 통신을 4채널 CAN 통신으로 변환하는 펌웨어입니다.

## 기본 정보

- **정식 명칭**: SPIne (SPI to quad CAN firmware)
- **기능**: SPI ↔ 4-Channel CAN Bus 중계
- **개발자**: Ben Katz (MIT)
- **플랫폼**: ARM mbed
- **라이센스**: MIT License

## 주요 기능

### 통신 중계

- UP board 또는 Raspberry Pi와 4개 CAN 버스 간 통신 중계
- SPI 프로토콜을 CAN 프로토콜로 변환
- 로봇의 상위 제어 보드와 각 액추에이터 간 통신 담당

### 하드웨어 호환성

- UP board 지원
- Raspberry Pi 지원
- mbed 호환 마이크로컨트롤러

## 개발 이력

|날짜|버전|주요 변경사항|
|---|---|---|
|2017-05-26|rev 0|최초 커밋|
|2017-11-18|rev 1|동작 버전 완성|
|2017-11-29|rev 2|클래스 데모 코드|
|2018-05-02|rev 3|UP board 연결 시 SPI 초기화 버그 수정|
|2018-05-02|rev 4|로봇에서 동작 확인|
|2018-07-15|rev 5-7|디버깅 및 새 로봇 버전 지원|
|2018-07-18|rev 8|데이터 출력 기능 추가|
|2019-10-04|rev 9|Mini Cheetah 새 버전용 (최대 속도 65 rad/s, 시작 지연 1초)|

## 기술적 특징

### 의존성

- **mbed-dev_spine**: 커스텀 mbed 개발 환경
- **Teleop_Controller**: 원본 프로젝트 (포크)

### 주요 개선사항

- **칩 셀렉트 버그 수정**: UP board 연결 시 SPI가 칩 셀렉트 로우 상태에서 초기화되지 않는 문제 해결
- **속도 최적화**: 최대 속도 65 rad/s 지원
- **안정성 개선**: 시작 시 1초 지연으로 초기화 안정성 향상

## 활용 사례

### MIT Mini Cheetah에서의 역할

1. **상위 제어기**: UP board/Raspberry Pi에서 SPI로 명령 전송
2. **SPIne 모듈**: SPI 신호를 CAN 신호로 변환
3. **액추에이터**: 각 다리의 모터 제어기가 CAN으로 명령 수신

### 통신 구조

```
[상위 제어 보드] --SPI--> [SPIne 모듈] --CAN--> [액추에이터들]
  (UP board/     (SPI to CAN    (4개 다리의
   Raspberry Pi)   Bridge)      모터 제어기)
```

## 관련 리소스

### 코드 저장소

- **Mbed**: `https://os.mbed.com/users/benkatz/code/SPIne/`
- **GitHub**: `bgkatz/SPIne` (UP board / raspberry pi to quad CAN Bus)

### 관련 프로젝트

- **HKC_MiniCheetah**: Mini Cheetah 액추에이터 펌웨어
- **motorcontrol**: 모터 제어 펌웨어 (GitHub: bgkatz/motorcontrol)

### 개발자 정보

- **Ben Katz**: MIT 기계공학과 석사, Mini Cheetah 주요 개발자
- **현재**: Boston Dynamics 근무 (ATLAS 프로젝트)
- **블로그**: BuildIts (build-its.blogspot.com)

## 기술 문서

### 설정 파라미터

- 최대 속도: 65 rad/s
- 시작 지연: 1초
- CAN 채널: 4개
- 통신 프로토콜: SPI Master → CAN

### 알려진 이슈 및 해결책

1. **UP board 연결 이슈**: 칩 셀렉트가 로우로 당겨진 상태에서 SPI 초기화 실패
    
    - **해결책**: 초기화 순서 변경으로 문제 해결
2. **초기화 안정성**: 시작 시 불안정한 동작
    
    - **해결책**: 1초 지연 추가로 안정성 확보

## 라이센스

MIT License로 공개되어 있어 자유롭게 사용, 수정, 배포 가능합니다.

---

_이 문서는 MIT Biomimetics Lab의 SPIne 모듈에 대한 공개 정보를 기반으로 작성되었습니다._