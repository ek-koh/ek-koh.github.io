---
title:  "함수적 기법 (2)"
excerpt: "고급 파이썬 학습: 함수적 기법 (2)"
toc: true
toc_sticky: true

categories:
  - Python
tags:
  - [Python, Function, Comprehension, Generator, Lambda, Callables, Decorator]
last_modified_at: 2020-07-08 16:33:54
---
참고자료: Functional Programming in Python (by. David Mertz)

## 0. 함수형 프로그래밍
- 파이썬은 순수한 functional paradigm 언어는 아니다.
- 하지만 파이썬은 다양한 방식의 프로그래밍을 지원하므로 어떤 것을 사용할지는 선택!
- 함수형 프로그래밍에서 연속된 실행은 합성함수로 한다.

## 1. Encapsulation을 하는 이유
- 재사용을 하기 위해서
- 함수로 만들면 다시 사용하고자 할 때 해당 함수를 호출하기만 하면 된다.

## 2. Comprehension
- 동시에 여러 개 만드는 것
- `속도가 빠르고 간결하다.`
- list, set, dictionary는 conprehension으로 만들 수 있지만 튜플로는 만들 수 없다. (튜플 -> 제너레이터)

### (1) list comprehension
```py
a = [str(x) for x in range(10) if x%2==0]
# ['0', '2', '4', '6', '8']
```

```py
a = [x+1 for x in range(10)]
# [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

### (2) set comprehension
```py
a = {x+1 for x in range(10)}
# {0: 1, 1: 2, 2: 3, 3: 4, 4: 5, 5: 6, 6: 7, 7: 8, 8: 9, 9: 10}
```

### (3) dictionary comprehensino
```py
a = {x: x+1 for x in range(10)}
# {1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
```

## 3. Generator
next() 사용 가능

### (1) tuple을 통해 만드는 방식
```py
a = (x for x in range(3))

next(a)
```

### (2) yield를 통해 만드는 방식
```py
def b():
#     yield 1
#     yield 2
    yield from range(10)

t = b()
next(t)
```


## 4. Recursion
- 수학과 유사하기 때문에 수학식으로 표현될 수 있으면 Recursion을 사용할 수 있다.
- Recursion은 속도가 느리고 메모리를 차지하기 때문에 파이썬에서는 사용하지 않는 것이 좋다.


## 5. lambda
- 메모리에 올라가지 않는 휘발성이기 때문에 일회용으로 사용하기 위해 lambda를 사용함
    - `b = lambda : 2`처럼 람다식을 할당할 수도 있고 할당하면 메모리에 남는다.
- 람다를 사용할 때와 일반적인 함수를 사용할 때의 속도 차이는 거의 없음

## 6. Callables
### Callable이란?
- call: 함수를 호출
- callable: 함수처럼 괄호를 붙일 수 있는가? 여부

```py
a = 1
callable(a) # False
```

### Callable 유형
- Function 계열
    ```py
    def f():
        return 1

    callable(f) # True
    ```

- 객체 클래스를 인스턴스할 때
- __call__이 정의되어 있는 클래스의 인스턴스

### First Class Function
- 값처럼 쓸 수 있음
    ```py
    a = [len, str]
    a[0]([1,2,3])
    # 3
    ```

## 7. Lazy Evaluation
- iterator로 변신시키면 구현하기 쉽다.
- generator를 만들거나 generator 표현식(튜플 형태의 컴프리헨션)을 통해 할 수도 있다.

## 8. 데코레이터(@)
- 기존에 만들어졌던 함수의 기능을 추가하거나 변경할 때 사용할 수 있음
- 메소드에 데코레이터를 붙이면 괄호 없이 사용할 수 있음

### 데코레이터 기본 생성 규칙
1. 맨 밖에 있는 함수(Wrapper)는 인자로 fucntion을 받아야 한다.
2. 인자로 받은 function은 중첩된 함수 안에서 실행되어야 한다.
3. 리턴은 함수로 리턴되어야 한다.

그러면 `@이름`으로 바꿀 수 있다.

```py
def foo(fn):
    def inner():
        print('About to call function.')
        fn()
        print('Finished calling function.')
    return inner
```
```py
# 여기에 있는 함수를 foo()에 집어넣는다.
@foo
def bar():
    print('Calling function bar.')

bar()
# About to call function.
# Calling function bar.
# Finished calling function.
```
### 2중구조
- 인자를 몇 개를 받던지 범용적으로 적용되게 하기 위해 `*t, **s` 사용
- y.__name__하면 y가 아니라 inner가 나오기 때문에 기능을 변경시키기 위해 다시 decorator를 씀
    - @wraps
  
```py
from functools import wraps

def author(func):
    @wraps(func)
    def inner(*t, **s):
        print('by author')
        func(*t, **s)
    return inner
```
```py
@author
def y(t):
    print(t)

y(3)
# by author
# 3
```



### 3중구조
- 조건을 하나 더 붙일 때 3중구조를 사용
- `if tt==7 ~`와 같이 사용할 수 있음
    ```py
    def outer(tt):
        def wrapper(func):
            @wraps(func)
            def inner(*t, **s):
                func(*t, **s)
                print(tt)
            return inner
        return wrapper
    ```
    
    ```py
    @outer(tt=7)
    def y(t):
        print(t)

    y(3)
    # 3
    # 7
    ```

### 여러 개의 데코레이터 사용
- 가까운 데코레이터(decorator2)부터 실행되고 그 반환값이 함수이면 다시 위에 있는 데코레이터(decorator1)를 실행하게 된다.

    ```py
    @decorator1
    @decorator2
    def ...
    ```





