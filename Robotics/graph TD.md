sequenceDiagram
    participant RM as 로봇 모델
    participant CM as COM 계산기
    participant DM as 동역학 계산기
    participant QPO as QP 최적화기
    participant RS as 로봇 상태

    RM->>CM: 현재 관절 상태 제공
    CM->>CM: COM 위치/속도 계산
    CM->>QPO: COM 오차 계산
    
    RM->>DM: 현재 관절 상태 제공
    DM->>DM: M(q), h(q,qdot) 계산
    DM->>QPO: 동역학 행렬 제공
    
    RM->>QPO: 휠 접촉 자코비안 계산
    
    QPO->>QPO: QP 문제 정의
    QPO->>QPO: QP 문제 해결
    QPO->>RS: 최적 관절 가속도 적용
    RS->>RM: 로봇 상태 업데이트