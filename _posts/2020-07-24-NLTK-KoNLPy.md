---
title:  "텍스트전처리 - NLTK, KoNLPy"
excerpt: "빅데이터 수집 / 전처리 : 텍스트전처리 - NLTK, KoNLPy"
toc: true
toc_sticky: true

categories:
  - Preprocessing
tags:
  - [Preprocessing, Text, NLTK, KoNLPy, Empirical Law]
last_modified_at: 2020-07-24 13:55:44
---

## 1. 텍스트 전처리
- 토큰
  - 문자열의 한 조각
  - 토큰의 단위는 어절, 음절, 단어의 나열, 구 등 여러가지가 가능하다.
- 형태소분석 
  - 토큰 분리, 어간 추출, 품사 부착, 색인, 백터화
- 구문분석
  - 문장경계인식, 구문분석, 공기어, 개체명 인식
- 의미분석
  - 대용어 해소, 의미 중의성 해결(동명이인, 이명동인)
  - 그 때, 거기가 어디인지를 알려면 그 이전의 히스토리를 분석해야 하는데 그게 어려움
- 담론분석
  - 분류, 군집, 중복, 요약, 가중치, 순위화, 평판분석, 감성분석...


## 2. NLTK
### (1) 설치 및 import
```py
!pip install nltk

import nltk

nltk.download('gutenberg')
nltk.download('stopwords')
nltk.download('punkt')
nltk.download('averaged_perceptron_tagger')
```

### (2) Corpus (말뭉치)

```py
from nltk.corpus import gutenberg

corpus = gutenberg.open(gutenberg.fileids()[0]).read()

# string 내 줄의 개수(문장 아닐 수 있음), 단어의 개수
len(corpus.splitlines()), len(corpus.split())
# (16823, 158167)
```

### (3) Tokenizing
Tokenizing은 비정형데이터를 token 단위로 나누는 방법을 말한다.  

```py
from nltk.tokenize import sent_tokenize
from nltk.tokenize import word_tokenize
from nltk.tokenize import TweetTokenizer
from nltk.tokenize import regexp_tokenize

# 문장 단위 토크나이징
print(len(sent_tokenize(corpus))) 
# 단어 단위 토크나이징
print(len(word_tokenize(corpus))) 
# 이모티콘까지 고려한 토크나이징, 짧은 글
print(len(TweetTokenizer().tokenize(corpus))) 
# 정규표현식 활용한 토크나이징
print(len(regexp_tokenize(corpus, r'\b(\w+)\b')))
```  

sent_tokenize
- 구두점을 기반으로 sentence를 자른다.
- 구두점을 사용하더라도 띄어쓰기 안돼있으면 약어로 인식한다.

```py
data = '''울릉도. 동남쪽.뱃길따라! 이백리.'''
sent_tokenize(data2)
# ['울릉도.', '동남쪽.뱃길따라!', '이백리.']
```

> 구두점 확인하기
> ```py
> from string import punctuation
> punctuation 
> # '!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~'
> # 여기에 포함되어 있어도 ^, ;는 안된다.
> ```

### (4) 빈도 확인하기

```py
from nltk import Text
emma = Text(word_tokenize(corpus))
emma # <Text: Emma by Jane Austen 1816>
len(emma), len(set(emma)) # 전체단어갯수, 고유단어갯수. 

emma.vocab() # 빈도분포

emma.vocab().most_common(10) # 가장 많이 나온 10가지

emma.count('Emma'), emma.vocab().freq('Emma'), emma.vocab().get('Emma') # (855, 0.004458117162447532, 855)

emma.similar('Emma', 5) # 위치상 Emma와 가장 유사한 단어들 : she it he i weston

emma.dispersion_plot(['Emma','Jane']) # 전체 텍스트에서 어느 위치에 각 단어들이 어떻게 분포?

```

## 3. KoNLPy
- 한글 자료를 다룰 수 있음
- 인터페이스

```
konlpy(interface)  -- Jpype -- JAVA(JVM)
```

### (1) 형태소 분석, 태깅 라이브러리
```py
from konlpy.tag import Kkma, Komoran, Hannanum, Okt
```
- **Kkma (꼬꼬마)**: 서울대학교 IDS 연구실에서 개발함
- **Komoran (코모란)**: Shineware에서 개발함
- **Hannanum (한나눔)**: KAIST Semantic Web Research Center에서 개발함
- **Open Korean Text(오픈소스 한국어 분석기)**: 과거 트위터 형태소 분석기
  
이 클래스들은 nouns(명사 추출), morphs(형태소 추출), pos(품사 부착) 메서드를 공통적으로 사용한다.    

```py
sentence = '''방역당국 "코로나19 일일 신규 확진자 수 내일 100명 넘을 듯"(종합)'''
Kkma().morphs(sentence)

print([_[1] for _ in Kkma().pos(sentence)])
# ['NNG', 'NNG', 'SS', 'NNG', 'NR', 'NR', 'NNM', 'NNG', 'MAG', 'NNG', 'NNG', 'NNG', 'NR', 'NNM', 'VV', 'ETD', 'NNB', 'SS', 'SS', 'NNG', 'SS']
```  

품사표는 다음과 같은 방법으로 확인 가능하다.  

```py
Kkma().tagset
```

### (2) kolaw, kobill

```py
from konlpy.corpus import kolaw, kobill

law = Text(word_tokenize(kolaw.open(kolaw.fileids()[0]).read()))

law.vocab().most_common(20)

# 형태소 분석 적용
corpus = '\n'.join([kobill.open(_).read() for _ in kobill.fileids()])
ma = Kkma().morphs
bill = Text(ma(corpus))

bill.vocab().N(), bill.vocab().B() # (21695, 1489)
bill.vocab().most_common(50)
```

## 4. Empirical Law
### (1) Zipf's Law
- 빈도가 높은 순으로 쭉 내려가는 그래프를 그리게 됨.
- 앞쪽에 있으면 빈도가 높은 것들 (주로 구두점, 많이 나오는 관사, to부정사, 전치사, 한글에서는 숫자나 1음절 단어들, 조사, 형식형태소 등)
  - 이러한 stopwords(불용어)는 제거한다.
- 아예 뒤쪽은 한번씩 정도밖에 안나오는 것들(저빈도 단어) : 이것도 가치는 없음)
- 의미 있는 단어들(significant words)은 중간에 있음

![image](https://user-images.githubusercontent.com/58713684/88512778-0df75580-d022-11ea-94af-1192149dd662.png)
  
  
### (2) Heap's Law

- 문서의 고유한 단어 수를 문서 길이의 함수로 설명하는 실험법

![image](https://user-images.githubusercontent.com/58713684/88513125-ae4d7a00-d022-11ea-9a73-15efd62e832e.png)
  
  



