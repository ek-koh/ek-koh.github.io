---
title:  "Attention, Transformer"
excerpt: "빅데이터 분석 (딥러닝 / 강화학습) : Attention, Transformer"
toc: true
toc_sticky: true

categories:
  - Data Analysis
tags:
  - [Data Analysis, Seq2Seq, Attention, Transformer, BERT]
last_modified_at: 2020-08-06 09:24:40
---

본 포스팅은 [딥러닝을 이용한 자연어처리 입문](https://wikidocs.net/book/2155)을 참고하여 작성하였습니다.  

- [Seq2Seq](https://wikidocs.net/24996)
- [Attention](https://wikidocs.net/22893)
- [Transformer](https://wikidocs.net/31379)

## 1. Seq2Seq
시퀀스-투-시퀀스(Sequence-to-Sequence)는 두 개의 RNN 구조로 되어 있으며, 기본적으로 encoder와 decoder로 구성되어 있다.  

- encoder: 정보를 압축하는 역할
- decoder: 압축을 풀어 문장을 생성하는 역할  

인코더가 입력문장을 입력받아 압축하면 context vector가 생성되며, 디코더는 이를 받아 처리(번역)된 단어를 하나씩 출력한다.  

디코더에서는 문장의 시작에 해당하는 단어(`<sos>`)를 받아 다음 단어를 예측한다. 인식이 잘 되면 이것이 다시 다음 시점의 입력으로 들어가고, 이를 반복하여 문장의 끝에 해당하는 단어(`<eos>`)가 나올 때까지 진행한다.  (auto-regressive)  

- `<sos>` : start of sequence
- `<eos>`: end of sequence  

디코더에서 예측할 때 사용하는 함수는 softmax 함수이다. 디코더에서 각 시점의 RNN 셀에서 출력 벡터가 나오면, 해당 벡터는 소프트맥스 함수를 통해 출력 시퀀스의 각 단어별 확률값을 반환하고, 디코더는 출력 단어를 결정한다.  

![image](https://user-images.githubusercontent.com/58713684/89479078-da0ff300-d7cc-11ea-917a-eae590c11e0a.png)
  

디코더는 학습할 때와 예측할 때 동작방식이 다르다. 다음과 같은 예시를 보자.  
```
I am a student -> je suis étudiant
```
학습이 제대로 되지 않은 상태에서는 je가 가장 처음 단어로 예측되지 않을 수도 있을 것이다. 그러나 학습할 때는 학습이 잘 되면 가장 높은 확률로 je가 나올 거라고 가정하고 다음 입력 단어로 je를 선택한다. (`teacher forcing`)  
한편 실제 예측을 할 때는 디코더는 오직 컨텍스트 벡터와 `<sos>`만을 입력받아 다음에 올 단어를 예측하고, 이를 다음 시점의 RNN 셀에 입력으로 넣게 된다.  

## 2. Attention

### (1) Attention이란?  

Attention은 처음에는 이미지인식 분야에서 나온 매커니즘이며, Seq2Seq 모델의 문제점을 개선하기 위해 등장하였다.  

> Seq2Seq 문제점
> - 하나의 고정된 크기의 벡터에 모든 정보를 압축하려고 하니까 정보 손실이 발생
> - RNN의 고질적인 문제인 기울기 소실(Vanishing Gradient) 문제  

### (2) Attention 함수  

![image](https://user-images.githubusercontent.com/58713684/89480673-e8600e00-d7d0-11ea-9ccc-abb56838bd0d.png)  

Query, Key, Value가 입력값이 되며 Attention Value가 출력값이 된다.  

```
Attention(Q, K, V) = Attention Value
```  

Attention에서는 디코더에서 출력단어를 예측하는 매 시점마다 인코더에서의 전체 문장을 다시 한 번 참고한다.   

- from decoder: Query
- from encoder: Key, Value  

### (3) Attention 매커니즘  

Attention 매커니즘의 단계를 살펴보자.  

1. 어텐션 스코어(Attention Score)를 구한다.
2. 소프트맥스(softmax) 함수를 통해 어텐션 분포(Attention Distribution)를 구한다.
3. 각 인코더의 어텐션 가중치와 은닉 상태를 가중합하여 어텐션 값(Attention Value)을 구한다.
4. 어텐션 값과 디코더의 t 시점의 은닉 상태를 연결한다. (Concatenate)
5. 출력층 연산의 입력이 되는 값을 계산한다.
6. 5번의 결과를 출력층의 입력으로 사용한다.  

어텐션을 이용하면 기본적으로 성능은 올라가게 된다.  

## 3. Transformer

### (1) Transformer란?  

Transformer는 기존 Seq2Seq 모델에서 문제를 개선하기 위해 Attention을 쓰는 것을 넘어, Attention으로 인코더와 디코더를 만든다.  

여기서 인코더와 디코더는 여러 개가 존재할 수 있다. 인코더와 디코더를 여러 개를 두어 더 세밀하게 모델링한 것이지, 결국 인코더의 처리 결과 context vector가 나온다는 사실은 같다.    

> 하나의 인코더에서 나온 출력과 다음 인코더로의 입력이 동일한 타입 및 사이즈를 가져야 한다. 

### (2) Self-attention  

Transformer에서는 내가 속해 있는 문장의 단어들 사이의 관계성을 보고자 하며, 이를 **self-attention**이라고 한다. 각각의 인코더와 디코더 내에서 Query, Key, Value가 모두 등장하며 자기자신을 포함하는 모든 단어에 대해서 **self-attention**이 이루어진다.

> 디코더 Masked Self Attention   
![image](https://user-images.githubusercontent.com/58713684/89485305-704b1580-d7db-11ea-8c25-399661580acc.png)
  

### (3) Positional Encoding

트랜스포머는 단어 입력을 순차적으로 받는 방식이 아니기 때문에 단어의 위치 정보를 다른 방식으로 알려줄 필요가 있다. 트랜스포머는 단어의 위치 정보를 얻기 위해서 각 단어의 임베딩 벡터에 위치 정보들을 더하여 모델의 입력으로 사용하는데, 이를 `포지셔널 인코딩(positional encoding)`이라고 한다.  

![image](https://user-images.githubusercontent.com/58713684/89484149-d1251e80-d7d8-11ea-9389-60ca5de0ca86.png)  

원-핫 인코딩을 이용하여 포지셔널 인코딩을 할 수도 있지만, 그렇게는 쓰지 않는다. 학습시키는 문장보다 더 긴 문장이 들어오면 길이가 긴 것에 대해서 예측할 수가 없는 문제가 생기기 때문이다.  

그래서 사인함수, 코사인함수를 사용한 방법을 적용한다.   

### (4) Add & Norm   

**잔차연결(Residual Connection)** 은 서브층의 입력과 출력을 더하는 것을 말하며, **층 정규화 (Layer Normalization)** 는 Normalization Layer를 추가함으로써 범위가 큰 것을 한정시키는 역할을 하는 것이다.

모델링을 할 때 정규화를 하는 이유는 값이 커지면 한쪽의 가중치값이 줄어드는 문제가 생길 수 있기 때문이다. 정규화 방식으로 다음 두 가지 방식이 있다.  

- 데이터가 들어오면 입력을 한정
- Normalization Layer를 추가  

> [Transformer 모델 더 깊게 공부하기](https://nlpinkorean.github.io/illustrated-transformer/)


## 4. BERT
BERT(Bidirectional Encoder Representations from Transformer)는 트랜스포머에서 encoder만 가지고 만든 모델이다. 기계에 언어를 가르친 후 Downstream Task (ex. 기계번역, 질의응답)를 수행하게 한다.  




