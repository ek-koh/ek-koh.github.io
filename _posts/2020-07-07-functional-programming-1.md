---
title:  "함수적 기법 (1)"
excerpt: "고급 파이썬 학습: 함수적 기법 (1)"
toc: true
toc_sticky: true

categories:
  - Python
tags:
  - [Python, Function, Positional, Keyword, AndOr, Global, Nonlocal, Else]
last_modified_at: 2020-07-07 23:57:21
---

## 1. 함수 생성
- `def ... return...`
- return은 생략할 수 있지만, 생략할 때는 `print()`처럼 내부적 yield가 있어야 함
    - `return NONE`을 생략한 형태임

## 2. Positional ( / ) / Keyword ( * ) 방식
- `/` : positional 방식으로만 사용할 수 있음
  - keyword 방식의 argument를 받지 않음
  - ex. `len(obj, /)`를 사용할 경우
    ```py
    len(obj=[1,2,3])
    ```
    ```
    TypeError                                 Traceback (most recent call last)
    <ipython-input-3-d05711b44b3b> in <module>
    ----> 1 len(obj = [1,2,3])

    TypeError: len() takes no keyword arguments
    ```
- `*`, `*args`, `**kwargs`
  - *: 이후부터 keyword 방식으로만 써야 함
    ```py
    def function_name(*, a):
      return a
    
    function_name(3)
    # Error
    ```
  - *a: 가변 포지션
    ```py
    def function_name(*a):
      return sum(a)

    function_name([1,2,3,4])
    function_name([1,2,3])
    ```
  - **a: 가변키워드
    ```py
    def function_name(**a):
      return a

    function_name(a=1, b=1)

    # {'a': 1, 'b': 1}
    ```
  - `*`, `**`은 같이 쓸 수도 있지만 `*`이 `**`보다 먼저 와야 함
  - unpacking
    ```py
    def x(*a):
        return a

    x(*[1,2,3,4,5])
    # (1, 2, 3, 4, 5)

    x(*{'a':1, 'b':2})
    # ('a', 'b')
    # dictionary로 넣으면 key만 나옴
    ```

## 3. 삼항연산자
- 다음과 같이 사용 가능
  ```py
  b = 3 if a>0 else 5
  ```
  ```py
  def aa(x):
    return x

  aa(3 if a>0 else 5)
  ```

## 4. and / or
- and는 앞에가 True면 뒤에 거 반환, False면 앞에 거 반환
  - -1, 1, 2, 3...은 기본적으로 True
  - 0, '', {}, (), [], None, set()...은 기본적으로 False
  ```py
  3 and 4
  # 4
  ```
- or는 앞에가 True면 앞에 거 반환, False면 뒤에 거 반환
  ```py
  3 or 4
  # 3
  ```

## 5. global / nonlocal
- global
  - global은 사용을 자제하는 것이 좋음
  ```py
  x = 1
  def a():
    x = 2
    def b():
      global x
      x = x+3
      return x
    return b()

  a() # 4
  ```
- nonlocal
  ```py
  x = 1
  def a():
    x = 2
    def b():
      nonlocal x
      x = x+3
      return x
    return b()

  a() # 5
  ```

## 6. else의 사용
### (1) for
for문에서 사용되는 else는 for문이 다 돌아가고 나서 실행되는 구문을 만들 때 사용한다.  

```py
for i in range(5):
    print(i)
else:
    print('end')
# 0
# 1
# 2
# 3
# 4
# end
```
```py
for i in range(5):
    if i==3:
        break
    print(i)
else:
    print('end')
# 0
# 1
# 2
```

### (2) while
while에서도 for문에서와 마찬가지로 else는 while문 실행을 완벽하게 수행하고 나서 실행되는 구문을 만들 때 사용한다.  

```py
i = 0
while i>5:
    print(i)
    i += 1
else:
    print('end')

# end

# 아예 안돌아가도 else는 실행됨
```

### (3) try
에러가 발생하지 않았을 경우 실행되는 구문을 만들 때 사용한다.  

```py
try:
    a = 1/0
except ZeroDivisionError:
    # except는 여러 개 사용할 수 있음.
    # 단 ZeroDevisionError처럼 이름 지정해야하고 이름 지정한 except 구문이 먼저 와야 함
    print('zeros')
except:
    print('error')
else:
    print('no error')
finally:
    # 에러가 나던지 안나던지 항상 실행
    print('always')
```

## 7. Error Check
- 강제로 에러 발생시키기
  - assert
    ```py
    a = 1
    assert a!=1
    # False가 나오면 에러 발생
    # 중간 체크 용도로 사용
    ```
  - raise
    ```py
    def x(a):
      if a==1:
        raise Exception
    
    x(1)
    # Exception
    ```
- as 사용
  ```py
  try:
      a = 1/0
  except Exception as e:
      # 설명을 출력하고 싶을 때 as 사용
      print(e)
  
  # division by zero
  ```

## 8. 리스트 컴프리헨션
- list comprehension
  ```py
  a = [x+1 for x in range(10)]
  # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
  ```
- set, dictionary도 이렇게 만들 수 있지만 tuple은 못만듦

  ```py
  a = {x: x+1 for x in range(10)}
  # {0: 1, 1: 2, 2: 3, 3: 4, 4: 5, 5: 6, 6: 7, 7: 8, 8: 9, 9: 10}
  ```

## 9. High-order Function (고차함수)
함수를 인자로 받고, 함수를 리턴할 수 있는 함수

### (1) map
```py
list(map(lambda x:x+1, [1,2,3,4,5]))
# [2, 3, 4, 5, 6]
```
```py
list(map(lambda x,y: x+y, range(10), range(10)))
# [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```


### (2) filter
```py
list(filter(lambda x:x>3, [1,2,3,4,5]))
# [4,5]
```

### (3) reduce
```py
from functools import reduce

# 1+2=3, 3+3=6, 6+4=10, 10+5=15
reduce(lambda x,y:x+y, [1,2,3,4,5])
# 15
# 이것보다는 sum()이 더 빠르기는 함
```
```py
import numpy as np
np.add.reduce([1,2,3,4,5]) 
# 15
```

### (4) partial
- 함수의 일부를 내 마음대로 바꿀 수 있음
```py
from functools import partial

def add(x,y):
    return x+y

add1 = partial(add, y=1)

add1(3)
# 4
```  

  
## 기타
### 속도체크하기
- `%%timeit`: 전체 셀의 실행시간을 알려줌
- `%timeit` : 한 줄의 실행시간을 알려줌

### 현재 커널 메모리에 할당된 변수정보 반환
- `%whos` : 변수명, 유형, 데이터 정보를 상세히 반환

### 함수명을 변수로 잘못 사용했을 때
```py
print = 1

# 원래대로
del print
```

