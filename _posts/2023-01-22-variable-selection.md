---
title: "[Data Analysis] 5. Variable Selection"
categories:
  - Data Analysis
  - Variable Selection
tags:
  - Data Analysis
  - Variable Selection
  - Exhaustive Serach 
  - Forward Selection
  - Backward Elimination 
  - Stepwise Selection
  - Genetic algorithm
toc: true
toc_sticky: true
toc_label: "On This Page"
toc_icon: "bookmark"
use_math: true
---

# Chapter 5. 주요 변수 선택 기법 
## 차원축소 : Dimensionality Reduction 

### 차원의 저주 : Curse of Dimensionality 

동등한 설명력을 갖기 위해서는 변수가 증가할 때 필요한 개체의 수는 기하급수적으로 증가

![1](https://user-images.githubusercontent.com/86525868/213905886-8b70dce5-f25b-4722-ae01-4e885eb16e61.png)

**차원의 저주 (Curse of Dimensionality)** : 데이터의 차원이 높아짐에 따라 생기는 문제점 

* 변수가 증가할수록 잡음(noise)이 포함될 확률이 높아 예측 모델의 성능을 저하시킴
* 변수가 증가할수록 예측 모델의 학습과 인식 속도가 느려짐
* 변수가 증가할수록 예측 모델에 필요한 학습 지밥의 크기가 커짐 

**차원의 저주를 극복하는 방법**

* 사전 지식의 활용

* 목적 함수의 smoothness를 증가시킴

* 정량적 분석을 통한 차원 감소 

  ![2](https://user-images.githubusercontent.com/86525868/213905888-5872a81f-4aa7-4296-9353-c5afda4ef15a.png)

↳ 변수가 일정 수준 이상으로 커질 때는 예측에 필요한 변수들이 대부분으로 성능이 향상하지만 일정수준 이상으로 변수가 증가하면 변수 추가에 따른 이득보다 손해가 더 커짐 



### 차원축소 : Dimensionality Reduction 

#### 배경 

이론적으로는 변수의 수가 증가할수록 모델의 성능이 향상됨 → **변수간 독립성 만족할 때**

하지만 실제 상황에서는 변수간 독립성 가정을 위배하는 경우가 대부분이고, 노이즈의 존재 등으로 변수가의 수가 일정 수준 이상 증가하면 모델의 성능이 저하되는 경향이 있음

#### 목적

향후 분석 과정에서 성능을 저하시키지 않는 최소한의 변수 집합 판별

#### 차원축소 효과 

* 변수간 상관성을 제거하여 결과의 통계적 유의성 제고 

* 사후 처리(post-processing)의 단순화 

* 주요정보를 보존한 상태에서 중복되거나 불필요한 정보만 제거 

* 고차원의 정보를 저차원으로 축소하여 시각화(visualization)

#### 차원축소 방식

##### 교사적 차원 축소 : Supervised dimentionality reduction

* 축소된 차원의 적합성을 검증하는데 있어 예측 모델을 적용
* 동일한 데이터라도 적용되는 예측 모델에 따라 축소된 차원의 결과가 달라질 수 있음

![3](https://user-images.githubusercontent.com/86525868/213905889-24c4a774-f1ca-48af-9830-926976b16b38.png)

##### 비교사적적 차원 축소 : Unsupervised dimensionality reduction

* 축소된 차원의 적합성을 검증하는데 있어 예측 모델의 적용하지 않음
* 특정 기법에 따른 차원축소 결과는 동일함

![4](https://user-images.githubusercontent.com/86525868/213905890-5243180b-55b9-4cc6-95c4-c4ef3caff186.png)

#### 차원축소 기법 

##### 변수 선택 : variable / feature selection

* 원래의 변수 집단으로부터 유용할 것으로 판단되는 소수의 변수들을 선택 
* Filter : 변수 선택 과정과 모델 구축 과정이 독립적 
* Wrapper : 변수 선택 과정이 머신러닝 모델의 결과를 최적화 하는 방향으로 이루어짐

##### 변수 추출 : variable / feature extraction

* 원래의 변수 집단을 보다 효율적인 적은 수의 새로운 변수 집단으로 변환
* 머신러닝 모델에 독립적인 성능 지표가 추출된 변수의 효과를 측정하는 데 사용됨

##### 변수 선택 vs 변수 추출

![5](https://user-images.githubusercontent.com/86525868/213905892-77b7bed5-b579-4e56-bf8a-ad8f7a64262e.png)

### 변수 선택 기법 

#### 1. 전역 탐색 : Exhaustive Search

가능한 모든 경우의 조합에 대해 모델을 구축한 뒤 최적의 변수 조합을 찾는 방식 

  ↳ 예 : 3개의 변수가 존재하는 경우 

![6](https://user-images.githubusercontent.com/86525868/213905893-bc5dcf5e-d33b-45eb-9dac-4112577ef8a7.png)

변수 선택을 위한 모델 평가 기준 

* 회귀 및 분류에서 사용하는 성능 평가 지표를 주로 사용하며, 동일한 성능일 경우 적은 수의 변수를 사용하는 것을 선호하는 장치 추가 기능

  ↳ 예 : 선형회귀분석 - Akaike Information Criteria(AIC), Bayesian Information Criteria(BIC), 수정 R-제곱합, Mallow's Cp 등

$$
AIC=n\cdot \ln \Big(\frac{SSE}{n}\Big)+2k \\
Adjusted \ R^2=1-\Big(\frac{n-1}{n-k-1}\Big)(1-R^2)=1-\frac{n-1}{n-k-1}\cdot\frac{SSE}{SST}
$$

$AIC$는 낮을수록 좋은 지표 → 같은 오류값(Sum of Squared Error)이라면 변수의 수를 적게 쓰는 것을 선호 

**하지만, 현실적으로 전역 탐색이 융효한 변수 선택 기법이 아닌 이유** : 1초에 10,000개의 모델을 평가할 수 있는 컴퓨터를 활용할 경우 변수 선택에 소요되는 시간을 감당할 수 없음

![7](https://user-images.githubusercontent.com/86525868/213905894-8c154485-1f4b-400a-b542-736cbd510370.png)

#### 2. 전진 선택법 : Forward Selection 

설명변수가 하나도 없는 모델에서부터 시작하여 가장 유의미한 변수를 하나씩 추가해 나가는 방법

한번 선택된 변수는 제거되지 않음

![8](https://user-images.githubusercontent.com/86525868/213905895-2b5646f7-5ca2-48a9-8c17-938d715baa7c.png)

종료시점 : 모든 조합에 대해서 더이상 추가적인 변수를 투입해도 성능 향상이 일어나지 않을 때 

##### 예시 

![9](https://user-images.githubusercontent.com/86525868/213905896-faec261e-db8d-4f12-82f7-cb640874b20b.png)

$x_4$변수를 추가하고 난 뒤에 변수가 추가 됐을 때 성능 향상이 일어나지 않음 → $x_4$까지 변수 추가 

#### 3. 후진 소거법 : Back Elimination 

모든 변수를 사용하여 구축한 모델에서 유의미하지 않은 변수를 하나씩 제거해 나가는 방법

한번 제거된 변수를 다시 선택될 가능성이 없음

![10](https://user-images.githubusercontent.com/86525868/213905897-20ec172d-9a46-4be7-bd58-cf035966b7da.png)

##### 예시

![11](https://user-images.githubusercontent.com/86525868/213905898-fff3d8ac-e566-4d11-8e70-2f3b0f0b497c.png)

한 변수만 제거된 모델을 모두 평가하여 성능 저하가 발생하지 않는 변수 $x_3, x_8$ 제거 → 이후에는 어떠한 변수를 제거하더라도 유의미한 성능 감소 발생 

#### 4. 단계적 선택법 : Stepwise Selection 

앞에서 소개한 전진 선택법과 후진 소거법은 효율성을 극대화 시키기 위해 경우의 수를 많이 줄이기 때문에 조금 더 성능을 높이기 위한 방법 

전진 선택법과 후진 소거법을 번갈아 가면서 수행하는 변수 선택 기법 

한번 선택된 변수가 이후 과정에서 제거되거나, 제거된 변수가 이후 과정에서 재선택될 수 있음

선택된 변수의 총 수는 증가와 감소가 번갈아가며 일어날 수 있음 

![12](https://user-images.githubusercontent.com/86525868/213905900-ef63b812-8c19-4253-a817-9a870cf4c7be.png)

### 유전 알고리즘

**휴리스틱 기반의 변수 선택 기법들의 한계점** 

* 전역탐색 : 최적 변수 집합 선정을 보장하나 너무 오랜 시간이 걸림 

* 전진 선택 / 후진소거 / 단계적 선택 : 전역탐색에 비해서는 매우 효율적이나 최적 변수 집합을 찾을 가능성이 낮아짐

  ![13](https://user-images.githubusercontent.com/86525868/213905901-149fb000-46d1-4473-b65b-ed5a2f4a39ec.png)

**유전 알고리즘** 

기존 휴리스틱 기법들보다는 더 많은 시간을 사용해서 최적의 변수 집합을 찾을 가능성을 높임 

**메타 휴리스틱 기법** 

닫힌 해가 존재하지 않는 복잡한 문제에 대해서 시행착오를 줄이는 효율적 해 탐색 기법 (최적화 알고리즘 중에는 자연 시스템을 모방한 것들이 상당수 존재)

#### 유전 알고리즘 : 생명체(포유류)의 생식 과정을 모사한 진화 알고리즘의 종류(자연선택설 기반)

우수한 유전자(문제의 solution)는 생신을 통해 다음 세대에서도 잘 발현될 수 있도록 권장 

##### 유전 알고리즘의 세 가지 핵심 단계 

1. 선택(Selection) : 현재 기능 해집합에서 우수한 해들을 선택하여 다음 세대를 생성하기 위한 부모 세대로 지정
2. 교배(Crossover) : 선택된 부모 세대들의 유전자를 교환하여 새로운 세대를 생성
3. 돌연변이(Mutation) : 낮은 확률로 변이를 발생시켜 Local Optimum에서 탈출할 수 있는 기회 제공 

##### 변수 선택을 위한 유전 알고리즘 절차 

![14](https://user-images.githubusercontent.com/86525868/213905902-72b84225-4550-411b-8506-052c405039d4.png)

##### 1단계 : 초기화 

**염색체 인코딩 : Chromosome Encoding** 

유전 알고리즘은 변수선택 뿐만 아니라 다양한 최적화 문제에 적용 가능한 일반적인 기법 

염색체를 인코딩하는 방식은 task에 따라 다름

변수 선택에서는 이진 인코딩을 사용 

![15](https://user-images.githubusercontent.com/86525868/213905903-34e2e6eb-7261-4029-886e-8a45b3cc4fe1.png)

↳ 1일 경우 : 해당 변수 사용 / 0일 경우 : 해당 변수 미사용

**하이퍼파라미터 설정**

* Chromosome의 수 (Population size) : 한 세대에서 고려하는 변수 집합의 총 수, 크게 설정할수록 많은 범위를 탐색할 수 있지만, 그만큼 연산 자원이 많이 필요
* 적합도 함수 (Fitness function) : 현재의 변수 집합이 얼마나 우수한지를 평가할 수 있는 지표
* 교배 방식 (Crossover mechanism) : 부모 염색체 간 유전자를 교환하는데 적용되는 방식
* 돌연변이율 (Mutation rate) : 각 유전자마다 적용되는 돌연변이율
* 종료 조건 (Stopping criteria) : 적합도 함수가 일정 수준 이상 개선되지 않을 때, 최대 세대 수까지 진행되었을 때 $\cdots$

![18](https://user-images.githubusercontent.com/86525868/213905906-1c14c992-2110-4ae5-8ce4-9b2fa9e60330.png)

↳ best 결과와 평균결과 비교 → 세대가 거듭될수록 자연선택설에 의해서 우수한 것들만 남아 개선하다보면 평균결과와 best결과의 차이가 거의 없어짐 

**세대 초기화** 

염색체의 각 유전자마다 난수(ramdom number) 생성

생성된 난수와 기준값 (cut-off)을 비교하여 0/1의 이진 값으로 변환 

**예시 : population size = 8, cut-off=0.5**

![16](https://user-images.githubusercontent.com/86525868/213905904-1577e8ff-5422-43a0-890c-58c772977d7d.png)

##### 2단계 : 모델 학습

**각 염색체에 담긴 정보(해당 변수의 모델링 사용 유무)를 활용하여 염색체 수만큼 모델을 학습**

![17](https://user-images.githubusercontent.com/86525868/213905905-534f1f48-17fa-4efb-adac-33510bd47fef.png)

**각 염색체의 정보를 사용하여 학습된 모형의 적합도 평가**

적합도 함수 : 염색체의 유열을 가릴 수 있는 정량적 지표, 높은 값을 가질수록 우수한 염색체(우수한 변수 조합)

적합도 함수가 가져야하는 일반적인 조건

* 두 염색체가 동일한 여측 성능을 나타낼 경우, 적은 수의 변수를 사용한 염색체 선호 
* 두 염색체가 동일한 변수를 사용했을 경우, 우수한 예측 성능을 나타내는 염색체를 선호 

선형 회귀분석의 경우, 수정 $R^2$, AIC, BIC 등이 사용됨 

##### 3단계 : 적합도 평가 

**각 염색체의 정보를 사용하여 학습된 모형의 적합도 평가**

![19](https://user-images.githubusercontent.com/86525868/213905907-adebc2bd-536f-488d-9bb6-c1bd04ce81e0.png)

##### 4단계 : 부모 염색체 선택

현재의 세대에서 높은 적합도를 나타내는 염색체들을 부모로 선택하여 다음 세대의 염색체를 생성하는데 사용 

1. **확정적 선택, Determinisic selection** : 적합도 기준 상위 N%에 해당하는 염색체만 부모 염색체로 사용 가능 (하위 100-N%에 해당하는 염색체는 부모 염색체로 사용될 가능성 없음)
2. **확률적 선택, Probabilistic selection** : 적합도 함수에 비례하는 가중치를 사용하여 부모 염색체를 선택 (모든 염색체가 부모 염색체로 선택될 가능성이 있으나 적합도 함수가 낮을수록 선택 가능성 역시 낮아짐)

**예시**

**1. 확정적 선택 (N=50%)**

![20](https://user-images.githubusercontent.com/86525868/213905908-ea3267a5-ca42-440a-b2e3-4d1f94c5bf3c.png)

Adjusted $R^2$가 0.5를 넘는 염색체 (1, 2, 4, 8번염색체)만 다음 세대를 생성하는 부모 염색체의 역할을 수행

**2. 확률적 선택**

첫 번째 부모 염색체 쌍을 선택하기 위해 난수 생성  (**생성된 난수 : 0.881, 0.499**)

![21](https://user-images.githubusercontent.com/86525868/213905909-85d3ec39-29d2-4b74-ab4d-fde2bd841e29.png)

생성된 난수가 누적 적합도 값(Weight) 어디에 해당하는 지 확인 후, 부모 염색체 쌍으로 선택 

![22](https://user-images.githubusercontent.com/86525868/213905910-6e877931-4996-48d3-a2ba-1f1c7e9aa44e.png)

##### 5단계 : 교배 및 돌연변이

**1. 교배**

앞 단계에서 선택된 한 쌍의 부모 염색체들이 서로 유전자 정보를 일부 교환하여 새로운 자식 염색체들을 생성

교배 지점은 최소 1개부터 최대 전체 유전자 변수 수 까지 가능 

**예시 : 교배지점 1개**

![23](https://user-images.githubusercontent.com/86525868/213905911-21326186-71a5-47a3-92dd-d8bfe4d8f33b.png)

**예시 : 교배지점 10개**

![24](https://user-images.githubusercontent.com/86525868/213905913-2a329fe1-0052-4cf8-a90c-bfe8598be972.png)

↳ 난수 생성 후, cut-off 넘는 변수들을 교환하여 자식 염색체 생성 

**2. 돌연변이**

세대가 진화하는 과정에서 한쪽으로 쏠려 local optima에 빠지는 것을 방지 → 세대가 진화해가는 과정에서 다양성을 확보하기 위한 장치 

특정 유전자의 정보를 낮은 확률로 반대값으로 변환하는 과정을 통해 돌연변이 유도 

너무 높은 돌연변이율은 유전 알고리즘의 수렴속도를 늦춤 (주로 0.01 이하 값 사용)

![25](https://user-images.githubusercontent.com/86525868/213905914-feb866ac-147b-4122-9521-09bc2d3c8a40.png)

