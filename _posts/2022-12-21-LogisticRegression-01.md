---
title: "[Data Analysis] 2. Logistic Regression"
categories:
  - Data Analysis
  - Logisitc Regression
tags:
  - Data Analysis
  - Logisitc Regression
toc: true
toc_sticky: true
toc_label: "On This Page"
toc_icon: "bookmark"
use_math: true
---

# 로지스틱 회귀분석 : Logisitc Linear Regression



**다중 선형 회귀분석** : 수치형 설명변수 $X$와 종속변수 $Y$ 간의 관계를 선형으로 가정하고 이를 잘 표현할 수 있는 회귀계수를 추정

<img width="457" alt="01" src="https://user-images.githubusercontent.com/86525868/208841857-59a2a2c7-4937-4fd9-859b-0f4bbf4035fa.png">{: width="40%" height="40%"}{: .align-center}

  $y$ 와 $\hat{y}$ 의 차이가 적게 되는 $\hat{\beta_i}=(X^T X)^{-1}X^TY$ 를 추정



### 예시

1. 33명의 성인 여성에 대한 나이와 혈압 사이의 관계

  <img width="1323" alt="02" src="https://user-images.githubusercontent.com/86525868/208841867-d61b649e-6e7e-4599-8e7a-6830188dc266.png">{: width="70%" height="70%"}{: .align-center}

  해석 : p-value가 충분히 작다는 가정하에 혈압은 81.54를 기준으로 나이가 들어갈수록 1.222씩 늘어난다. 



2. 연속형 변수가 아닌 이진형 Binary 변수인 Cancer Diagnosis일 경우 

  <img width="1356" alt="03" src="https://user-images.githubusercontent.com/86525868/208841878-360bd924-3a25-4a0c-8e97-25ac5c9efac6.png">{: width="70%" height="70%"}{: .align-center}

  해석 : 현재 데이터에 선형회귀를 적용할 수 는 있지만 제대로된 추정이 아님 → **이진분류에 사용할 수 있는 로지스틱 사용**



### 0 or 1의 이진 값이 아닌 확률값을 종속 변수로 사용한다면?

선형회귀분석의 우변의 범위에 대한 제한이 없기 때문에 종속변수(좌변) 역시 범위의 제한을 받지 않으므로 적절하지 않음

<img width="448" alt="04" src="https://user-images.githubusercontent.com/86525868/208841885-b82bd1d9-e735-4511-bbab-8a6f3598d040.png">{: width="40%" height="40%"}{: .align-center}

$y\in \{0,1\}$ 의 범위와 $\hat{y}\in\{-\infty, \infty\}$ 의 범위는 적절하지 않음 

  → $y$를 확률로 바꾸어 $0\le p(y=1)\le 1$ ($y$가 1이 될 확률), $p(y=1) \in [0,1]$로 바꾸어 생각 

  → 하지만 선형 회귀선이 계속해서 늘어나면 확률이 갖지 못하는 값 (1보다 크고 0보다 작은 값)이 될 수 있기 때문에 이것도 적절하지 않음

  → 해결 방법 : **Logistic Regression**



## 로지스틱 회귀분석 : Logistic Regression 

**목적** : 이진형 (0 or 1)의 형태를 갖는 종속변수(분류문제)에 대해 회귀식의 형태로 모형을 추정하는 것 

**속성** 

1. 종속변수 $Y$ 자체를 그대로 사용하는 것이 아니라 $Y$에 대한 logit function를 회귀식의 종속변수로 사용 → 좌변의 범위를 $[-\infty, \infty]$ 로 맞추기위해 
2. logit function는 설명변수의 선형결합으로 표현될 수 있음 
3. logit function의 값은 종속변수에 대한 성공 확률로 역산될 수 있으며, 이는 따라서 분류 문제에 적용할 수 있음



### ODDS 

$$
Odds=\frac{p}{1-p} \quad\quad
\text{(p:성공범주, class=1에 속할 확률)}
$$

2010 World Cup Betting Odds → 스페인 9:2, 한국 250:1

* 스페인의 우승 odds는 $\frac{2}{9}$이므로 스페인의 우승 확률은 $\frac{2}{11}$
* 대한민국의 우승 odds는 $\frac{1}{250}$이므로 대한민국의 우승확률은 $\frac{1}{251}\fallingdotseq 0.00398 (0.398\%)$


확률값이 0부터 1로 변화함에 따라 승산 odds는 0부터 무한대의 값을 가짐 $[0, \infty]$

* $p$ 가 0에 가까우면 $\frac{p}{1-p}$ 는 0에 가까운 값을 가짐
* $p$ 가 1에 가까우면 $\frac{p}{1-p}$ 는 무한대에 가까운 값을 가짐

<img width="538" alt="05" src="https://user-images.githubusercontent.com/86525868/208841887-699c20c9-70c2-474a-b635-4cc41c0a9abe.png">{: width="40%" height="40%"}{: .align-center}

**하지만 여전히 범위에 대한 제약이 존재함 $0<odds<\infty$** → 비대칭성 Asymmetric 

→ 해결방법 : **Odds에 $\log$를 취함**

$$
\log(Odds)=\log\Big(\frac{p}{1-p}\Big)
$$

  ↳ 성공확률 $p$ 가 작으면 음수값을 갖고, 성공확률 $p$가 크면 양수값을 가짐

<img width="538" alt="06" src="https://user-images.githubusercontent.com/86525868/208841888-a937464a-1c42-4c51-a530-c8447ea9f24d.png">{: width="40%" height="40%"}{: .align-center}

### 로지스틱 회귀분석의 식 

1. Log Odds를 이용한 회귀분석식

   $$
   \log (Odds)=\log\Big(\frac{p}{1-p}\Big)=\hat{\beta_0}+\hat{\beta_1}x_1+\hat{\beta_2}x_2 +\cdots+\hat{\beta_d}x_d
   $$

   회귀식의 형태를 유지해서 얻을 수 있는 장점 : 변수의 통계적 유의성 확인 가능 / 변수의 증감과 성공확률의 관계를 해석하고 이해할 수 있음

2. 양변에 지수를 취하면 

   $$
   \frac{p}{1-p}=e^{\hat{\beta_0}+\hat{\beta_1}x_1+\hat{\beta_2}x_2 +\cdots+\hat{\beta_d}x_d}
   $$

3. 성공확률에 대한 식으로 표현 → 시그모이드 함수

   $$
   p=\frac{1}{1+e^{-(\hat{\beta_0}+\hat{\beta_1}x_1+\hat{\beta_2}x_2 +\cdots+\hat{\beta_d}x_d)}}=\sigma(x|\beta)
   $$

   $\sigma(x\|\beta)$ : $\beta$ 라는 추정값을 전제하고 $x$ 라는 새로운 값이 주어졌을 때, 나타나는 성공확률

$$
\ln\Big(\frac{p}{1-p}\Big) : \text{logit}\\
\ln\Big(\frac{p}{1-p}\Big)=\beta_0+\beta_1x_1 +\cdots+\beta_ix_i
$$

* 로지스틱 회귀 모형은 종속변수가 이분형일 때 선형회귀 모형의 제약을 극복하기 위해 확률에 대한 로짓 변환을 고려하여 분석 

$$
p=\frac{e^{\hat{\beta_0}+\hat{\beta_1}x_1+\cdots+\hat{\beta_i}x_i}}{1+e^{\hat{\beta_0}+\hat{\beta_1}x_1+\hat{\beta_2}x_2 +\cdots+\hat{\beta_d}x_d}}
$$

* 위의 모형식에서 추정된 회귀계수로부터 사후확률에 대한 추정식을 계산 

## 로지스틱 회귀분석 : 학습

동일한 데이터셋에 대해 다음과 같이 두 가지 로지스틱 회귀분석 모형이 존재한다고 하면 어떤 모형이 현재 데이터를 더 잘 설명하는 모형인가?

<img width="543" alt="07" src="https://user-images.githubusercontent.com/86525868/208841892-c6711895-4e02-495d-9b22-57c4ccce7636.png">{: width="70%" height="70%"}{: .align-center}

  ↳ cut-off가 0.5라면 분류 정확도는 둘다 100% 이지만, 실제 정답이 1일 때 1 범주로 예측할 확률이 높고 정답이 0일 때도  0 범주로 예측할 확률이 높으므로 Model A가 더 우수한 모형

### 로지스틱 회귀분석에서 회귀 계수의 추정 

동일한 데이터셋에 대해 위와 같이 둑 자의 로지스틱 회귀분석 모형이 존재한다고 하면 어떤 모형이 현재 데이터를 잘 설명하는 모형인가?

→ **우도함수, Likelihood function**

1. 개별 객체의 우도 함수는 해당 학습 데이터가 정답 범주에 속할 확률 (고객1의 우도 함수 값은 0.908, 고객2의 우도함수 값은 0.799)
2. 데이터의 생성 과정이 독립임을 가정할 수 있을 때, 전체 데이터셋의 우도 함수는 개별 객체의 우도 함수를 모두 곱한 값

   $P(A,B)=P(A)\cdot P(B)$ → 사건 $A$와 $B$가 독립일 때 

3. 일반적으로 데이터셋의 우도 함수는 매우 작은 값을 가지므로(1보다 작은 소수가 계속 곱해지므로) 로그 우도 함수를 주로 사용


### 우도함수 

<img width="744" alt="08" src="https://user-images.githubusercontent.com/86525868/208841894-e2f9676c-d3e4-4f0e-aed7-124ae7e9033f.png">{: width="70%" height="70%"}{: .align-center}

Model A의 (로그)우도 함수가 Model B의 (로그)우도 함수보다 큼 → Model A 가 Model B보다 데이터셋을 더 잘 설명하는 모델 

### 최대 우도 추정법, Maximum likelihood estimation (MLE)

학습 데이터의 개별 객체들이 갖는 정답 범주에 대한 확률을 극대화하기   

1. 성공에 대한 식에서 

   $$
   p=\frac{1}{1+e^{-(\hat{\beta_0}+\hat{\beta_1}x_1+\hat{\beta_2}x_2 +\cdots+\hat{\beta_d}x_d)}}=\sigma(\mathbf{x}|\beta)
   $$

2. $i$번째 객체에 대한 우도 함수 

   $$
   P(\mathbf{x}_i, y_i|\beta)=
   \begin{cases}
   \sigma(\mathbf{x}_i|\beta), & \mbox{if }y_i=1\\
   1-\sigma(\mathbf{x}_i|\beta), & \mbox{if }y_i=0
   \end{cases}
   $$

3. 출력변수가 1과 0임을 고려하여 다음과 같이 변형가능 

   $$
   P(\mathbf{x}_i, y_i|\beta)=\sigma(\mathbf{x}_i|\beta)^{y_i}(1-\sigma(\mathbf{x}_i|\beta))^{1-y_i}
   $$

4. 학습 데이터셋의 객체들이 도립적으로 발생됨을 가정할 경우 전체 데이터 셋에 대한 우도 함수는 다음과 같이 표현 

   $$
   L(\mathbf{X}, \mathbf{y}|\beta)=\prod_{i=1}^N \sigma(\mathbf{x}|\beta)^{y_i}(1-\sigma(\mathbf{x}_i|\beta))^{1-y_i}
   $$

5. 양변에 로그를 취하면 

   $$
   \log L(\mathbf{X}, \mathbf{y}|\beta)=\sum_{i=1}^N \Big(y_i \log(\sigma(\mathbf{x}|\beta))+(1-y_i)\log(1-\sigma(\mathbf{x}_i|\beta))\Big)
   $$

6. 우도함수와 로그-우도함수는 회귀계수 $\beta$에 대해 비선형이므로 선형회귀분석과 같이 명시적인 해가 존재하지 않음 → **Conjugate gradient** 등의 최적화 알고리즘을 차용해 해를 구함 

### 기울기 하강, Gradient descent algorithm

<img width="534" alt="09" src="https://user-images.githubusercontent.com/86525868/208841897-208da8e9-7876-471e-9682-ce70e02405f7.png">{: width="70%" height="70%"}{: .align-center}

파란색 선 : 미지수 w의 변화에 따른 목적함수 값의 변화 

검정색 점 : 현재 해의 위치 

화살표 : 목적함수를 최적화하기 위해 미지수 w가 이동해야 하는 방향 

gradient : 목적함수를 미지수에 대해 1차 미분한 값 → 최저점 Global cost minimum $J_{min}(w)=0$ 

![1_rCCH2J1JHTdknga6qFpwyA](https://user-images.githubusercontent.com/86525868/208846393-0205efcf-3646-4221-a5e3-76ff367bd20b.gif){: width="70%" height="70%"}{: .align-center}

**비용함수를 현재의 가중치 값(w)에 대해 1차 미분을 수행한 뒤 아래의 절차를 따름**

1. 1차 미분 값 (gradient)이 0인가?

   : 그렇다 → 현재의 가중치 값이 최적이므로 학습 종류

   : 아니다 → 현재의 가중치 값이 최적이 아니므로 계속 학습 

2. 1차 미분 값(gradient)가 0이 아닐 경우 어떻게 해야 좀 더 잘하는 모델을 만들 수 있는 가?

   : 1차 미분 값 (gradient)의 부호에 대한 반대 방향으로 조금씩 가중치를 이동 

**식 전개**

1. 함수의 테일러 전개 

  $$
   f(w, \Delta w)=f(w)+\frac{f'(w)}{1!}\Delta w +\frac{f''(w)}{2!}(\Delta w)^2 + \cdots
   $$

2. 목적함수가 최소인 경우 함수의 1차 미분 값(gradient)이 0이 아니면 Gradient의 반대 방향으로 이동해야 목적함수 값을 감소시킬 수 있음

  $$
   w_{new}=w_{old}{\color{red}{-}}{\color{blue}{\alpha}} f'({\color{red}{w}}), \quad \text{where } 0<\alpha<1
   $$
   
   $\color{blue}\alpha$는 얼마만큼 갈 것인가를 나타내고 $\color{red}-, w$는 어느방향으로 갈 것인가 

3. 이동 후의 새로운 함수 값은 이동 전의 함수 값보다 작음

   $$
   f(w_{new})=f(w_{old}-\alpha f'(w_{old})) \cong f(w_{old})-\alpha |f'(w)|^2<f(w_{old})
   $$

### 로지스틱 회귀분석의 그래프 표현 

설명 변수의 값들을 적절히 취합해서 시그모이드 Sigmoid 함수를 통해 출력값을 산출 

<img width="523" alt="10" src="https://user-images.githubusercontent.com/86525868/208841899-d4c3900b-ccf4-4979-8df7-37bfefbaa0db.png">{: width="70%" height="70%"}{: .align-center}

<img width="688" alt="11" src="https://user-images.githubusercontent.com/86525868/208841901-6ffd7824-73d2-4d26-84a9-cf78ee8b12cf.png">{: width="70%" height="70%"}{: .align-center}

### Gradient Descent 에서 회귀계수가 업데이트 되는 원리 

<img width="620" alt="12" src="https://user-images.githubusercontent.com/86525868/208841905-8f7069c9-cb60-4530-99b2-58616d4906d6.png">{: width="70%" height="70%"}{: .align-center}

현재 모델이 얼마나 잘 맞추는지, 해당 회귀계수와 연결되어 있는 설명변수가 얼마나 값이 크냐 작냐에 따라 값이 업데이트 됨 



## 로지스틱 회귀분석 : 성공확률

회귀계수가 추정되고 나면 주어진 설명변수 집합에 대한 성공확률을 다음과 같이 계산할 수 있음

$$
P(y=1)=\frac{1}{1+e^{-(\hat{\beta_0}+\hat{\beta_1}x_1+\hat{\beta_2}x_2 +\cdots+\hat{\beta_d}x_d)}}
$$


<img width="371" alt="13" src="https://user-images.githubusercontent.com/86525868/208841907-a2331cc9-060a-4d39-8701-a6b7bea281bf.png">{: width="70%" height="70%"}{: .align-center}

### 이진분류를 위한 cut-off 설정 

<img width="528" alt="14" src="https://user-images.githubusercontent.com/86525868/208841911-67ddd35f-68a3-408d-93cd-03fd46bfcf5b.png">{: width="40%" height="40%"}{: .align-center}

일반적으로 0.5가 주로 사용되고 사전확률을 고려한 cut-off나 검증 데이터의 정확도를 최대화하는 cut-off 등이 사용될 수도 있음

### 로지스틱 회귀분석 회귀계수의 의미 

1. 선형 회귀분석 회귀식 

  $$
   \hat{y}=\hat{\beta_0}+\hat{\beta_1}x_1+\hat{\beta_2}x_2 +\cdots+\hat{\beta_d}x_d
   $$

2. 선형 회귀분석에서의 회귀계수는 해당 변수가 1 증가함에 따른 종속변수의 변화량

   로지스틱 회귀분석 회귀식

  $$
   \log (Odds)=\log\Big(\frac{p}{1-p}\Big)=\hat{\beta_0}+\hat{\beta_1}x_1+\hat{\beta_2}x_2 +\cdots+\hat{\beta_d}x_d\\
   p=\frac{1}{1+e^{-(\hat{\beta_0}+\hat{\beta_1}x_1+\hat{\beta_2}x_2 +\cdots+\hat{\beta_d}x_d)}}
   $$

3. 로지스틱 회귀분석에서의 회귀계수는 해당 변수가 1 증가함에 따른 **로그 승산의 변화량**

### 승산 비율 : Odds Ratio

로지스틱 회귀분석에서 나머지 벼수는 모두 고정시킨 상태에서 한 변수를 1만큼 증가시켰을 때 변화하는 Odds의 비율 

**Odds Ratio**

$$
\frac{odds({\color{red}{x_1+1}}, \cdots, x_d)}{odds(x_1, \cdots, x_d)}=
\frac{e^{\hat{\beta_o}+\hat{\beta_1}({\color{red}{x_1+1}})+\hat{\beta_2}x_2+\cdots+\hat{\beta_d}x_d}}{e^{\hat{\beta_o}+\hat{\beta_1}x_1+\hat{\beta_2}x_2+\cdots+\hat{\beta_d}x_d}}=e^{\hat{\beta_1}}
$$

$x_1$이 1 증가하게 되면 성공에 대한 승산 비율이 $e^{\beta_1}$만큼 변화함 (직관적인 확인이 어려움)

회귀 계수가 양수 → 변수가 증가하면 성공확률이 증가 = p값이 커짐 (성공 범주와 양의 상관 관계)

회귀 계수가 음수 →  변수가 증가하면 성공확률이 감소 (성공 범주와 음의 상관 관계)

$$
\frac{p}{1-p}=e^{\hat{\beta_o}+\hat{\beta_1}x_1+\hat{\beta_2}x_2+\cdots+\hat{\beta_d}x_d}
$$

### 로지스틱 회귀분석 결과 및 해석 

$$
p=\frac{1}{1+e^{-(\hat{\beta_0}+\hat{\beta_1}x_1+\hat{\beta_2}x_2 +\cdots+\hat{\beta_d}x_d)}}
$$

<img width="504" alt="15" src="https://user-images.githubusercontent.com/86525868/208841913-96eef6f3-7683-4bff-ad4f-44a7f9227218.png">{: width="70%" height="70%"}{: .align-center}

1. 회귀계수, Coefficient 

  로지스틱 회귀분석에서 각 변수에 대응하는 베타값
  
  선형회귀분석에서는 해당 변수가 1단위 증가할 때 종속변수의 변화량을 의미하지만, 로지스틱 회귀분석에서는 해당 변수가 1단위 증가할 때 로그승산비의 변화량을 의미
  
  양수이면 성공확률과 양의 상관관계, 음수이면 성공 확률과 음의 상관관계
  
  ✎ `age`는 p-value값이 높기 때문에 나이라는 속성은 대출유무를 판단하는데 유효한 속성이 아님 
  
  ✎ `family` 는 p-value값이 낮기 때문에 가족 구성원 수는 대출유무에 영향을 미치고 회귀 계수가 양수이기 때문에 가족 구성원 수가 많을 수록 대출을 이용할 확률이 높아짐 

2. 유의 확률, P-value

  로지스틱 회귀분석에서 해당 변수가 통계적으로 유의미한지 알려주는 지표 
  
  0에 가까울수록 모델링에 중요한 변수이며, 1에 가까울수록 유의미하지 않은 변수임 
  
  특정 유의수준($\alpha$)을 설정하여 해당 값 미만의 변수만을 사용하여 다시 로지스틱 회귀분석을 구축하는 것도 가능함 (주로 $\alpha=0.0.5$ 사용)
  
  ✎ `age, experience`의 p-value는 0.05보다 큰 값으로 나이와 직장경력은 신용대출 유무를 판별하는데 유의미하지 않지만 나머지 변수들은 유의미하다

3. 승산비율, Odds Ratio

  나머지 변수는 모두 고정시킨 상태에서 한 변수를 1만큼 증가시켰을 때 변화하는 Odds의 비율 

### Geometric interpretation 

로지스틱 회귀분석은 d차원의 데이터를 구분하는 (d-1)차원의 초평면을 찾는 것으로 이해할 수 있음 

<img width="520" alt="16" src="https://user-images.githubusercontent.com/86525868/208841916-69195b77-578a-4082-9eca-b24ef31ed9a8.png">{: width="70%" height="70%"}{: .align-center}

### 신용카드 연체 예측

<img width="483" alt="17" src="https://user-images.githubusercontent.com/86525868/208841920-7b396538-ec60-41b2-8f6a-c25c0fa6febb.png">{: width="70%" height="70%"}{: .align-center}

대출 금액(Balance)에 따른 연체 여부를 예측하는 것은 가능해 보이나, 수입(Income)에 따른 연체 여부를 예측하는 것은 불가능해 보임 → `Balance`는 유효한 변수라는 것을 알 수 있음 

**단변량 로지스틱 회귀분석**

<img width="558" alt="18" src="https://user-images.githubusercontent.com/86525868/208841924-3bad0eee-1c21-426f-adec-9a009f4b385b.png">{: width="70%" height="70%"}{: .align-center}

`balance` 변수를 가지고 단변량 로지스틱 회귀분석을 시행하면 

1. p-value가 낮기 때문에 연체를 예측하는데 중요한 변수가 될 것 
2. coefficient가 0보다 크기 때문에 잔액이 클수록 연체할 확률이 높아질 것 
3. 대출액이 \$1000일 때의 연체 확률은 0.6%이지만 대출액이 \$2000인 경우의 연체 확률은 58.6%로 높아진 것을 볼 수 있음

**다변량 로지스틱 회귀분석**

<img width="543" alt="19" src="https://user-images.githubusercontent.com/86525868/208841928-472df43f-8632-40b1-a6df-1ff40ac25d50.png">{: width="70%" height="70%"}{: .align-center}

1. `student[Yes]`일 경우에 coefficient가 음수라는 것은 학생이 아닐 경우 보다 연체를 일으킬 확률이 낮아진다는 것 
2. `income`의 p-value는 0.05보다 큰 값이므로 유의미한 변수가 아님



## 다항 로지스틱 회귀분석

지금까지의 로지스틱 회귀분석은 이범주 분류를 풀기 위한 방식 → 범주가 3개 이상은 다범주 분류에는 로지스틱 회귀분석을 어떻게 적용할 수 있을까?

**기준이 되는 범주를 설정하고 이 범주 대비 다른 범주가 발생할 로그 승산을 회귀식으로 추정**

<img width="360" alt="20" src="https://user-images.githubusercontent.com/86525868/208841930-3a65328b-d8ff-46a0-b199-3f289168599a.png">{: width="70%" height="70%"}{: .align-center}

### 예시

범주가 3개인 분류 문제의 경우 아래 두 개의 회구식에 대한 회귀 계수를 추정 

1. 범주 3 대비 범주 1의 발생 확률에 대한 로지스틱 회귀분석 

  $$
  \log\Big(\frac{p(y=1)}{p(y=3)}\Big)=\hat{\beta_{10}}+\hat{\beta_{11}}x_1+\hat{\beta_{12}}x_2 +\cdots+\hat{\beta_{1d}}x_d=\beta^T_{1\cdot}\mathbf{x}
  $$

2. 범주 3 대비 범주 2의 발생 확률에 대한 로지스틱 회귀분석
   
  $$
  \log\Big(\frac{p(y=2)}{p(y=3)}\Big)=\hat{\beta_{20}}+\hat{\beta_{21}}x_1+\hat{\beta_{22}}x_2 +\cdots+\hat{\beta_{2d}}x_d=\beta^T_{2\cdot}\mathbf{x}
  $$

**왜 범주는 3개인데 2개의 모형만 학습하는가? (일반화하면 K개의 범주가 있을 때, (K-1)개의 모형만 학습하는 이유는?)** 

각 범주에 속할 확률의 합은 항상 1이므로 나머지 K번째 범주에 대한 확률은 자동으로 산출 됨 

<img width="566" alt="21" src="https://user-images.githubusercontent.com/86525868/208841932-5f60ff66-c3d4-4143-bcb0-7bc54612ba5f.png">{: width="70%" height="70%"}{: .align-center}

### 와인의 화학적 성분 비율을 통해 포도 품종 맞추기 

<img width="778" alt="22" src="https://user-images.githubusercontent.com/86525868/208841934-808190d8-ba92-47e9-baf1-e5e566bf5113.png">{: width="70%" height="70%"}{: .align-center}

1. 12번째 변수는 3가지의 포도 품종이 비슷한 분포를 띄고 있기 때문에 유의미한 변수가 아닐 것 
2. 8번째 변수는 초록색 포도품종을 구분하는데 도움이 되지만 노랑색, 파란색의 포도품종을 구분하는데는 도움이 되지 않을 것 

<img width="655" alt="23" src="https://user-images.githubusercontent.com/86525868/208841937-8d6fb572-b14e-4b3d-bcd6-bc8e7e225548.png">{: width="70%" height="70%"}{: .align-center}

1. 5번째 그래프에서 PCA0, PCA1 첫 두개의 주성분만 사용해도 어느정도 잘 구분되는 것을 볼 수 있음

<img width="483" alt="24" src="https://user-images.githubusercontent.com/86525868/208841942-a76c28b8-ae79-47ae-a018-8a5382df8ae7.png">{: width="70%" height="70%"}{: .align-center}

1. `Total.phenols, Flavanolds` 는 3가지 품종을 나누는데 중요한 역할을 할 것 

2. `Ash, Proanthocyanins`는 1번과 3번 범주를 구분하는데는 유의미하지 않지만, 2번과 3번 범주를 구분하는데는 유의미할 것 

   
