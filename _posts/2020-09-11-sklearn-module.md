---
title:  "사이킷런 주요 모듈"
excerpt: "사이킷런에서 사용하는 주요 모듈에 대하여 정리한 글입니다."
toc: true
toc_sticky: true

categories:
  - Data Analysis
tags:
  - [Data Analysis, Machine Learning, Scikit learn]
last_modified_at: 2020-09-11 22:05:44
---

## 사이킷런 주요 모듈  
아래 표는 <파이썬 머신러닝 완벽 가이드>를 참조하여 정리한 것이다.  

|분류|모듈명|설명|
|:---|:----|:---|
|예제 데이터|sklearn.datasets|사이킷런에 내장되어 예제로 제공하는 데이터 세트|
|피처 처리|sklearn.preprocessing|데이터 전처리에 필요한 다양한 가공 기능 제공 (ex. 문자열 -> 숫자형 코드 값으로 인코딩, 정규화, 스케일링)|
||sklearn.feature_selection|알고리즘에 큰 영향을 미치는 피처를 우선순위대로 셀렉션하는 다양한 기능 제공|
||sklearn.feature_extraction|텍스트 데이터 or 이미지 데이터의 벡터화된 피처를 추출하는 데 사용 (ex. Count Vectorizer / Tf-idf Vectorizer 생성)|
|피처 처리 & 차원 축소|sklearn.decomposition|차원 축소와 관련된 알고리즘을 지원하는 모듈. (ex. PCA(주성분분석), NMF(음수 미포함 행렬분해), Truncated SVD(특이 값 분해))|
|데이터 분리, 검증 & 파라미터 튜닝|sklearn.model_selection|교차 검증을 위한 학습용/테스트용 데이터 분리. Grid Search로 최적 파라미터 추출 등의 API 제공|
|평가|sklearn.metrics|분류, 회귀, 클러스터링, 페어와이즈(Pairwise)에 대한 다양한 성능 측정 방법 제공 (ex. Accuracy, Precision, Recall, ROC-AUC, RMSE)|
|ML 알고리즘|sklearn.ensemble|앙상블 알고리즘 제공 (ex. 랜덤 포레스트, 에이다 부스트, 그래디언트 부스팅)|
||sklearn.linear_model|회귀 관련 알고리즘(ex. 선형 회귀, 릿지(Ridge), 라쏘(Lasso), 로지스틱 회귀), SGD(Stochastic Gradient Descent) 관련 알고리즘 제공|
||sklearn.naive_bayes|나이브 베이즈 알고리즘 제공 (ex. 가우시안 NB, 다항 분포 NB)|
||sklearn.neighbors|최근접 이웃 알고리즘 제공 (ex. K-NN)|
||sklearn.svm|서포트 벡터 머신 알고리즘 제공|
||sklearn.tree|의사 결정 트리 알고리즘 제공|
||sklearn.cluster|비지도 클러스터링 알고리즘 제공 (ex. K-평균, 계층형, DBSCAN)|
|유틸리티|sklearn.pipeline|피처 처리 등의 변환과 ML 알고리즘 학습, 예측 등을 함께 묶어 실행할 수 있는 유틸리티 제공|  









