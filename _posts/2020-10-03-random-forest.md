---
title:  "머신러닝 앙상블 학습(Ensemble Learning) - 배깅(Bagging)"
excerpt: "앙상블 학습 방식 중 하나인 배깅(Bagging)의 대표적인 알고리즘 랜덤포레스트(Random Forest)에 대해 정리한 글입니다."
toc: true
toc_sticky: true

categories:
  - Data Analysis
tags:
  - [Data Analysis, Machine Learning, Classification, Ensemble, Bagging, Random Forest, Scikit learn]
last_modified_at: 2020-10-03 22:12:41
---

앙상블 학습(Ensemble Learning) 중 배깅(Bagging)은 Bootstrap Aggregation의 줄임말로, 투표를 통해 최종 예측 결과를 결정하는 방식이다. 같은 유형의 알고리즘으로 여러 개의 분류기를 만들어, 서로 다르게 샘플링한 데이터로 학습을 수행해 보팅을 진행한다는 점에서 보팅(Voting)과 구분된다.   

## 1. 랜덤 포레스트란?  

랜덤 포레스트는 배깅(Bagging)의 대표적인 알고리즘이다. 결정 트리 기반의 랜덤 포레스트는 앙상블 알고리즘 중 비교적 빠른 수행 속도를 가진다는 장점이 있다.  

교차 검증에서는 분할하는 데이터셋 간에 중첩을 허용하지 않았지만, 랜덤 포레스트에서 사용하는 부트스트래핑(bootstraping) 분할은 여러 개의 데이터셋을 중첩되게 분리한다.

## 2. 사이킷런에서 구현하기  

사이킷런은 RandomForestClassifier 클래스를 통해 랜덤 포레스트 기반의 분류를 지원한다.  

```py
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

cancer = load_breast_cancer()

X_train, X_test, y_train, y_test = train_test_split(cancer.data, cancer.target, test_size=0.2)

rf_clf = RandomForestClassifier(random_state=0)
rf_clf.fit(X_train , y_train)
pred = rf_clf.predict(X_test)
accuracy = accuracy_score(y_test , pred)
print('랜덤 포레스트 정확도: {0:.4f}'.format(accuracy)) # 0.9298
```  

## 3. 랜덤 포레스트 하이퍼 파라미터 튜닝  

랜덤 포레스트에서 사용하는 하이퍼 파라미터는 결정 트리에서 사용되는 하이퍼 파라미터와 대부분 유사하다.    

> [결정트리 하이퍼 파라미터 설명글 보기](https://ek-koh.github.io/data%20analysis/decision-tree/#3-%EA%B2%B0%EC%A0%95%ED%8A%B8%EB%A6%AC%EC%9D%98-%EA%B3%BC%EC%A0%81%ED%95%A9)    

이 글에서는 결정 트리와 차이가 나는 파라미터들만 따로 정리했다.  

- `n_estimators` : 랜덤 포레스트에서 사용하는 결정 트리의 개수 지정(디폴트는 100)
- `max_features` : 디폴트가 None이 아니라 sqrt(=auto)

어떤 하이퍼 파라미터가 있는지 알고 싶다면 `rf_clf.get_params()`와 같이 사용해보자.   

하이퍼 파라미터 튜닝은 GridSearchCV를 이용하면 쉽게 수행할 수 있다.  

```py
from sklearn.model_selection import GridSearchCV

params = {
    'n_estimators':[100],
    'max_depth' : [4, 6, 8, 10], 
    'min_samples_leaf' : [8, 12, 18],
    'min_samples_split' : [8, 16, 20]
}
# RandomForestClassifier 객체 생성 후 GridSearchCV 수행
rf_clf = RandomForestClassifier(random_state=0, n_jobs=-1)
grid_cv = GridSearchCV(rf_clf, param_grid=params, cv=2, n_jobs=-1)
grid_cv.fit(X_train, y_train)

print('최적 하이퍼 파라미터:\n', grid_cv.best_params_)
print('최고 예측 정확도: {0:.4f}'.format(grid_cv.best_score_))
```  

```
최적 하이퍼 파라미터:
 {'max_depth': 4, 'min_samples_leaf': 8, 'min_samples_split': 8, 'n_estimators': 100}
최고 예측 정확도: 0.9539
```  

튜닝된 하이퍼 파라미터로 재학습한 후 예측과 평가를 수행해보자.  

```py
rf_clf1 = RandomForestClassifier(n_estimators=300, max_depth=4, min_samples_leaf=8, min_samples_split=8, random_state=0)
rf_clf1.fit(X_train, y_train)
pred = rf_clf1.predict(X_test)
print('예측 정확도: {0:.4f}'.format(accuracy_score(y_test, pred))) # 0.9474
```


