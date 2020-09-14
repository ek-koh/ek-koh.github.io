---
title:  "사이킷런에서 데이터 전처리하기 - 데이터 인코딩"
excerpt: "사이킷런에서 LabelEncoder, OneHotEncoder를 사용해 인코딩하는 과정을 정리한 글입니다."
toc: true
toc_sticky: true

categories:
  - Data Analysis
tags:
  - [Data Analysis, Machine Learning, Scikit learn, Preprocessing, Encoding]
last_modified_at: 2020-09-14 21:37:09
---

## 1. 데이터 전처리 시에 고려할 사항
- 데이터 클린징
  - 오류 데이터를 수정한다.
- Null 값 처리
  - Null값이 포함된 데이터로는 머신러닝을 진행할 수 없기 때문이다.
- 인코딩
  - 사이킷런의 머신러닝 알고리즘은 문자열 값을 입력 값으로 허용하지 않기 때문이다.
- 스케일링
  - 단위가 다른 경우 맞춰서 비교가 가능해지도록 해줄 필요가 있다.
- 이상치 제거
  - 알고리즘의 이상 동작을 유발하지 않도록 제거해줄 필요가 있다.
- 피처 선택, 추출, 가공
  - 때로는 너무 많은 피처 활용이 성능 저하로 이어질 수 있다.

## 2. 데이터 인코딩  
### (1) Label Encoding  
레이블 인코딩은 카테고리형 피처를 코드형 숫자 값으로 변환하는 작업을 말한다. 01, 02, 03과 같은 코드 값도 문자열이므로 인코딩 작업이 필요하다.  

지도학습에서 `fit()`과 `predict()`를 사용한다면 데이터 인코딩에서는 `fit()`과 `transform()`을 사용한다.  

Label Encoding을 수행한 결과는 ndarray 형태로 반환된다.

```py
from sklearn.preprocessing import LabelEncoder

items=['TV','냉장고','전자렌지','컴퓨터','선풍기','선풍기','믹서','믹서']

encoder = LabelEncoder()
encoder.fit(items)
labels = encoder.transform(items)
print(labels) # [0 1 4 5 3 3 2 2]
print(type(labels)) # <class 'numpy.ndarray'>
```  

사이킷런의 LabelEncoder를 사용하면 인코딩 클래스 확인과 디코딩 역시 쉽게 수행할 수 있다.  

```py
from sklearn.preprocessing import LabelEncoder

items=['TV','냉장고','전자렌지','컴퓨터','선풍기','선풍기','믹서','믹서']

encoder = LabelEncoder()
encoder.fit(items)

print(encoder.classes_)
# ['TV' '냉장고' '믹서' '선풍기' '전자렌지' '컴퓨터']

print(encoder.inverse_transform([1, 1, 2, 3, 2, 5, 0])
# ['냉장고' '냉장고' '믹서' '선풍기' '믹서' '컴퓨터' 'TV']
```  

레이블 인코딩을 할 경우, 숫자의 크고 작음이 중요하지 않더라도 머신러닝 알고리즘(특히 선형회귀)에서는 이러한 크고 작음이 반영이 될 수 있다. 따라서 레이블 인코딩을 수행한 후 원-핫 인코딩을 함께 진행해줄 필요가 있다.  


### (2) One Hot Encoding  

원-핫 인코딩은 피처 값의 유형에 따라 새로운 피처를 추가해 고유 값에 해당하는 칼럼에만 1을 표시하고 나머지에는 0을 표시한다. 이렇게 하면 레이블 인코딩만 수행했을 때의 숫자의 크고 작음이 반영되는 문제를 해결할 수 있다.  

```py
from sklearn.preprocessing import LabelEncoder, OneHotEncoder
import numpy as np

# 레이블 인코딩
items=['TV','냉장고','전자렌지','컴퓨터','선풍기','선풍기','믹서','믹서']

encoder = LabelEncoder()
encoder.fit(items)
labels = encoder.transform(items)

# 컬럼 1개의 2차원 데이터로 변환 후 원-핫 인코딩
labels = labels.reshape(-1,1)
print(labels.shape) # (8, 1)

oh_encoder = OneHotEncoder()
oh_encoder.fit(labels)
oh_labels = oh_encoder.transform(labels)
print(oh_labels.shape) # (8, 6)


print(type(oh_labels)) # <class 'scipy.sparse.csr.csr_matrix'>
print(oh_labels.toarray())
```  

```
                        .
                        .
                        .
[[1. 0. 0. 0. 0. 0.]
 [0. 1. 0. 0. 0. 0.]
 [0. 0. 0. 0. 1. 0.]
 [0. 0. 0. 0. 0. 1.]
 [0. 0. 0. 1. 0. 0.]
 [0. 0. 0. 1. 0. 0.]
 [0. 0. 1. 0. 0. 0.]
 [0. 0. 1. 0. 0. 0.]]
```  

판다스의 `get_dummies()`를 이용한다면 라벨 인코딩 과정이 굳이 필요하지 않기 때문에 더 쉽게 원-핫 인코딩을 수행할 수 있다.  

```py
import pandas as pd

df = pd.DataFrame({'item':['TV','냉장고','전자렌지','컴퓨터','선풍기','선풍기','믹서','믹서'] })
pd.get_dummies(df)
```  


