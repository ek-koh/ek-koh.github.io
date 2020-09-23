---
title:  "분류 평가방법 (2) - 정밀도(Precision), 재현율(Recall)의 트레이드오프"
excerpt: "정밀도와 재현율의 트레이드오프 관계와 분류 결정 임계값(Threshold)을 설정하는 방법에 대해 정리한 글입니다."
toc: true
toc_sticky: true

categories:
  - Data Analysis
tags:
  - [Data Analysis, Machine Learning, Scikit learn, Classification, Evaluation, Precision, Recall, Threshold, Binarizer]
last_modified_at: 2020-09-19 22:57:38
---

## 1. 정밀도, 재현율 트레이드오프  

이전 글 ["분류 평가방법 (1) - 정확도(Accuracy), 정밀도(Precision), 재현율(Recall)"](https://ek-koh.github.io/data%20analysis/evaluation/)에서 정밀도와 재현율은 경우에 따라 그 중요성이 달라진다고 하였다. 이 때, 둘 중 중요성이 높은 지표의 수치를 높이는 것이 가능한데 바로 분류 결정 임계값, 즉 Threshold를 조정하는 것이다. 다만 어떤 한 지표가 높아지면 다른 지표는 낮아지게 되는 트레이드오프 관계가 존재한다는 점에 유의해야 한다.  

기본적으로 사이킷런의 이진분류에서는 0.5(50%)를 임계값으로 설정하고 이에 따라 레이블을 예측한다. 즉, 예측 확률이 1로 예측되는 확률이 50%보다 크면 Positive(1)로, 작으면 Negative(0)으로 예측하게 되는 것이다.  

사이킷런의 `predict_proba()` 메서드를 사용하면 이 예측 확률을 직접 확인하는 것도 가능하다. `predict_proba()` 반환 값의 첫 번째 컬럼은 Negative(0)의 확률, 두 번째 컬럼은 Positive(1)의 확률을 나타낸다.    

```py
# Estimator: lr_clf
# 테스트 피처 데이터셋: X_test

pred_proba = lr_clf.predict_proba(X_test)
pred = lr_clf.predict(X_test)

pred_proba_result = np.concatenate([pred_proba, pred.reshape(-1,1)], axis=1)
print(pred_proba_result[:3])
```  

```
[[0.46162417 0.53837583 1.        ]
 [0.87858538 0.12141462 0.        ]
 [0.87723741 0.12276259 0.        ]]
```

## 2. Threshold 커스터마이징  

Threshold를 필요에 따라 조절하는 방법은 `Binarizer` 클래스를 사용하는 것이다.  

```py
from sklearn.preprocessing import Binarizer
from sklearn.metrics import accuracy_score, precision_score, recall_score, confusion_matrix
import numpy as np
 
custom_threshold = 0.5

# Positive 컬럼에 Binarizer 적용
pred_proba_1 = pred_proba[:,1].reshape(-1,1)

binarizer = Binarizer(threshold=custom_threshold).fit(pred_proba_1) 
custom_predict = binarizer.transform(pred_proba_1)

# 테스트 레이블 데이터셋: y_test

# 오차행렬
print(confusion_matrix(y_test, custom_predict))
# 정확도 : 0.8492
print(accuracy_score(y_test, custom_predict))
# 정밀도 : 0.7742
print(precision_score(y_test, custom_predict))
# 재현율 : 0.7869
print(recall_score(y_test, custom_predict))
```  

이렇게 구한 결과는 `fit()`, `predict()`로 얻은 예측값으로 정확도, 정밀도, 재현율을 구한 결과와 같다. 즉 custom_threshold 부분을 조정해준다면 원하는 분류 결정 임계값을 새로 설정할 수 있는 것이다. 리스트로 여러 임계값을 입력해놓은 후 for 문을 사용해 여러 결과를 한 번에 확인하는 것도 가능할 것이다.   

## 3. precision_recall_curve()  

for 문을 사용해 확인하지 않더라도 사이킷런의 `precision_recall_curve()` API를 사용하면 임계값 변화에 따른 정밀도와 재현율의 트레이드오프 관계를 더욱 쉽게 확인할 수 있다.  

```py
import matplotlib.pyplot as plt
import matplotlib.ticker as ticker
%matplotlib inline

def precision_recall_curve_plot(y_test, pred_proba_c1): 
    precisions, recalls, thresholds = precision_recall_curve(y_test, pred_proba_c1)
    
    plt.figure(figsize=(8,6))
    threshold_boundary = thresholds.shape[0]
    plt.plot(thresholds, precisions[0:threshold_boundary], linestyle='--', label='precision')
    plt.plot(thresholds, recalls[0:threshold_boundary],label='recall')
    
    # 그래프 설정: x scale, x/y label, legend, grid
    start, end = plt.xlim()
    plt.xticks(np.round(np.arange(start, end, 0.1),2))
    plt.xlabel('Threshold value')
    plt.ylabel('Precision and Recall value')
    plt.legend()
    plt.grid()
    plt.show()

# Estimator: lr_clf
# 테스트 피처 데이터셋: X_test
# 테스트 레이블 데이터셋: y_test

precision_recall_curve_plot(y_test, lr_clf.predict_proba(X_test)[:, 1])
```   

![image](https://user-images.githubusercontent.com/58713684/93669717-11aed200-fad1-11ea-9866-ce5c30ffd6d5.png)  




