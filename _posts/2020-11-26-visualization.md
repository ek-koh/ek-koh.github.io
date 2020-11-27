---
title:  "R로 데이터 분석하기 - 시각화 (1) : 기본 plot 함수"
excerpt: "R을 활용한 데이터 분석 과정 중 시각화 과정에 대해 정리한 글입니다. 가장 먼저 기본 plot 함수에 대해 다룹니다."
toc: true
toc_sticky: true

categories:
  - R
tags:
  - [R, Data Analysis, Visualization]
last_modified_at: 2020-11-26 21:25:34
---

## 1. plot() 함수  

R에서 기본 plot() 함수를 사용해 시각화 하는 방법을 알아보자. 사용 데이터는 이전 글에서 사용했던 weather 데이터이며, 데이터 형태는 다음과 같다.  

```r
str(weather)
```  

```
'data.frame':	636 obs. of  9 variables:
 $ Date     : int  20160101 20160102 20160103 20160104 20160105 20160106 20160107 20160108 20160109 20160110 ...
 $ Dust     : int  56 42 86 73 31 49 40 33 39 28 ...
 $ Temp_mean: num  1.2 5.7 6.5 2 -2.7 -1.7 -3.4 -3.3 -2.1 0.3 ...
 $ Temp_min : num  -3.3 1 5.1 -2.5 -4.8 -4.9 -5.9 -6.9 -6.2 -2.7 ...
 $ Temp_max : num  4 9.5 9.4 5.3 1.5 1.7 1.4 1 2.4 3.8 ...
 $ Rain     : num  0 0 0 0 0 0 0 0 0 0 ...
 $ Humidity : num  73 76.9 80.6 54.4 39.4 54.3 51.8 49.8 57.1 42.3 ...
 $ Rain2    : num  0 0 0 0 0 0 0 0 0 0 ...
 $ year     : chr  "2016" "2016" "2016" "2016" ...
```  

여러 변수 중 평균 기온(Temp_mean) 변수와 습도(Humidity) 변수를 사용해 산점도를 그려볼 것이다. 다른 설정 없이 x값과 y값에 해당하는 변수만 입력한다면 다음과 같이 그래프를 그릴 수 있다.  

```r
plot(weather$Temp_mean, weather$Huminity)
```  

![image](https://user-images.githubusercontent.com/58713684/100352535-78e88580-3030-11eb-8b3a-a34fe3720e2f.png)  

그럼 이제 여러 조건을 적용하면서 그래프를 발전시켜보자.  

```r
plot(weather$Temp_mean, weather$Humidity,
     xlab="Temperature", ylab="Humidity",
     main="Scatter plot", col="red", cex=1,
     pch=20, xlim=c(-30,50), ylim=c(0,100))
```  

![image](https://user-images.githubusercontent.com/58713684/100356797-121a9a80-3037-11eb-9e4a-c805c008d575.png)  


각 조건들의 기능은 다음과 같다.  

- xlab, ylab : x축, y축 이름
- xlim, ylim : x축, y축 범위
- main : 그래프 제목
- col : 그래프 색깔
- cex : 점의 크기
- pch : 점의 모양


## 2. 그래프 새 창 띄우기  

시각화 작업을 하다보면 그래프를 크게 보고 싶을 때가 있다. 대부분의 경우 그래프가 나타나는 우측 하단의 plot 탭에서 `Zoom` 버튼을 누르면 크게 볼 수 있지만, 때로는 창이 해당 그래프를 그릴만큼 충분히 크지 않아 에러가 발생하기도 한다. 예를 들어, 다음과 같은 에러를 볼 수 있다.  

```
Error in plot.new() : figure margins too large
```  

이 경우, 처음부터 새 창에서 그래프를 그린다고 명령을 해주면 그래프가 잘 그려진다. 이 때 쓸 수 있는 명령어가 `dev.new()`이다.  

`dev.new()` 명령어를 실행하면 R Graphics 창이 자동으로 생성되는데 이후 에러가 났던 명령어를 다시 입력하면 기존의 plot 탭이 아니라 R Graphics 창에 그래프가 그려진다.  

