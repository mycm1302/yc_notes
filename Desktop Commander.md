# Desktop Commander

## 개념 정의

Desktop Commander는 **MCP 프로토콜의 서버 구현체**로, 로컬 컴퓨터에서 실행되어 AI와 시스템 자원 사이의 브리지 역할을 담당합니다. 클라우드 AI가 안전하게 로컬 파일 시스템과 터미널에 접근할 수 있도록 중개하는 핵심 컴포넌트입니다.

## 기본 정보

### 버전 정보
```
현재 버전: 0.2.8
플랫폼: Windows, macOS, Linux
기본 셸: PowerShell (Windows), bash (Unix)
```

### 주요 기능
- **파일 시스템 관리**: 읽기, 쓰기, 검색, 이동
- **프로세스 제어**: 터미널 세션, 명령어 실행
- **보안 관리**: 권한 제어, 명령어 필터링
- **설정 관리**: 경로 제한, 사용 통계

## 핵심 명령어 체계

### 파일 시스템
```
read_file          - 파일 내용 읽기
write_file         - 파일 쓰기 (청킹 권장)
search_files       - 파일명 검색
search_code        - 코드 내용 검색
list_directory     - 디렉토리 목록
```

### 프로세스 관리
```
start_process      - 새 터미널 세션 시작
interact_with_process - 대화형 명령어 실행
read_process_output   - 프로세스 출력 읽기
list_sessions         - 활성 세션 목록
```

## 설정 구조

### 보안 설정
- **blockedCommands**: 차단된 위험 명령어 목록
- **allowedDirectories**: 접근 허용 디렉토리 (빈 배열 = 전체 접근)

### 성능 설정
- **fileReadLineLimit**: 파일 읽기 라인 제한 (기본: 1000)
- **fileWriteLineLimit**: 파일 쓰기 라인 제한 (기본: 50)

## 연결 관계

**상위 개념**: [[MCP 프로토콜 (Model Context Protocol)]]
**핵심 기능**: [[로컬 파일 접근]] ← → [[터미널 세션 관리]]
**보안 측면**: [[차단 명령어 시스템]] ← → [[경로 제한 설정]]

---

**연결 노트**: [[MCP 프로토콜 (Model Context Protocol)]] | [[로컬 파일 접근]] | [[터미널 세션 관리]]

**태그**: #데스크탑매니저 #MCP서버 #로컬시스템