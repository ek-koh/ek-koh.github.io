---
title:  "Git에서 커밋하기 5단계"
excerpt: "Git에서 커밋하는 기본적인 방법 5단계"
toc: true
toc_sticky: true

categories:
  - Blog
tags:
  - [Git, Basic]
last_modified_at: 2020-06-30 17:36:29
---

## Stage 1
- Git Bash를 실행하고 **cd** 명령어로 관련 위치로 이동한다.
- 현재는 minimal-mistakes이다.
## Stage 2
- **git status** 명령어를 입력해 변경된 사항을 확인한다.
## Stage 3
- **git add** 명령어를 통해 1번에서 확인한 사항을 스테이지에 올린다.
    - **git add _config.yml**과 같이 하나의 파일을 올릴 수도 있고
    - **git add .**과 같이 전체 파일을 한 번에 올릴 수도 있다.
## Stage 4
- **git commit -m "커밋에 대한 설명"** 명령어로 스테이지에 올라간 사항을 커밋한다.
## Stage 5
- **git push origin master**와 같이 커밋한 변경 사항을 원격저장소에 반영한다.
