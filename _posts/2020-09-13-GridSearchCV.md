---
title:  "GridSearchCV"
excerpt: "사이킷런 Model Selection 모듈에서 GridSearchCV를 사용하는 방법에 대해 다룬 글입니다."
toc: true
toc_sticky: true

categories:
  - Data Analysis
tags:
  - [Data Analysis, Machine Learning, Scikit learn, Model Selection, GridSearchCV]
last_modified_at: 2020-09-13 01:43:22
---

사이킷런 Model Selection 모듈의 `GridSearchCV`를 사용하면 앞선 포스팅 ["교차검증 수행하기 - K 폴드, Stratified K 폴드, cross_val_score"](https://ek-koh.github.io/data%20analysis/cv/)에서 수행했던 교차 검증과 최적 하이퍼 파라미터 튜닝을 한 번에 수행할 수 있다. 즉, `GridSearchCV는` 교차 검증을 기반으로 하이퍼 파라미터의 최적 값을 찾게 해주는데, 데이터 세트를 교차검증을 위한 학습/테스트 세트로 자동으로 분할한 뒤에 하이퍼 파라미터 그리드에 기술된 모든 파라미터를 순차적으로 적용해 최적의 파라미터를 찾는 것이다. 만약 cv=n이라면, 개별 파라미터 조합마다 n개의 폴딩 세트를 n회에 걸쳐 학습/평가해 평균값으로 성능을 측정한다.  

--------------------------------------------------------

iris 데이터를 이용해 학습에 적용해보자.   

먼저 데이터를 로딩하고 학습 데이터와 테스트 데이터를 분리한다.  

```py
from sklearn.datasets import load_iris
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import GridSearchCV, train_test_split
from sklearn.metrics import accuracy_score
import pandas as pd

iris = load_iris()
X_train, X_test, y_train, y_test = train_test_split(iris_data.data, iris_data.target, test_size=0.2)
dtree = DecisionTreeClassifier()
```   

다음으로, parameter들을 dictionary 형태로 설정한다. Estimator별 parameter 목록은 `get_params()`로 확인할 수 있다.  

```py
dtree.get_params()
```  

```
{'ccp_alpha': 0.0,
 'class_weight': None,
 'criterion': 'gini',
 'max_depth': None,
 'max_features': None,
 'max_leaf_nodes': None,
 'min_impurity_decrease': 0.0,
 'min_impurity_split': None,
 'min_samples_leaf': 1,
 'min_samples_split': 2,
 'min_weight_fraction_leaf': 0.0,
 'presort': 'deprecated',
 'random_state': None,
 'splitter': 'best'}
```  

```py
parameters = {'max_depth':[1, 2, 3], 'min_samples_split':[2,3]}
```   
GridSearchCV 객체를 생성하자. `cv=3`으로 설정하여 param_grid의 하이퍼 파라미터들을 3개의 train, test set fold 로 나누어서 테스트를 수행하게 한다. 또한, `refit=True`(default)로 설정하여 가장 좋은 파라미터 설정으로 재학습하도록 만든다.  

```py
grid_dtree = GridSearchCV(dtree, param_grid=parameters, cv=3, refit=True, return_train_score=True)
```  

앞서 분리해놓은 붓꽃 Train 데이터로 param_grid의 하이퍼 파라미터들을 순차적으로 학습하고 평가하게 한다. GridSearchCV 결과는 `cv_results_` 라는 딕셔너리에 저장되는데, 이를 보기 쉽게 하기 위해 DataFrame으로 변환할 것이다.  

```py
grid_dtree.fit(X_train, y_train)

scores_df = pd.DataFrame(grid_dtree.cv_results_)
scores_df[['params', 'mean_test_score', 'rank_test_score', 
           'split0_test_score', 'split1_test_score', 'split2_test_score']]
```  

- `params`: 수행할 때마다 적용된 개별 하이퍼 파라미터값의 조합
- `mean_test_score`: 개별 하이퍼 파라미터별로 CV의 폴딩 테스트 세트에 대해 수행한 전체 평가의 평균값
- `rank_test_score`: 하이퍼 파라미터별로 성능이 좋은 score 순위로, 1이 가장 뛰어남
- `split?_test_score`: 폴딩 세트에서 각각 테스트한 성능 수치  

마지막으로 최적의 파라미터와 최고 정확도를 확인하고 이를 활용해 예측을 수행해보자. `refit=True`로 설정된 GridSearchCV 객체가 fit()을 수행 시 학습이 완료된 Estimator를 내포하고 있으므로 predict()를 통해 예측도 가능하다.      

```py
print(grid_dtree.best_params_) # {'max_depth': 2, 'min_samples_split': 2}
print(grid_dtree.best_score_) # 0.9417

pred = grid_dtree.predict(X_test)
print(accuracy_score(y_test,pred)) # 0.9333
```  

다음의 코드도 위와 같은 결과를 반환한다. GridSearchCV의 best_estimator_는 refit으로 이미 최적 하이퍼 파라미터로 학습이 된 Estimator이다.

```py
estimator = grid_dtree.best_estimator_

pred = estimator.predict(X_test)
print(accuracy_score(y_test,pred)) # 0.9333
```




