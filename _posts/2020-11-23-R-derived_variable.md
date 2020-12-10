---
title:  "R로 데이터 분석하기 - 파생변수 생성"
excerpt: "R을 활용한 데이터 분석 과정 중 파생변수를 생성하는 방법에 대해 정리한 글입니다."
toc: true
toc_sticky: true

categories:
  - R
tags:
  - [R, Data Analysis, Preprocessing]
last_modified_at: 2020-11-23 18:09:04
---

## 파생변수 생성하기  

데이터분석을 하다보면 가지고 있는 변수에서 파생변수를 생성해 적용하는 것이 더 나을 때도 있다. 우선 연속형 변수를 이용해 범주형 변수처럼 만드는 방법을 확인해보자.  

weather라는 변수에 저장한 데이터프레임 중 강수량 변수인 Rain 데이터가 다음과 같이 있다고 해보자.  

```
0.0   0.0   0.4   0.3   0.0   0.0   0.1   ...
```  

이 변수를 이용해 비가 오는 경우(1), 비가 오지 않는 경우(0)으로 나눠보고 싶다면 다음과 같이 수행해볼 수 있을 것이다.  

```r
weather$Rain2 <- ifelse(weather$Rain>0,1,0)
```  

이 명령문은 weather 데이터프레임의 Rain 데이터 값이 0보다 크면 1, 그렇지 않으면 0을 가지는 Rain2라는 변수를 weather 데이터프레임에 새로 추가한다는 의미이다.  

이번에는 날짜데이터를 이용해 파생변수를 생성해보자. 예컨대 Date 데이터가 다음과 같이 있다고 해보자.  

```
20201101    20201102    20201103   20201104   20201105...
```  

이 날짜 데이터에서 연도를 추출하고 싶다면 다음과 같이 앞의 4개의 숫자를 추출하면 될 것이다.  

```r
weather$year <- substr(weather$Date,1,4)
```  

Date 데이터가 int형이더라도 `substr()`함수를 사용해서 year 변수를 생성할 수 있다. 이렇게 생성된 year변수는 character형이 된다.  

R의 lubridate 패키지를 이용하면 날짜 데이터를 보다 쉽게 처리할 수 있다. 먼저 lubridate를 설치하고 불러와보자.   

```r
install.packages("lubridate")
library(lubridate)
```  

lubridate의 `ymd()`를 사용하면 문자나 숫자로 된 날짜 데이터를 Date타입으로 바꿀 수 있다. 그 후, `year()`, `month()`, `day()`함수로 쉽게 연/월/일을 추출해낼 수 있다.  

```r
sam <- c("20201110", "20201111") # 숫자형도 가능
str(sam) # chr [1:2] "20201110" "20201111"
sam <- ymd(sam)
str(sam) # Date[1:2], format: "2020-11-10" "2020-11-11"

year(sam) # 2020 2020
month(sam) # 11 11
day(sam) # 10 11
```   

