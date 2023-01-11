---
title: "[Data Analysis] 4. Decision Tree"
categories:
  - Data Analysis
  - Decision Tree
tags:
  - Data Analysis
  - Decision Tree
toc: true
toc_sticky: true
toc_label: "On This Page"
toc_icon: "bookmark"
use_math: true
---

# Chapter 4. 의사결정나무

**여러 가지 머신러닝 알고리즘이 필요한 이유?** → 특정 알고리즘이 모든 상황에서 다른 알고리즘보다 우월하다는 결론을 내릴 수 없음 

![1](https://user-images.githubusercontent.com/86525868/211734327-606770cc-e303-42d5-9dc7-27bf8472fb68.png){: width="70%" height="70%"}{: .align-center}


## 목적

* **한번에 하나씩의 설명변수**를 사용하여 정확한 예측(+설명)이 가능한 규칙들의 집합을 생성

* 최종 결과물은 나무를 뒤집어 높은 형태인 규칙들의 집합 

  예시 : 만일 내일 날씨가 맑고 습도가 70% 이하이면 아이는 밖에 나가서 놀 것이다. or 만일 내일 비가 오고 바람이 불면 아이가 밖에 나가 놀지 않을 것이다.

![2](https://user-images.githubusercontent.com/86525868/211734350-1c61f305-ee83-4ad4-9b58-366db1b9f9bf.png){: width="50%" height="50%"}{: .align-center}


## 용어 

* 노드 : 입력 데이터 공간의 특정 영역
* 부모 노드 : 분기 전 노드
* 자식 노드 : 부모 노드로부터 분기 후 파생된 노드 
* 분기 기준 : 한 부모 노드를 두 개 이상의 자식 노드들로 분기하는데 사용되는 변수 및 기준 값
* 시작 / 뿌리 노드 : 전체 데이터를 포함하는 노드, 자식 노드만 존재하며 부모 노드가 존재하지 않음
* 말단 / 잎새 노드 : 더 이상 분기가 수행되지 않는 노드, 부모 노드만 존재하며 자식 노드가 존재하지 않음 



## Decision Tree

### 의사결정나무를 사용하는 이유 

* 결과를 사람이 이해할 수 있는 규칙의 형태로 제공
* 데이터의 사전 전처리를 최소화 함 (정규화 / 결측치 처리 등을 하지 않아도 됨)
* 수치형 변수와 범주형 변수를 함께 다룰 수 있음



### Key Ideas

1. **재귀적 분기 : Recursive Partitioning** → 입력변수의 영역을 두 개로 구분 (구분하기 전보다 구분된 뒤에 각 영억의 순도가 증가하도록)
2. **가지치기 : Pruning the Tree** → 과적합을 방지하기 위하여 너무 자세하게 구분된 영역을 통합 



### Classification and Regresstion Tree : CART

* 개별 변수의 영역을 반복적으로 분할함으로써 전체 영역에서의 규칙을 생성하는 지도학습 기법

* if-then 형식으로 표현되는 규칙을 생성함으로써, 결과에 대한 예측과 함께 그 이유를 설명할 수 있는 장점이 있음

* 수치형 변수와 범주형 변수에 대한 동시 처리 가능 



#### 재귀적 분기 : Recursive Partitioning 

* 특정 영역 (부모 노드)에 속하는 개체들을 **하나의 기준 변수 값**의 범위에 따라 분기 
* 분기에 의해 새로 생성된 자식 노드의 **동질성이 최소화** 되도록 분기점 선택 
* 불순도를 측정하는 기준으로는 범주형 변수에 대해서는 지니계수, 수치형 변수에 대해서는 분산을 이용 

![3](https://user-images.githubusercontent.com/86525868/211734360-4d1efa6d-76da-4cfe-b19e-1db1434d51e5.png){: width="50%" height="50%"}{: .align-center}


#### 가지치기 : Pruning 

재귀적 분기 후에 과적합을 방지하기 위하여 하위 노드들을 상위 노드로 결합하는 것 

* Pre-pruning : Tree를 생성하는 과정에서 최소 분기 기준을 이용하는 사전적 가지치기 
* Post-pruning : Full-Tree 생성 후, 검증 데이터의 오분류율과 Tree의 복잡도(말단 노드의 수) 등을 고려하는 사후적 가지치기 



## 예시 

### CART 재귀적 분기 

![4](https://user-images.githubusercontent.com/86525868/211734365-9313f1d4-01f9-41d5-b642-a9d154c39a9b.png){: .align-center}


목적 : 24개의 가정에 대해 잔디깍기 기계 구입 여부를 분류 

설명변수 : 수입(income), 집의 크기(lot size)



#### **전체 영역을 어떻게 분할해야 하는가** 

분할 후 각 노드에 한 범주에 속하는 객체들이 다수 존재하도록 분할 하는 것이 좋음 → **이러한 개념을 하나의 숫자로 어떻게 정량화할 것인가**

![5](https://user-images.githubusercontent.com/86525868/211734371-c83bd5aa-148e-4032-94b3-6077b2d46ed5.png){: .align-center}


##### 분순도 지표1 : 지니 계수(Gini Index)

**c개의 범주가 존재하는 A영역에 대한 지니계수** 

$$
I(A)=1-\sum_{k=1}^c P^2_k
$$

- $p_k$ = A영역에 속한 객체들 중 k 범주에 속하는 레코드의 비율 

  ![6](https://user-images.githubusercontent.com/86525868/211734376-be6ce1a5-d6d9-4953-b07e-1e6b28b74484.png){: width="70%" height="70%"}{: .align-center}


  → $0.4688$은 초록색 박스 안에 데이터가 혼재되어 있는 정도 

  * 두 범주만 존재하는 상황에서 영역 A의 모든 객체들이 동일한 범주에 속할 경우 : $I(A)=0$

  * 두 범주만 존재하는 상황에서 영역 A에 속하는 객체들이 1/2의 비중으로 존재할 경우 : $I(A)=0.5$

    → 즉, 지니 계수는 $0 \le I(A) \le 0.5$ 사이의 값을 갖고 0일때 순도가 높고 0.5에 가까울 수 록 불순도가 높다고 표현 

    

**두 개 이상의 영역에 대한 지니 계수**

$$
I(A)=\sum_{i=1}^d \Big(R_i(1-\sum_{k=1}^cp^2_{ik}\Big)
$$

* $R_i$ = 분할 전 레코드 중 분할 후 $i$영역에 속하는 레코드의 비율 

  ![7](https://user-images.githubusercontent.com/86525868/211734382-2bb002ba-ca3f-4fbf-8282-56436ef6e3e9.png){: width="90%" height="90%"}{: .align-center}


→ **분기 후의 정보 획득**(빨간색 점선으로 얻을 수 있는 효과) : $0.4688-0.3438=0,1250$ 



##### 불순도 지표 2 : Deviance 

**특정 영역에 대한 Deviance**

$$
D_i=-2\sum_kn_{ik}\log(p_{ik})
$$

![8](https://user-images.githubusercontent.com/86525868/211734390-230e9311-97ac-4b01-ad13-a9aaf9981312.png){: width="70%" height="70%"}{: .align-center}


![9](https://user-images.githubusercontent.com/86525868/211734396-be4a7a7a-9f9c-47a0-a13b-6a3ef564772f.png){: width="70%" height="70%"}{: .align-center}


→ 분기 후의 정보 획득 = $21.17-16.62=4.55$



#### - 한 변수(Lot Size)를 기준으로 하여 정렬 

![10](https://user-images.githubusercontent.com/86525868/211734402-0aa795e2-a94b-42b6-90df-eb0e12d52b79.png){: width="70%" height="70%"}{: .align-center}


#### - 순차적으로 가능한 분기점에 대한 정보 획득 계산 

![11](https://user-images.githubusercontent.com/86525868/211734442-348159e5-3047-41bf-bc1b-64edd975b51d.png){: .align-center}

#### - 다른 한 변수(Income)를 기준으로 정렬 후 순차적으로 가능한 분기점에 대한 정보 획득 계산 

![12](https://user-images.githubusercontent.com/86525868/211734448-390607be-53dc-46e3-8bfb-586cb75904f1.png){: .align-center}

#### - 최적의 분기점

![13](https://user-images.githubusercontent.com/86525868/211734458-c8650c15-c527-4c6b-b1c2-40f9d9a1ac04.png){: .align-center}

#### - 의사결정나무 구조 

* 분기점은 나무의 노드가 되며 원 안의 숫자는 분기 기준이 되는 변수의 값

* 사각형은 말단노드 (Leaf node, 더 이상 분기가 수행되지 않는 노드)

* 선 아래 숫자는 분기로 인해 나뉜 레코드의 수를 의미 

  ![14](https://user-images.githubusercontent.com/86525868/211734464-22be8cb0-7e3e-4e59-bab0-1fc5357f56be.png){: .align-center}

#### - 모든 노드의 순도가 100%가 될 때까지 분기를 수행 

![15](https://user-images.githubusercontent.com/86525868/211734468-11b259e6-0265-4017-b924-c907bd85b8d3.png){: .align-center}

#### - 재귀적 분기 완료 (Full Tree)

모든 말단 노드에는 한 범주의 객체들만 존재 → 순도 100%, Training error = 0

![16](https://user-images.githubusercontent.com/86525868/211734477-17426c8b-b835-46fb-852c-508706acba92.png){: width="50%" height="50%"}{: .align-center}

#### - 의사결정나무를 통한 분류 예측 

해당 객체가 속하는 말단 노드에 속한 학습 객체들의 비율을 통해 판정 (일반적으로 0.5를 분류 기준값, cut-off로 사용)

![17](https://user-images.githubusercontent.com/86525868/211734483-8aeb3994-1366-47d2-809e-c37f8771bec7.png){: width="50%" height="50%"}{: .align-center}

#### - 과적합 문제 

재귀적 분기는 모든 말단 노드의 순도가 100%일 때 종료됨

![18](https://user-images.githubusercontent.com/86525868/211734489-ff406e06-edbb-4cc8-95e8-a0f7fe756fdc.png){: width="50%" height="50%"}{: .align-center}

* depth가 일정 깊이까지는 깊어질 수록 노이즈에 영향을 받지 않는 일반적인 패턴 $f(x)$ 를 찾게 됨 → **데스트 데이터도 에러 감소**
* depth가 일정 깊이 이상으로 깊어지면 노이즈를 패턴으로 인식 → **테스트 데이터 에러는 증가**

일로 이러한 Full Tree는 과적합 문제를 내포하고 있으며, 새로운 데이터에 대한 예측 성능 저하 (일반화 성능 감소)의 위험을 갖고 있음 

의사결정나무의 노드 수가 증가할 때, 처음에는 새로운 데이터에 대한 오분류율이 감소하지만 일정 수준 이상이 되면 오분류율이 증가하는 현상 발생 

![20](https://user-images.githubusercontent.com/86525868/211734500-65b388db-6e41-462f-8944-225415d1e5b1.png){: .align-center}

#### - 사후적 가지치기 (Post-Pruning)

의사결정나무는 Full Tree를 생성한 뒤 적절한 수준에서 말단 노드를 결합하는 가지치기 수행 (Post-Pruning)

검증데이터에 대한 오분류율이 증가하는 시점에서 가지치기 → Full Tree에 비해 구조가 단순한 의사결정나무가 생성

**비용 복잡도**를 사용하여 최적의 의사결정나무 구조 선택 



##### 비용 복잡도 (Cost Complexity)

$$
CC(T)=Err(T)+\alpha \times L(T)
$$

* $CC(T)$ : 의사결정나무의 비용 복잡도 (낮을수록 우수한 의사결정나무)
* $ERR(T)$ : 검증 데이터에 대한 오분류율
* $L(T)$ : 말단 노드의 수 (구조 복잡도)
* $\alpha$ : $ERR(T)$와 $L(T)$를 결합하는 가중치 (사용자에 의해 부여, 일반적인 소프트웨어패키지에서는 default값이 존재)

비용 복잡도는 같은 수의 말단 노드를 갖는 Tree라면 검증데이터셋 오류율이 낮은 Tree를 선택하거나 동일한 검증 데이터셋 오류율이라면 말단 노드의 수가 적은 Tree를 선택



##### 가지치기 전 vs 후 

![19](https://user-images.githubusercontent.com/86525868/211734494-921d8b83-03d1-4813-bebe-19205fce3941.png){: .align-center}

오분류된 데이터가 발생하더라도 구조를 **단순화**하는 것이 **"일반화"**에 도움이 됨 



#### - 사전적 가지치기 (Post-Pruning)

![21](https://user-images.githubusercontent.com/86525868/211734505-ce122588-8039-4edc-818a-258bcfccb4b7.png){: .align-center}

**예시**

* 분기 전후 Information Gain의 최저 기준
* 분기 대상이 되는 노드에 속하는 최소 객체 수 
* 의사결정나무가 가질 수 있는 최대 깊이 등 



##### Universal Bank의 개인신용대출 예측

고객의 인구통계학적 정보와 은행 이용 행태를 바탕으로 개인신용대출을 이용할 고객 판별 

![22](https://user-images.githubusercontent.com/86525868/211734510-ef225049-fae0-4d31-86d5-68908421cf30.png){: width="50%" height="50%"}{: .align-center}

![23](https://user-images.githubusercontent.com/86525868/211734518-20de09c6-2f5a-48d4-93a5-f3963502baeb.png){: .align-center}

말단 노드를 통해 똑같이 대출을 이용했더라도 어떠한 기준, 이유인지를 알면 마케팅 포인트로 활용할 수 있음 

or 

**규칙 생성** : 각 말단에 도달하는 규칙을 생성하고 각 말단 노드에 해당하는 규칙을 시스템에 반영 → 새로운 데이터가 들어올 때, 적절한 액션을 취할 수 있음 



## 회귀의사결정나무 

### 말단 노드의 예측값 

전체 영역을 Split Point를 기준으로 영역을 나누고 각 구간의 존재하는 $y$값들의 평균으로 예측 

![24](https://user-images.githubusercontent.com/86525868/211734524-6e05ead0-b8c3-4a3b-a8e9-09ecca1a8583.png){: width="50%" height="50%"}{: .align-center}

↳ split point = 5.5 → if $y\lt5.5, y=4$, if $y\gt5.5, y=8$

### 회귀 모형에서의 불순도

Split하기 전에 혼재되어 있는 정도와 split 한 후에 양쪽의 혼재되어 있는 정도를 각각 계산 → 혼재의 감소가 클수록 좋은 SPLIT POINT !

**불순도, Impurity 측정** → 얼마나 중심으로부터 퍼져있는가 **분산의 개념(오차제곱합)**

![25](https://user-images.githubusercontent.com/86525868/211734528-11e44768-3dcd-43b8-9a17-04cdd77b5ee5.png){: .align-center}

$$
SSE(Parent)=(3.8-6.0)^2+(4.2-6.0)^2+\cdots+(7.6-6.0)^2=40.5\\
SSE(Left\ Child)=(3.8-4.0)^2+(4.2-4.0)^2+\cdots+(3.9-4.0)^2=0.1\\
SSE(Right\ Child)=(8.2-8.0)^2+(7.8-8.0)^2+\cdots+(7.6-8.0)^2=0.4\\
Information \ Gain=40.5-(0.1+0.4)=40.0
$$

### 토요타 코롤라 중고차 가격 예측 문제 

![26](https://user-images.githubusercontent.com/86525868/211734535-36061d58-dc6e-4f95-8479-9aa9aebec33f.png){: width="70%" height="70%"}{: .align-center}

* 5번째 말단 노드 : $52.5 \le$ 사용기간 $\le 58.5$ and 마력 $\le 93.5$ 일 경우, 10.468 Euro에 팔림

회귀의사나무는 실질적으로 연속형의 숫자를 예측하긴 하지만, 실수에 대한 예측값은 회귀나무의 말단노드의 개수만큼만 만들어짐 



## 의사결정나무의 장단점

**장점**

1. 의사결정나무는 예측에 대한 설명을 제공할 수 있음
2. 변수 선택 과정이 자동적으로 수행됨
3. 특별한 통계적 가정을 요구하지 않음
4. 결측치가 존재하는 상황에서도 모델 구축이 가능함

**단점**

1. 한 번에 하나의 변수만 고려하므로 변수간 상호작용을 파악하기 어려움 
