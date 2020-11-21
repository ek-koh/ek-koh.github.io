---
title:  "R로 데이터 분석하기 - 데이터 로드"
excerpt: "R을 활용한 데이터 분석 과정 중 데이터를 불러오고 데이터 형태를 확인하는 방법에 대해 정리한 글입니다."
toc: true
toc_sticky: true

categories:
  - R
tags:
  - [R, Data Analysis]
last_modified_at: 2020-11-21 16:00:44
---

## 1. 데이터 로드  

원하는 경로에서 데이터를 로드하기 위해서는 우선 Working Directory 설정이 필요하다. Working Directory는 `setwd("작업하고자 하는 위치의 경로")`를 실행하거나 R Studio의 Session -> Set Working Directory -> Choose Directory에서 선택하여 설정할 수 있다.  

먼저 텍스트 파일을 로드해보자. 텍스트 파일은 `read.table()`을 통해 로드할 수 있다. header의 디폴트 설정은 FALSE이므로 데이터의 첫 줄이 변수 이름으로 되어있다면 `header=T` 옵션을 통해 첫 줄이 헤더임을 알려주어야 한다.    

```r
read.table("txt_data.txt", header=T)
```  

csv 파일은 `read.csv()`로 로드할 수 있다. `read.csv()`는 `read.table()`과 달리 디폴트 설정이 header=TRUE이다. 따라서, 이 경우에는 첫 줄을 헤더로 인식시키기 위해 header 설정을 따로 해줄 필요는 없다.  

```r
read.csv("csv_data.csv")
```  

본격적으로 데이터 형태를 확인하기에 앞서, 불러들인 데이터는 변수에 할당해 두는 것이 좋다.  

```r
data <- read.csv("csv_data.csv")
```  

## 2. 데이터 형태 확인하기  

데이터를 불러들였다면, 이 데이터가 어떤 데이터인지 간략하게 확인해볼 필요가 있다. 이 때 주로 사용하는 함수로는 `head()`, `tail()`, `str()`, `summary()`, `names()`, `is.na()`가 있다. R에서는 데이터를 할당한 변수명을 괄호 안에 넣어 사용한다.  

- `head()`: 가장 처음의 6개 행을 보여준다. (Python에서는 5개가 기본)
- `tail()`: 가장 마지막 6개 행을 보여준다. (Python에서는 5개가 기본)
- `str()`: 데이터의 structure를 보여준다. 어떤 형태의 자료형인지, 몇 개의 행과 열로 구성되어 있는지, 어떤 변수가 있고 각 변수의 데이터 타입은 어떤지를 알 수 있다.
- `summary()`: 연속형 자료에 대해서는 최대, 최소, 4분위수, 평균, 중앙값의 기초통계량과 결측치 개수를 나타내주며 factor형 자료에 대해서는 카테고리별 빈도수를 나타내준다.
- `names()`: 변수명을 알 수 있다. 변수가 매우 많을 때 특히 유용하다.
- `is.na()`: 결측치가 있으면 TRUE, 없으면 FALSE로 반환한다. 각 컬럼별 결측치 개수를 알기 위해서는 `colSums(is.na(data))`를, 전체 결측치 개수를 알기 위해서는 `sum(is.na(data))`를 이용한다.  
