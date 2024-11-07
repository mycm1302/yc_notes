# VS Code LaTeX 설정 가이드

## 목차
- [간단한 설치 방법](#간단한-설치-방법)
- [상세 설치 방법](#상세-설치-방법)
- [온라인 대안](#온라인-대안)
- [설정 및 사용법](#설정-및-사용법)

## 간단한 설치 방법

### Chocolatey를 이용한 설치 (추천)
PowerShell을 관리자 권한으로 실행 후 아래 명령어 입력:
```powershell
choco install miktex
choco install vscode
choco install latex-workshop
```

## 상세 설치 방법

### 1. MiKTeX 설치
1. [miktex.org](https://miktex.org)에서 "Basic MiKTeX Installer" 다운로드
2. 기본 설정으로 설치 진행
3. 필요한 패키지는 자동으로 설치됨

### 2. VS Code 설치
1. [code.visualstudio.com](https://code.visualstudio.com)에서 VS Code 다운로드
2. LaTeX Workshop 확장 프로그램 설치

## 온라인 대안

### 1. Overleaf
- [overleaf.com](https://www.overleaf.com)
- 장점:
  - 설치 불필요
  - 브라우저에서 즉시 사용
  - 한글 지원
  - 실시간 협업 기능
  - 다양한 템플릿 제공

### 2. HackMD
- [hackmd.io](https://hackmd.io)
- 장점:
  - 마크다운 + LaTeX 수식 지원
  - 간단한 수식 작성에 적합
  - 설치 불필요
  - 가벼운 문서 작성에 최적화

## 설정 및 사용법

### VS Code 설정
settings.json에 아래 설정 추가:
```json
{
    "latex-workshop.latex.tools": [
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "%DOC%"
            ]
        }
    ],
    "latex-workshop.latex.recipes": [
        {
            "name": "latexmk",
            "tools": [
                "latexmk"
            ]
        }
    ],
    "latex-workshop.view.pdf.viewer": "tab"
}
```

### 기본 문서 템플릿
```latex
\documentclass{article}
\begin{document}
Hello World!
\end{document}
```

### 한글 문서 템플릿
```latex
\documentclass{article}
\usepackage{kotex}
\begin{document}
안녕하세요! 한글 테스트입니다.
\end{document}
```

### 단축키
- 컴파일: Ctrl + Alt + B
- PDF 미리보기: Ctrl + Alt + V

### 주의사항
- PATH 환경변수에 LaTeX 실행 파일이 등록되어 있어야 함
- 한글 사용시 kotex 패키지 필요
