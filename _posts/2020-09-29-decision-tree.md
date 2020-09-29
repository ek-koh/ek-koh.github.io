---
title:  "머신러닝 결정트리(Decision Tree) 알고리즘"
excerpt: "ML 알고리즘 중 하나인 결정트리 알고리즘에 대해 정리한 글입니다."
toc: true
toc_sticky: true

categories:
  - Data Analysis
tags:
  - [Data Analysis, Machine Learning, Classification, DecisionTree, Scikit learn]
last_modified_at: 2020-09-29 22:09:14
---

결정트리 알고리즘은 데이터에 있는 규칙을 학습을 통해 자동으로 찾아내면서 트리 기반의 분류 규칙을 만드는 알고리즘이다.

## 1. 결정트리의 구조  

- **루트 노드(Root Node)** : 트리의 최상위 노드
- **규칙 노드(Decision Node, 결정 노드)** : 규칙 조건을 생성하는 노드. 자식 노드가 있는 브랜치 노드
- **리프 노드(Leaf Node, 말단 노드)** : 최종 분류 값이 결정되는 노드. 더 이상 자식 노드가 없는 노드
- 서브 트리(Sub Tree, 브랜치) : 새로운 규칙 조건마다 생성되는 트리  

## 2. 결정트리의 예측 방식  

결정트리 알고리즘에서는 `균일도 조건`에 따라 예측이 이루어진다. 균일도 조건이란 규칙 노드(결정 노드)가 정보 균일도가 높은 데이터셋을 먼저 선택할 수 있도록 하는 규칙 조건이다. 즉, 균일도가 높아지는 방식으로 최초의 데이터셋에서 자식 데이터셋으로 쪼개고 그렇게 생성된 서브 데이터셋에서도 같은 방식을 계속해서 반복해 나가는 것이다.  

가장 좋은 규칙 조건(속성과 분할기준)을 찾기 위해서는 정보 이득(Information Gain) 지수나 지니(Gini) 계수를 사용한다.

|지표|설명|
|:---|:---|
|정보이득지수|(1 - 엔트로피 지수). 데이터 집합의 혼잡도가 높으면 엔트로피는 높고 정보이득은 낮으며, 같은 값이 섞여 있을수록 엔트로피가 낮고 정보이득이 높다. 결정트리는 **정보이득이 높은 속성을 기준으로 분할**한다.|
|지니계수|0에 가까울수록 평등하며(균일도 높으며), 1에 가까울수록 불평등하다(균일도가 낮다). 결정트리는 **지니계수가 낮은 속성을 기준으로 분할**한다.|

## 3. 결정트리의 과적합  

균일도에 따른 규칙으로만 서브 트리를 계속 생성하다보면, 피처가 많고 균일도가 다양할수록 트리가 깊어지고 복잡해지는 과적합(Overfitting) 문제가 발생한다. 모델이 복잡하면 학습 데이터셋에 대한 성능은 좋을 수 있어도 테스트 데이터를 적용했을 때는 성능이 저하되게 된다. 따라서 모델이 너무 복잡해지지 않도록 트리의 크기를 제한하는 것이 필요하기도 하다.  

과적합을 제어하기 위해 결정트리의 파라미터를 조정할 수 있다. 사이킷런의 결정트리 알고리즘 클래스에서는 다음과 같은 파라미터를 제공한다.

- `min_samples_split`
  - 노드를 분할하기 위한 최소한의 샘플 데이터 수
  - 디폴트는 2
- `min_samples_leaf`
  - 리프 노드(말단 노드)가 되기 위한 최소한의 샘플 데이터 수
  - 디폴트는 1
  - imbalanced data는 특정 클래스의 데이터가 극도로 작을 수 있기 때문에 이 파라미터를 작게 설정할 필요가 있지만, 그렇지 않은 경우에는 너무 작게 설정되어 있으면 과적합이 발생할 수 있다.
- `max_features`
  - 최적의 분할을 위해 고려할 최대 피처 개수(int형으로 지정) or 전체 피처 중 대상 피처의 비율(float형으로 지정)
  - 디폴트는 None (모든 피처 사용)
  - sqrt(=auto): sqrt(전체 피처 개수)
  - log : log2(전체 피처 개수)
- `max_depth`
  - 트리의 최대 깊이
  - 디폴트는 None (완벽한 클래스 결정 값이 될 때까지 or 노드의 데이터 개수가 `min_samples_split`보다 작아질 때까지 분할)
- `max_leaf_nodes`
  - 리프 노드(말단 노드)의 최대 개수
  - 디폴트는 None  

하이퍼 파라미터 튜닝을 간편하게 하기 위해서는 GridSearchCV를 사용해볼 수 있을 것이다.  

> [GridSearchCV에 대한 글 바로가기](https://ek-koh.github.io/data%20analysis/GridSearchCV/)

## 4. feature_importances_  

사이킷런에서는 DecisionTreeClassifier 객체의 `feature_importances_` 속성을 통해, 알고리즘이 규칙을 정할 때 중요하게 작용하는 피처를 확인해볼 수 있다. `feature_importances_`는 ndarray 형태로 값을 반환한다.  

```py
from sklearn.tree import DecisionTreeClassifier
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
%matplotlib inline

dt_clf = DecisionTreeClassifier()

iris_data = load_iris()
X_train , X_test , y_train , y_test = train_test_split(iris_data.data, iris_data.target, test_size=0.2)

dt_clf.fit(X_train , y_train)

print(dt_clf.feature_importances_)

# feature별 importance 매핑
for name, value in zip(iris_data.feature_names , dt_clf.feature_importances_):
    print('{0} : {1:.3f}'.format(name, value))
```  

```
[0.01189272 0.01667014 0.54678538 0.42465176]
sepal length (cm) : 0.012
sepal width (cm) : 0.017
petal length (cm) : 0.547
petal width (cm) : 0.425
```


