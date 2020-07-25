---
title:  "Crawling, Regular Expression"
excerpt: "빅데이터 수집 / 전처리 : Crawling, Regular Expression"
toc: true
toc_sticky: true

categories:
  - Data Collection
tags:
  - [Data Collection, Crawling, Regular Expression]
last_modified_at: 2020-07-20 10:44:05
---


## 1. Crawling
- **crawler**: spider, bots, web crawler 등 다양한 이름으로 불린다.
- Web indexing 을 목적으로 한다.
- 처음 URL 리스트에서 시작해서 하이퍼링크들을 찾고 fetching 한다. 

## 2. BeautifulSoup vs. Scrapy
- BeautifulSoup: Parsing 목적
- Scrapy: 편하게 봇을 만들어주는 Framework

## 3. Regular Expression
- 텍스트에서 특정한 패턴을 찾기 위해 사용한다.
- 언어 처리하는 데에 정규식이 가장 빠르다.
  
![image](https://user-images.githubusercontent.com/58713684/87899874-93609000-ca8d-11ea-92db-b76e96d82775.png)

  
### (1) Meta Characters
- `.` : Any character
- `*` : Zero, one or more
- `+` : One or more
- `?` : Zero or one
- `{}` : Specified number of occurrences
- `[]` : Check any single character
- `[-]` : Range of characters
  - `[A-Z]`: ABCD...Z
  - `[a-z]`: abcd...z
  - `[ㄱ-ㅎ]`: 자음
  - `[ㅏ-ㅣ]`: 모음
  - `[가-힣]`: 음절

- `[^]` : Negation
- `^` : Beginning of string
  - ^x : SQL에서의 x%와 같다.
- `$` : End of string
  - x$: SQL에서의 %x와 같다.
- `()` : Grouping

### (2) Greedy vs. Lazy
- Greedy quantifier(탐욕적 수량자)
  - 가능한 한 가장 큰 덩어리를 찾기 위해 텍스트 마지막에서 시작해 찾는다.
  - `*`, `+`, `{n,}`이 여기에 해당한다.

- Lazy quantifier(게으른 수량자)
  - 가능한 한 최소로 일치하게 만든다.
  - 기존 수량자 뒤에 `?`를 붙여 표현한다.
  - `*?`, `+?`, `{n,}?`

  
|Greedy / Lazy|Greedy|Lazy|
|:---:|:---:|:---:|
|예시|`python <b>AB</b> and <b>CD</b>`|
|정규표현식|`<[Bb]>.*<\/[Bb]>`|`<[Bb]>.*?<\/[Bb]>`|
|결과값|`<b>AB</b> and <b>CD</b>`|`<b>AB</b>`, `<b>CD</b>`|  

### (3) 파이썬 re 모듈
파이썬에서 정규표현식을 지원하기 위해 제공하는 모듈이다.  

```py
import re
```

#### compile
```py
p = re.compile('정규표현식')
```

- `re.compile()`
  - 정규표현식을 컴파일한다.
  - 컴파일 결과로 돌려주는 객체를 사용해 match(), search(), findall(), finditer() 작업을 수행할 수 있다.
  - 정규식을 컴파일한 결과를 **패턴**이라고 한다.

#### match, search, findall, finditer, sub
- `re.match()`
  - 문자열의 처음부터 정규식과 매치하는지 조사
  - 정규식과 매치되면 match 객체를, 매치되지 않으면 None을 반환한다.
- `re.search()`
  - 문자열 전체를 검색해 정규식과 매치되는지 조사
  - 정규식과 매치되면 match 객체를, 매치되지 않으면 None을 반환한다.  
- `re.findall()`
  - 정규식과 매치되는 모든 문자열(substring)을 리스트로 반환
- `re.finditer()`
  - 정규식과 매치되는 모든 문자열(substring)을 반복 가능한 객체로 반환
- `re.sub()`
  - 문자열에서 패턴과 매치하는 텍스트를 주어진 형태로 치환한다.

#### match 객체의 메서드
match 객체는 re.match(), re.search()의 결과로 반환된 객체이다.  

- `group()`
  - 매치된 문자열을 반환
- `start()`
  - 매치된 문자열의 시작 위치 반환
- `end()`
  - 매치된 문자열의 끝 위치 반환
- `span()`
  - 매치된 문자열의 (시작, 끝)을 나타내는 튜플 반환
