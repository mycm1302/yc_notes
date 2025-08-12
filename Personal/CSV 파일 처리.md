# CSV 파일 처리

## 개념 정의

CSV 파일 처리는 **MCP Desktop Commander를 통해 구조화된 데이터를 분석하고 조작**하는 핵심 기능입니다. Python의 Pandas 라이브러리를 활용하여 대량의 표 형태 데이터를 효율적으로 처리할 수 있습니다.

## 기본 처리 절차

### 1단계: 환경 준비
```python
start_process("python3 -i")
interact_with_process(pid, "import pandas as pd")
interact_with_process(pid, "import numpy as np")
```

### 2단계: 파일 로드  
```python
interact_with_process(pid, "df = pd.read_csv('/absolute/path/file.csv')")
interact_with_process(pid, "print(f'Shape: {df.shape}')")
interact_with_process(pid, "print(df.columns.tolist())")
```

### 3단계: 데이터 탐색
```python
interact_with_process(pid, "print(df.head())")
interact_with_process(pid, "print(df.describe())")
interact_with_process(pid, "print(df.isnull().sum())")
```

## 일반적인 분석 작업

### 필터링 및 선택
```python
# 조건부 필터링
"filtered = df[df['column'] > value]"

# 특정 컬럼 선택  
"selected = df[['col1', 'col2', 'col3']]"
```

### 그룹화 및 집계
```python
"grouped = df.groupby('category').agg({'value': ['sum', 'mean', 'count']})"
```

### 결과 저장
```python
"result.to_csv('/path/output.csv', index=False)"
```

## 연결 관계

**기술 기반**: [[데이터 분석 워크플로]] ← → [[터미널 세션 관리]]
**파일 접근**: [[로컬 파일 접근]]

---

**연결 노트**: [[데이터 분석 워크플로]] | [[로컬 파일 접근]] | [[터미널 세션 관리]]

**태그**: #CSV #데이터분석 #Pandas