---
title:  "추천 시스템 - Contents Based Filtering, Collaborative Filtering, Hybrid Filtering"
excerpt: "Contents Based Filtering, Collaborative Filtering 등 기본적인 추천 시스템 구현 방법론의 특징 및 장단점에 대해 정리한 글입니다."

categories:
  - Data Analysis
tags:
  - [Data Analysis, Recommendation]
last_modified_at: 2020-08-20 00:51:59
---
  
추천 시스템을 구현하기 위한 기본적인 방법으로는 크게 Contents Based Filtering, Collaborative Filtering 두 가지가 있다.  

------------------------------------------------  

**Contents Based Filtering**  
- 모든 유저와 아이템의 프로필 정보를 구축한 후 기존 사용이력이 있는 유저에게 이전에 선호했던 아이템과 유사한 아이템을 추천한다.
- 모든 유저와 아이템의 프로필 정보를 구축한 후 유저와 비슷한 유형의 사람들이 좋아하는 아이템을 추천한다.

여기에서는 콘텐츠의 내용을 분석해야 하기 때문에 군집분석, ANN, tf-idf 등 아이템 분석 알고리즘이 핵심이 된다. 다만, 이 방법은 방대한 유저와 아이템 모두에 대해 프로필 정보를 구축하기가 어렵다는 문제점이 있다.  

------------------------------------------------  

**Collaborative Filtering**  
- 프로필 데이터가 없더라도 적용 가능하다.
- 유저가 남긴 평점 데이터를 바탕으로 취향이 비슷한 사람들이 선호하는 아이템을 추천한다.  

여기에서는 비슷한 패턴을 가진 사용자나 항목을 추출할 수 있는 행렬분해, kNN 등의 기술이 핵심이 된다. 하지만 이 방법은 새로운 콘텐츠에 대한 추천이 어려워지거나 과거 활동 데이터가 없는 신규 유저의 경우 추천의 정확도가 떨어지는 Cold Start 문제, 사용자들의 관심이 적은 다수의 항목이 추천을 위한 충분한 정보를 제공하지 못하는 Long Tail 문제가 발생할 수 있다.  

Cold Start에 대해 좀 더 자세히 알아보자면, Cold Start에는 아이템 콜드 스타트, 이용자 콜드 스타트, 시스템 콜드 스타트가 있다. 

- 아이템 콜드 스타트 (Item Cold Start)  
새로운 콘텐츠가 제공됐을 때 충분한 수의 이용자가 새로운 콘텐츠를 소비하기 전에는 해당 콘텐츠가 추천되지 않음을 뜻하며, 새 콘텐츠가 추천 시스템에 신규 인입됐을 때 발생한다. 이 문제를 해결하기 위해서는 각 아이템의 콘텐츠 정보를 활용해볼 수 있을 것이다.

- 이용자 콜드 스타트 (User Cold Start)  
  신규 유저의 행동 패턴을 분석할 수 있을만큼 충분한 행동 데이터가 모이기 전에는 해당 유저에게 추천을 제공할 수 없음을 뜻한다. 이를 해결하기 위해서는 기본 demo 정보만을 활용하거나 개인화 추천이 아닌 인기 아이템, 평점이 높은 아이템 추천을 진행할 수도 있다. 이러한 개인화되지 않은 추천 방식으로도 어느 정도는 이용을 촉진함으로써 빠르게 이용자 콜드 스타트를 해결할 수 있다. 

- 시스템 콜드 스타트 (System Cold Start)  
  추천 서비스 자체가 출시된 지 오래되지 않아 이용자 행동 데이터가 전반적으로 부족해 추천 품질이 저하됨을 뜻한다. 이를 해결하기 위해서는 주성분분석(PCA)으로 User-Item Matrix의 차원을 줄이는 방법을 고려해볼 수 있다.  

------------------------------------------------  

Contents Based Filtering, Collaborative Filtering의 문제를 극복하기 위해 이 둘을 결합한 여러 **Hybrid Filtering** 방식도 등장하고 있다. 이용자의 선호도에 따라 두 가지 방법의 가중치를 주는 방법, 데이터가 일정량 이상 확보되면 Contents Based Filtering에서 Collaborative Filtering으로 넘어가는 방법 등이 사용된다.

 
