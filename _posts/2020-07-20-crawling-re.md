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
- `[^]` : Negation
- `^` : Beginning of string
- `$` : End of string
- `()` : Grouping

### (2) Greedy vs. Lazy
- Greedy: 가능한 마지막 매치를 찾는다.
- Lazy: 가능한 첫번째 매치를 찾는다.

### (3) re
```py
import re

re.search() # 가장 많이 쓰는 것
```


