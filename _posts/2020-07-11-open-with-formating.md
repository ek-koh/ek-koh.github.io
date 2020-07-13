---
title:  "Open / With / Formatting"
excerpt: "고급 파이썬 학습: Open / With / Formatting"
toc: true
toc_sticky: true

categories:
  - Python
tags:
  - [Python, Open, With, Formatting]
last_modified_at: 2020-07-11 23:37:14
---

## 1. Open

| 파일 모드 | 기능 | 설명 |
|:-------:|:----------:|:---------------|
|    r 	  | 읽기 전용 	| 파일을 읽기 전용으로 열기. 단, 파일이 반드시 있어야 하며 파일이 없으면 에러 발생 	|
|    w    | 쓰기 전용 	| 쓰기 전용으로 새 파일을 생성. 만약 파일이 있으면 내용을 덮어씀 	|
|    a    | 추가 	| 파일을 열어 파일 끝에 값을 이어 씀. 만약 파일이 없으면 파일을 생성 	|
|    x    | 배타적 생성 (쓰기) 	| 파일을 쓰기 모드로 생성. 파일이 이미 있으면 에러 발생 	|
|    r+   | 읽기/쓰기 	| 파일을 읽기/쓰기용으로 열기. 단, 파일이 반드시 있어야 하며 파일이 없으면 에러 발생 	|
|    w+   | 읽기/쓰기 	| 파일을 읽기/쓰기용으로 열기. 파일이 없으면 파일을 생성하고, 파일이 있으면 내용을 덮어씀 	|
|   a+ 	  | 추가 (읽기/쓰기) 	| 파일을 열어 파일 끝에 값을 이어 씀. 만약 파일이 없으면 파일을 생성. 읽기는 파일의 모든 구간에서 가능하지만, 쓰기는 파일의 끝에서만 가능함 	|
|   x+ 	  | 배타적 생성 (읽기/쓰기) 	| 파일을 읽기/쓰기 모드로 생성. 파일이 이미 있으면 에러 발생 	|
|   t     | 텍스트 모드 	| 파일을 읽거나 쓸 때 개행 문자 \n과 \r\n을 서로 변환 	|
|    b 	  | 바이너리 모드 	| 파일의 내용을 그대로 읽고, 값을 그대로 씀 	|


### 파일 쓰기

```py
%%writefile abcd.txt
sdfasfda
asfdas
fasdfsafdsffsfd
asdfasfasdf
```

### 파일 열기
#### 읽기모드

```py
file = open('abcd.txt')
```

- `open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)`
- 파이썬 파일의 3요소
    - 경로, 파일명, 확장자
    - 경로는 따로 설정하지 않으면 현재 내가 작업하고 있는 파일과 같은 위치



#### 쓰기모드

해당 파일이 존재하지 않는 경우에는 open()을 실행하면 해당 이름의 새로운 파일이 생성된다. 해당 파일이 존재했던 경우, 기존 파일의 내용을 지우고 새로 덮어쓴다.  

```py
# efgh.txt라는 이름의 파일이 존재하지 않았으면 이 이름의 새로운 파일 생성
file = open('efgh.txt', 'w')  # w: 쓰기모드

file.write('Hello, world!\n')      
file.write('Hello, Korea!')    

file.close()
```

데이터파일이 클 때 `close()`를 안하면 **메모리 문제**가 발생할 수 있다. 파이썬에는 사용이 끝난 메모리를 정리해주는 가비지 컬렉터가 있어 파일을 닫지 않아도 가비지 컬렉터가 파일을 닫아주기는 하지만, 너무 많은 파일을 열어두면 그만큼 메모리 공간을 차지하기 때문에 **성능에 영향**을 줄 수 있다. 또한, 파일을 닫지 않으면 데이터 쓰기가 완료되지 않을 수도 있고 파일이 잠금상태가 되어 다른 프로그램에서 사용하지 못할 수도 있다.


### 파일 내용 읽기
#### 한 줄씩 읽기
- `next()`: 내용이 다 끝나면 StopIteration 에러가 발생한다.  

    ```py
    next(file)
    ```
- `readline()`: 내용이 다 끝나면 에러 발생 없이 ''를 반환한다.  

    ```py
    file.readline()
    ```

#### 여러 줄 한꺼번에 읽기

- `readlines()`: 파일의 내용을 한 줄씩 읽어서 리스트로 가져온다.

    ```py
    file.readlines() # ['Hello, world!\n', 'Hello, Korea!']
    ```

- `read()`: 파일에서 문자열을 읽는다.
    ```py
    file.read() # 'Hello, world!\nHello, Korea!'
    ```

## 2. With 구문

- **context manager** : 해당 컨텍스트 안에서만 사용할 수 있게 한다.
- 파일을 열었을 시에 `close()`를 사용하지 않아도 되게 한다.
- `dir()`을 했을 때 `__enter__`, `__exit__`이 정의되어 있어야 with 구문을 사용할 수 있다.
    - ex. `open()`, `plt.xkcd()`

```py
with open('efgh.txt') as f:
    print(f.readlines())
```
```py
import matplotlib.pyplot as plt

with plt.xkcd():
    plt.plot([1,2,3,4])
```

## 3. Formatting

### `__repr__` vs. `__str__`

`__repr__`은 객체를 부를 때 출력되며, `__str__`은 print를 했을 때 출력된다.

```py
class X():
    def __repr__(self):  # 객체를 부를 때 출력
        return 'A'
    def __str__(self):    # print 했을 때 출력
        return 'B'

x = X()
x # A
print(x) # B
```

### Escape Sequences for Special Characters (이스케이프 문자)
이스케이프 문자는 백슬래시(\)를 사용하여 다음 문자가 특수 문자임을 알릴 수 있다 대표적인 이스케이프 문자는 다음과 같다.

이스케이프 문자 | 뜻
:--------------:|:------------|
\b|백스페이스(Backspace)
\t|탭(Tab)
\n|개행(Linefeed)
\\|역슬래시(Backslash)
\'|작은 따옴표(Single Quote)
\"|큰 따옴표(Double Quote)


### 출력방식 : r, u, b
r은 raw를 의미하고 u는 unicode, b는 byte를 말한다. 아무것도 붙이지 않을 경우 기본적으로는 unicode가 적용된다.

```py
a = r'\nabcd\tefgh'
print(a)
# \nabcd\tefgh

a = u'\nabcd\tefgh'
print(a)
a = '\nabcd\tefgh'
print(a)
#
# abcd	efgh

a = b'\nabcd\tefgh'
print(a)
# b'\nabcd\tefgh'
```

    

