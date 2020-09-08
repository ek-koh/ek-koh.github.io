---
title:  "판다스 DataFrame과 list, dictionary, ndarray 상호 변환"
excerpt: "판다스 DataFrame과 list, dictionary, ndarray를 상호 변환하는 방법에 대해 정리한 글입니다."
toc: true
toc_sticky: true

categories:
  - Data Analysis
tags:
  - [Data Analysis, Pandas]
last_modified_at: 2020-09-08 20:10:39
---

## 1. DataFrame을 ndarray, 리스트, 딕셔너리로 변환
### (1) DataFrame -> ndarray  

많은 머신러닝 패키지(ex. 사이킷런)가 기본 데이터 형으로 Numpy를 사용하므로, 데이터 핸들링은 DataFrame으로 하더라도 다시 넘파이 ndarray로 변환해야 하는 경우가 있다.  
DataFrame을 Numpy의 ndarray로 변환하기 위해서는 DataFrame 객체의 `values` 속성을 사용할 수 있다.   

```py
df = pd.DataFrame({'col1':[1, 2, 3], 'col2':[4, 5, 6]})
array_from_df = df.values
```

### (2) DataFrame -> list  

DataFrame을 리스트로 변환하기 위해서는 `values` 속성을 이용하여 먼저 DataFrame을 ndarray로 변환한 후, `tolist()`로 리스트로 변환하면 된다.  

```py
list_from_df = df.values.tolist()
```

### (3) DataFrame -> dictionary  

DataFrame을 딕셔너리로 변환하기 위해서는 DataFrame 객체의 `to_dict()` 메서드를 이용하면 된다.  

```py
dict_from_df = df.to_dict('list')
```  

`to_dict()` 메서드의 인자로 'list'를 입력하게 되면 딕셔너리의 값이 리스트형으로 반환된다.  

## 2. ndarray, 리스트, 딕셔너리를 DataFrame으로 변환
### (1) ndarray -> DataFrame  

DataFrame 객체의 생성 인자 data로 ndarray를 입력 받고, 생성 인자 columns로 컬럼명 or 컬럼명들의 리스트를 입력하면 된다.  

```py
col_name = 'colname'
array1 = np.array([1, 2, 3])
array_df = pd.DataFrame(array1, columns=col_name)
```  

### (2) list -> DataFrame  

ndarray를 DataFrame으로 변환할 때와 유사하게, DataFrame 객체의 생성 인자 data로 리스트를 입력 받고, 생성 인자 columns로 컬럼명 or 컬럼명들의 리스트를 입력하면 된다.    

```py
col_name = ['colname1', 'colname2']
list1 = [[1, 2, 3], [4, 5, 6]]
list_df = pd.DataFrame(list1, columns=col_name)
```  

### (3) dictionary -> DataFrame  

딕셔너리의 Key로 컬럼명을, Value를 리스트(or ndarray) 형식으로 하여 컬럼 데이터를 입력한다. 이후 DataFrame 객체의 생성 인자 data로 딕셔너리를 입력 받는다.  

```py
dict = {'col1':[1, 2, 3], 'col2':[4, 5, 6], 'col3': [7, 8, 9]}
dict_df = pd.DataFrame(dict)
```


