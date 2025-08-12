# Eigen C++ 라이브러리 체트시트

**출처**: GitHub - Eigen-Cheatsheet  
**원문 링크**: https://github.com/zxl19/Eigen-Cheatsheet  
**카테고리**: 프로그래밍/C++  
**태그**: #개발 #C++ #수학 #선형대수

## Eigen 라이브러리 개요

Eigen은 C++ 템플릿 라이브러리로 선형 대수학(행렬, 벡터, 수치 해석기, 관련 알고리즘)을 위한 라이브러리입니다.

## 기본 행렬과 벡터 선언

### 행렬 선언
```cpp
Matrix<double, 3, 3> A;               // 고정 크기 3x3 행렬
Matrix<double, 3, Dynamic> B;         // 고정 행, 동적 열
Matrix<double, Dynamic, Dynamic> C;   // 완전 동적. MatrixXd와 동일
Matrix<double, 3, 3, RowMajor> E;     // 행 우선 방식 (기본값은 열 우선)
Matrix3f P, Q, R;                     // 3x3 float 행렬
```

### 벡터 선언
```cpp
Vector3f x, y, z;      // 3x1 float 벡터
RowVector3f a, b, c;   // 1x3 float 벡터  
VectorXd v;            // 동적 크기 double 벡터
```

## 기본 연산

### 크기와 접근
```cpp
x.size()        // 벡터 크기
C.rows()        // 행의 수
C.cols()        // 열의 수
x(i)            // i번째 원소 (0-based)
C(i,j)          // (i,j) 원소
```

### 크기 조정
```cpp
A.resize(4, 4); // 런타임 오류 가능 (고정 크기 행렬인 경우)
B.resize(4, 9); // 동적 크기 행렬만 가능
```

## 특수 행렬 생성

```cpp
MatrixXd::Identity(rows,cols)    // 단위 행렬
MatrixXd::Zero(rows,cols)        // 영 행렬
MatrixXd::Ones(rows,cols)        // 모든 원소가 1인 행렬
MatrixXd::Random(rows,cols)      // 랜덤 행렬 (-1, 1 균등분포)
VectorXd::LinSpaced(size,low,high) // 등간격 벡터
```

## 행렬 슬라이싱과 블록

### 벡터 슬라이싱
```cpp
x.head(n)           // 첫 n개 원소
x.tail(n)           // 마지막 n개 원소  
x.segment(i, n)     // i부터 n개 원소
```

### 행렬 블록
```cpp
P.block(i, j, rows, cols)  // (i,j)부터 rows×cols 블록
P.row(i)                   // i번째 행
P.col(j)                   // j번째 열
P.leftCols(cols)           // 왼쪽부터 cols개 열
P.rightCols(cols)          // 오른쪽부터 cols개 열
P.topRows(rows)            // 위부터 rows개 행
P.bottomRows(rows)         // 아래부터 rows개 행
```

## 벡터 연산

### 내적과 외적
```cpp
Vector3d v(1, 2, 3);
Vector3d w(0, 1, 2);
double dot_product = v.dot(w);       // 내적
Vector3d cross_product = v.cross(w); // 외적 (3D만)
```

### 벡터 조작
```cpp
pos.normalize();    // 정규화
pos.norm();         // 크기 계산
pos.squaredNorm();  // 크기의 제곱
```

## 사원수(Quaternion) 연산

```cpp
Eigen::Quaterniond q(2, 0, 1, -3);
q.normalize();                        // 정규화
Eigen::Matrix3d R = q.toRotationMatrix(); // 회전 행렬로 변환

// 두 벡터 사이의 회전 설정
Eigen::Quaterniond rot;
rot.setFromTwoVectors(Eigen::Vector3d(0,1,0), pos);
```

## 로봇공학에서의 활용

### 동차좌표변환
```cpp
Eigen::Isometry3d transform = Eigen::Isometry3d::Identity();
transform.translation() = Eigen::Vector3d(x, y, z);
transform.rotation() = rotation_matrix;
```

### 자주 사용되는 벡터 컨테이너
```cpp
typedef std::vector<Eigen::Vector3d, Eigen::aligned_allocator<Eigen::Vector3d>> VectorVector3d;
typedef std::vector<Eigen::Quaterniond, Eigen::aligned_allocator<Eigen::Quaterniond>> VectorQuaterniond;
```

## 메모
로봇공학과 컴퓨터 비전에서 광범위하게 사용되는 라이브러리. 고성능과 사용 편의성을 모두 제공.