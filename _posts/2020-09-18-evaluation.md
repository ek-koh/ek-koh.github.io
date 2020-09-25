---
title:  "분류 평가방법 (1) - 정확도(Accuracy), 정밀도(Precision), 재현율(Recall)"
excerpt: "머신러닝 분류 문제에서 사용되는 평가방법인 오차행렬(Confusion Matrix)과 정확도(Accuracy), 정밀도(Precision), 재현율(Recall)에 대해 정리한 글입니다."
toc: true
toc_sticky: true

categories:
  - Data Analysis
tags:
  - [Data Analysis, Machine Learning, Scikit learn, Metrics, Classification, Evaluation, Confusion Matrix, Accuracy, Precision, Recall]
last_modified_at: 2020-09-18 19:15:33
---

머신러닝의 분류 문제에서는 정확도, 정밀도, 재현율, 오차행렬, F1 스코어, ROC AUC의 여러 성능 평가 지표를 이용해 예측 성능을 평가할 수 있다. 이 때 주의할 점은 이진 분류인지의 여부를 잘 확인해 각각의 평가 지표를 적절하게 적용해야 한다는 점이며 이 포스팅에서는 특히 오차행렬과 정확도, 정밀도, 재현율의 기초적인 내용을 다룬다.  

## 1. Confusion Matrix  

Confusion Matrix(오차행렬, 혼동행렬)는 이진 분류에서 성능 지표로 잘 활용되는 지표이다. 예측 클래스와 실제 클래스의 결정 값을 Negative(0), Positive(1) 두 가지씩으로 각각 분류하고 이들의 결합에 따라 TN, FP, FN, TP로 나눈다.  

앞의 T, F는 실제 값과 예측 값이 일치하는지 여부라고 생각하고 뒤의 N, P는 예측 값의 Negative, Positive 여부라고 생각하면 쉽다.  

![image](https://user-images.githubusercontent.com/58713684/93588442-61649f00-f9e6-11ea-8303-c00bfb38db08.png)


## 2. 정확도, 정밀도, 재현율  

위에서 정의한 오차행렬을 이용해 정확도와 정밀도, 재현율을 구해볼 수 있다.  

- 정확도(Accuracy)
  - **전체 데이터 수** 중 **예측 결과와 실제 값이 동일한 건수**(TN + TP)가 차지하는 비율
  - `(TN + TP) / (TN + FP + FN + TP)`
- 정밀도(Precision)
  - **예측을 Positive로 한 대상**(FP + TP) 중 **예측과 실제 값이 Positive로 일치한 데이터**(TP)의 비율
  - `TP / (FP + TP)`
  - 양성 예측도라고도 불린다.
- 재현율(Recall)
  - **실제가 Positive인 대상**(FN + TP) 중 **예측과 실제 값이 Positive로 일치한 데이터**(TP)의 비율
  - `TP / (FN + TP)`
  - 민감도(Sensitivity), TPR(True Positive Rate)이라고도 불린다.  

유의할 점은 **불균형한 레이블 클래스**를 가지는 **이진 분류** 모델에서는 정확도만 가지고 판단할 경우 모델의 신뢰도가 떨어질 수 있는 것이다. 이런 경우, 정밀도와 재현율을 사용하는 것이 더욱 바람직하다.  

정밀도와 재현율 중 어느 것이 더 중요한지는 경우에 따라 다르다. 암 판단 모델과 같이 실제 Positive(양성)인 데이터를 Negative(음성)로 잘못 판단하는 것이 큰 문제인 경우 `재현율`이 상대적으로 더 중요해지며, 스팸메일 분류모델과 같이 실제 Negative(일반메일)인 데이터를 Positive(스팸메일)로 잘못 판단하는 것이 큰 문제인 경우 `정밀도`가 상대적으로 더 중요해지는 것이다. 물론 가장 좋은 것은 두 수치 모두 높은 경우일 것이며, 한 수치가 상대적으로 낮더라도 그 차이가 심해서는 안된다.    

사이킷런에서 정확도, 정밀도, 재현율을 구하는 방법은 다음과 같다.  

```py
# 테스트용 실제 레이블 값을 y_test, 예측값을 pred라고 하자.
from sklearn.metrics import accuracy_score, precision_score, recall_score, confusion_matrix

# 오차행렬
print(confusion_matrix(y_test, pred))
# 정확도
print(accuracy_score(y_test, pred))
# 정밀도
print(precision_score(y_test, pred))
# 재현율
print(recall_score(y_test, pred))
```   
