---
title:  "Jupyter Notebook 단축키"
excerpt: "작업속도 향상을 위해 여러 Jupyter Notebook 단축키에 대해 정리한 글입니다."
toc: true
toc_sticky: true

categories:
  - Python
tags:
  - [Python, Jupyter Notebook, Shortcut]
last_modified_at: 2020-09-03 11:23:57
---

## 셀 선택 모드 vs. 코드 입력 모드  
- 셀 선택 모드: 셀이 파란색으로 표시된 상태
- 코드 입력 모드: 셀이 초록색으로 표시된 상태

```
                 Enter
            -------------->
셀 선택 모드                코드 입력 모드
            <--------------
             Esc / ctrl + m
```
> Jupyter Notebook에서 직접 어떤 단축키가 있는지 확인하고 싶다면 셀 선택 모드에서 `h`키로 단축키 목록에 접근할 수 있다.  

## 단축키
### 셀 선택 모드
- `h`: 단축키 목록 보기
- `enter`: 선택 셀의 코드 입력 모드로 돌아가기
- `dd`: 선택 셀 삭제
- `z`: 삭제한 셀 복원
- `a`: 위로 셀 추가
- `b`: 아래로 셀 추가
- `x`: 선택 셀 잘라내기
- `c`: 선택 셀 복사하기
- `v`: 선택 셀 아래에 붙여넣기
- `shift + m`: 선택 셀과 아래 셀을 합치기
  > 코드 입력 모드 `ctrl + shift + - `: 커서 위치에서 셀 둘로 나누기
- `o`: 실행결과 열기/닫기
- `m`: Markdown으로 변경
- `y`: Code로 변경
- `ctrl + s`: 파일 저장
- `shift + l`: 줄 번호 표시 토글

### 코드 입력 모드
- `Esc` or `ctrl + m`: 셀 선택 모드로 돌아가기
- `shift + enter`: 선택 셀 코드 실행 후 다음 셀로 이동(다음 셀 없으면 자동 추가)
- `ctrl + enter`: 선택 셀 코드 실행
- `alt + enter`: 선택 셀 실행 후 아래에 셀 하나 생성
- `ctrl + a`: 선택 셀의 코드 전체 선택
- `ctrl + z`: 선택 셀 내 실행 취소
- `ctrl + shift + z` or `ctrl + y`: 선택 셀 내 다시 실행
- `ctrl + d`: 커서 위치의 라인 삭제
- `ctrl + x`: 커서 위치의 라인 잘라내기
- `ctrl + c`: 커서 위치의 라인 복사하기
- `ctrl + /`: 커서 위치의 라인 주석처리
- `ctrl + shift + - `: 커서 위치에서 셀 둘로 나누기
  > 셀 선택 모드 `shift + m`: 선택 셀과 아래 셀을 합치기
- `ctrl + s`: 파일 저장
- `tab`: 자동완성 or indent추가
- `shift + tab`: 툴팁 또는 변수 상태 표시
  - `shift + tab + tab`: 설명 크게 보기  
