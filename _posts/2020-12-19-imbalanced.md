---
title:  "불균형 데이터로 머신러닝 수행하기 - 언더 샘플링(Undersampling), 오버 샘플링(Oversampling)"
excerpt: "머신러닝에서 불균형한 데이터로 학습을 하는 방법에 대해 정리한 글입니다."
toc: true
toc_sticky: true

categories:
  - Data Analysis
tags:
  - [Data Analysis, Machine Learning, Classification, Imbalanced Data, SMOTE]
last_modified_at: 2020-12-19 12:28:33
---

머신러닝을 수행하다 보면 신용카드 사기를 검출하는 경우와 같이 정상 레이블과 이상 레이블이 있고 정상 레이블의 건수가 매우 많은 경우를 종종 마주하게 된다. 이 경우, 정상 레이블에 치우친 학습을 수행하게 되기 때문에 예측 성능이 좋지 않을 수 있다. 이와 같은 불균형 데이터 세트 학습 시의 문제를 해결할 방법이 **언더 샘플링**(Undersampling)과 **오버 샘플링**(Oversampling)이다.  

## 1. 언더 샘플링(Undersampling)  

언더 샘플링(Undersampling)은 많은 레이블을 가진 데이터 세트를 적은 레이블을 가진 데이터 세트 수준으로 감소시키는 기법이다.  

이 기법을 사용하면 과도하게 많은 레이블을 가진 데이터로 학습하는 문제는 피할 수 있지만, 너무 많은 데이터를 감소시키기 때문에 많은 레이블을 가진 데이터의 경우 오히려 제대로 된 학습을 수행할 수 없기도 하다.  

## 2. 오버 샘플링(Oversampling)  

오버 샘플링(Oversampling) 기법은 적은 레이블을 가진 데이터 세트를 많은 레이블을 가진 데이터 세트 수준으로 증식하여 학습에 충분한 데이터를 확보하는 기법이다. 오버 샘플링 방식이 일반적으로 언더 샘플링보다 예측 성능이 더 유리하기 때문에 많은 경우 오버 샘플링을 주로 사용한다.  

대표적인 오버 샘플링 방식으로는 **SMOTE(Synthetic Minority Over-sampling Technique)** 방식이 있다. 동일한 데이터를 단순히 증식시켜버리면 과적합이 발생하기 때문에, SMOTE는 적은 데이터 세트에 있는 개별 데이터들의 K 최근접 이웃(K Nearest Neighbor)을 찾아 이 데이터와 K개 이웃들의 차이를 일정 값으로 만들어 기존 데이터와 약간만 차이가 나는 새로운 데이터를 생성한다.  

SMOTE를 사용하기 위해서는 먼저 패키지를 설치해야 한다. Anaconda Prompt에서 설치한다면 다음의 명령어로 설치할 수 있다.   

```
conda install -c conda-forge imbalanced-learn
```   

설치가 완료되었다면 다음과 같이 SMOTE를 임포트하고 객체를 생성한 후, `fit_sample()`로 오버샘플링을 진행한다.  
```py
from imblearn.over_sampling import SMOTE
smote = SMOTE()
X_train_over, y_train_over = smote.fit_sample(X_train, y_train)
```  

이렇게 하면 기존에 큰 차이를 보였던 각 레이블별 건수가 같아지게 되며, 전체 데이터 개수도 오버샘플링 진행에 따라 늘어난다.    

이렇게 오버샘플링한 train 데이터로 학습을 시킨 후 SMOTE를 적용하지 않은 테스트 데이터로 예측을 수행하면 된다.
