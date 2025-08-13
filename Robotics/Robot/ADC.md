# ADC

> 상위: [[전자회로설계]]

아날로그 센서 신호를 디지털로 변환하는 ADC 회로 설계입니다.

## 📊 ADC 기본 개념

### 로드셀 아날로그 신호 디지털 변환
```
로드셀 출력: ±10mV (풀스케일)
↓ 계측증폭기 (INA128, 이득 330배)
±3.3V
↓ 레벨 시프터 (+1.65V 오프셋)
0-3.3V
↓ ADC 변환
디지털 값 (12bit: 0-4095)
```

## ⚡ MCU별 ADC 특성

### Arduino (ATmega328P)
```cpp
// 10bit ADC (0-1023)
int adcValue = analogRead(A0);
float voltage = adcValue * 5.0 / 1023.0;

특징:
- 해상도: 10bit
- 기준전압: 5V (또는 3.3V)
- 채널: 6개 (A0-A5)
- 변환시간: ~100μs
```

### ESP32
```cpp
// 12bit ADC (0-4095)
adc1_config_width(ADC_WIDTH_BIT_12);
adc1_config_channel_atten(ADC1_CHANNEL_0, ADC_ATTEN_DB_11);
int adcValue = adc1_get_raw(ADC1_CHANNEL_0);

특징:
- 해상도: 12bit
- 기준전압: 3.3V
- 채널: 18개 (2개 ADC)
- 감쇠설정: 0dB, 2.5dB, 6dB, 11dB
```
### STM32
```cpp
// 12bit ADC with DMA
HAL_ADC_Start_DMA(&hadc1, (uint32_t*)adcBuffer, 100);

특징:
- 해상도: 12bit (16bit 가능)
- 기준전압: 3.3V
- 채널: 최대 24개 (3개 ADC)
- DMA: 고속 연속 변환
- 샘플링: 최대 5.14 MSPS
```

## 🔧 로드셀 ADC 구현

### 전용 ADC IC (HX711)
```cpp
// HX711 24bit ADC (로드셀 전용)
#include "HX711.h"

HX711 scale;
scale.begin(DOUT_PIN, SCK_PIN);
scale.set_scale(2280.f);  // 보정 계수
scale.tare();             // 영점 조정

float weight = scale.get_units(10);  // 10회 평균

특징:
- 해상도: 24bit
- 내장 증폭기: 32, 64, 128배
- 차동입력: 브리지 센서 직접 연결
- 디지털 필터: 내장
```

### 정밀도 향상 기법
```cpp
// 멀티 샘플링
float adcAverage(int pin, int samples) {
    long sum = 0;
    for(int i = 0; i < samples; i++) {
        sum += analogRead(pin);
        delayMicroseconds(100);
    }
    return (float)sum / samples;
}

// 이동평균 필터
class MovingAverage {
private:
    float buffer[16];
    int index = 0;
    float sum = 0;
    
public:
    float update(float value) {
        sum -= buffer[index];
        buffer[index] = value;
        sum += value;
        index = (index + 1) % 16;
        return sum / 16;
    }
};
```

---

## 🔗 연결 문서
- 상위: [[전자회로설계]]
- 관련: [[신호조절]], [[센서인터페이스]]
- 응용: [[상태추정]], [[힘토크센서]]