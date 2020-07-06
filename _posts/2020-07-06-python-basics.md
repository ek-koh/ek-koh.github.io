---
title:  "파이썬 자료구조 / 디버깅"
excerpt: "고급 파이썬 학습: 자료구조 / 디버깅"
toc: true
toc_sticky: true

categories:
  - Python
tags:
  - [Python, Basic, PEP]
last_modified_at: 2020-07-06 19:50:02
---

## 0. 명명규칙
### PEP : Python Enhancement Proposal
- PEP8: 명명 권장규칙
    - snake 방식: 소문자로 구성되어 있고 '_'로 연결
    - capsword 방식 (pascal 방식) : 첫번째 대문자로 시작
    - camel 방식 : 첫번째 문자를 소문자로 시작
        - ex. 클래스
- 실제로 파이썬에서는 capsword와 camel을 혼용함

- 상수
  - 재할당 불가능한 것을 상수라고 함
  - 파이썬은 내부적으로는 상수가 없지만, 관례상 상수'처럼' 쓰고 싶은 경우가 있음. 그럴 경우 PEP8에 의하면 모든 문자를 대문자로 써야 함

## 1. 숫자
### (1) 정수
- 파이썬에서 제공하는 일반적인 가장 큰 정수: 9223372036854775807
- 이보다 커도 파이썬의 숫자 범위는 무한대까지이기 때문에 overflow 안생김
- 다만, 이 범위 내에서는 메모리 구조를 효율적으로 관리함.

### (2) 실수
- 파이썬에서 float은 정확하게 표현되지 않음
- 근사적으로 표현
- 딥러닝에서도 정확한 값이 아니라 근사된 값을 예측하는 것
- 정확한 값을 표현하기 위해서는 다른 패키지를 사용해야 함

### (3) Boolean
- bool은 int 클래스를 상속받았음
- 즉 연산 등 int에서 지원하는 모든 기능 사용 가능

## 2. 문자열
- str, bytes, bytesarray, memoryview가 있음

## 3. Container / Collection
### (1) sequence
- 순서가 있는 container
  - 순서가 없으면 이름으로 지정해야 함
- indexing, slicing 지원
- 더하기, 곱하기 연산 가능
- **문자열, range, 리스트, 튜플**이 여기에 해당

### (2) dictionary
- 딕셔너리의 key는 유일해야하며, mutable type이 오면 안된다.
    - key에 들어갈 수 있는 것: 문자열, 숫자, 튜플
    - 단, 튜플의 원소가 mutable이면 안된다. ex. `(1, [2,3], 4)` => X
- 파이썬 내부구조가 딕셔너리로 되어 있음
- 딕셔너리 뷰: dict.keys(), dict.values(), dict.items() 메서드가 돌려주는 객체

### (3) set
- dictionary의 key값으로 구성되어 있다고 생각하면 좋음

## 기타
- 주피터노트북에서 `shift + tab`을 이용해서 설명을 확인할 때
  - init로 시작하면? : 클래스
    - 어떤 값을 기반으로 새로운 값을 만들어줌
    - ex. `float(3)`
  - init로 시작 안하면? : function or method
