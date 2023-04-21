---
title: "[DataScience] 1. 커널 기반 학습(Kernel Based Learning)"
categories:
  - ds
tags:
  - Data Analysis
  - Kernel Based Learning
toc: true
toc_sticky: true
toc_label: "On This Page"
toc_icon: "bookmark"
use_math: true
---

# 1.  Kernel Based Learning

💡 Data Science <br>
**강필성** 교수님의 강의를 보고 정리하였습니다.
{: .notice--info}

## 이론적 배경

### SHATTER 

함수 $F$는 n개의 point를 **shatter**할 수 있다 → 함수 $F$에 의해 n개의 point는 임의의 +1, -1을 Target value로 하는 분류 경계면의 생성(Binominal Classification)이 가능하다. 

<img width="519" alt="image-20230213153858657" src="https://user-images.githubusercontent.com/86525868/218806402-a7c1dcd4-fa0a-4d7b-a0cd-2e00ca81c7fd.png">{: .align-center}

  ↳ 예시에서는 점이 3개 이므로 생성 가능한 경계면은 8가지 ($2^3$)

선형분류기는 d차원에서 ($d+2$)개의 점을 shatter하는 것은 불가능

원점을 지나는 원형 분류기(원 안에 있으면 파란색, 원 밖에 있으면 빨간색으로 분류)는 2차원에서 2개의 점도 shatter 할 수 없지만

<img width="812" alt="image-20230213155609050" src="https://user-images.githubusercontent.com/86525868/219056426-3f3307c8-58e3-4f2e-84f7-a2939827a2ee.png">{: width="40%" height="40%"}{: .align-center}

원점을 지나는 조건을 삭제하면 원 형태의 분류 경계면은 2차원에서 3개의 점을 shatter할 수 있음 

<img width="625" alt="image-20230213155818115" src="https://user-images.githubusercontent.com/86525868/219056442-a7bd93e0-5619-450d-8c63-059e8e54b025.png">{: width="40%" height="40%"}{: .align-center}

### VC dimension 

- 어떤 함수의 Capacity(복잡한 경계면이나 회귀식을 추정하는 능력)를 측정하는 지표 
- 함수 $H$에 의해 최대로 shattered 될 수 있는 points의 수 = **VC dimension**
  - 위의 예시에서 선형 분류기의 VC dimension은 2차원에서 3
  - 원점을 중심으로하는 원 분류기의 VC dimension은 2차원에서 1
  - 원점 위치에 제약이 없는 원 분류기의 VC dimension은 2차원에서 3

### 구조적 위험 최소화 접근법, Structural Risk Minimization 

* 예측 모델의 정확도가 높다고해서 반드시 좋은 것만은 아님 
* 동일한 정확도라면 상대적으로 복잡도가 낮은 모델이 선호되어야 함 

<img width="493" alt="image-20230213160546309" src="https://user-images.githubusercontent.com/86525868/218806437-b3c4a35e-aa50-48ea-8d6f-28d69adf0b53.png">{: width="40%" height="40%"}{: .align-center}

$$
R_{emp}[f]=\frac{1}{n}\sum_{i=1}^nL(f(x_i), y)\\
R[f]\le R_{emp}[f]+\sqrt{\frac{f\big(\ln \frac{2n}{h}+1\big)-\ln\big(\frac{\delta}{4}\big)}{n}}
$$

* 인공신경망, 의사결정나무, 선형/로지스틱 회귀분석은 경험적 위험 $R_{emp}[f]$만을 최소화하는 것을 목적으로 함
  * 딥러닝의 가중치에 L2-norm 또는 L1-norm 형태의 페널티를 부여하는 것은 모델의 구조적 복잡도를 낮추기 위한 일환

![image-20230215012818340](https://user-images.githubusercontent.com/86525868/218806441-6322f533-0c18-4078-be06-1f80f0bdecea.png){: width="40%" height="40%"}{: .align-center}

* 구조적 위험 최소화 접근법에 따르면 내가 가진 데이터보다 최적화 시켜야 하는 파라미터 수가 많으면 에러율이 증가해야 하는데 Wide Resnet 모델은 복잡도가 증가해도 테스트 데이터에 대한 성능이 낮아지지 않음

    (모델의 복잡도를 제어하는 방식의 SVM / 내부적으로 모델의 복잡도를 키운 뒤 데이터 내부의 내제적인 관계를 학습히는 것이 딥러닝)

### Support Vector Machine 

서포트 백터 머신은 구조상 선형 분류기에 속함 (구조는 선형 분류기이지만 커널 트릿 기법을 통해 분류 경계면이 비선형으로 보임)

![image-20230215013224235](https://user-images.githubusercontent.com/86525868/218806449-91576868-0642-4bba-af35-529a27080264.png){: width="70%" height="70%"}{: .align-center}

해당하는 입력 공간에서 직선 식을 만들고 분류를 진행 → 데이터를 통해 상수항 $b$와 가중치 $w$를 최적화하고 가장 분류를 잘하는 분류 경계면 찾기 

### 이범주 분류 분제 

**학습 데이터** : $S=\Big((x_i, y_i), \cdots, (x_n, y_n)\Big)\in X \times \{-1, +1\}$ → 로지스틱에서는 $\{0, 1\}$ 로 승산을 의미하지만 SVM은 \{-1, +1\} 로 표시 

**문제** : 일반화 오류를 가장 낮출 수 있는 모델 $H$를 찾기 

### 좋은 경계면 

![image-20230215013812348](https://user-images.githubusercontent.com/86525868/218806455-ef12d118-2015-4281-8a62-56b5b20776e6.png){: width="40%" height="40%"}{: .align-center}

분류 경계면 A, B를 기준으로 위아래, 양옆으로 처음 데이터 포인트를 만나는 지점까지의 등간격 공간(Margin)을 생성 → **마진을 최대화 하는 분류 경계면이 좋은 분류 경계면**

![image-20230215013951163](https://user-images.githubusercontent.com/86525868/218806461-86793c3b-4bf6-439f-8aad-31123eee9ad5.png){: width="40%" height="40%"}{: .align-center}

$$
margin =\frac{1}{\lVert w\lVert^2}
$$

**마진 최대화가 구조적 위험 최소화로 어떻게 연결 되는가**

$$
h \le \min \Big(\Big[\frac{R^2}{\Delta^2}\Big], D\Big)+1\\ \\
\text{h : VC dimension, D : dimension}
$$

마진 최대화 → VC deminsion의 최소화 (복잡도 최소화) → Capacity 항 최소화 

$$
R[f]\le R_{emp}[f]+\sqrt{\frac{f\big(\ln \frac{2n}{h}+1\big)-\ln\big(\frac{\delta}{4}\big)}{n}}
$$

![image-20230215014848886](https://user-images.githubusercontent.com/86525868/218806465-cc4f7b24-73e1-43ba-aa76-f681c260a2bc.png){: width="40%" height="40%"}{: .align-center}

마진이 더 넓은 분류 경계면은 왼쪽 → 데이터를 분류할 수 있는 경계면의 수 감소 →  VC deminsion 감소 

### SVM의 종류 

![image-20230215015001590](https://user-images.githubusercontent.com/86525868/218806468-a4e713e0-a7c2-4c52-82a2-4c9d81ce0b01.png){: .align-center}

#### SVM Case 1 : Linear & Hard Margin 

선형 분류가 가능하면서 hard margin인 경우 

* **목적함수** 

  $$
  \min\frac{1}{2}\lVert\mathbf w\lVert^2
  $$
  
  $margin=\frac{2}{\lVert \mathbf w\lVert^2}$ 을 최대화 하는 것은 $margin$의 역수 $\frac{1}{2}\lVert \mathbf w\lVert^2$를 최소화 하는 것과 같음

* **제약식** 

  $$
  s.t. \quad y_i(\mathbf w^T\mathbf x_i +b)\ge1
  $$
  
  분류 경계면 등간격으로 마진 생성 → 마진 사이에는 실제 데이터가 없어야하고 특정한 범주에 데이터 포인트들은 자기 쪽 마진을 결정하는 경계면 보다 물러나 있어야 함 
  
![image-20230215013951163](https://user-images.githubusercontent.com/86525868/218806461-86793c3b-4bf6-439f-8aad-31123eee9ad5.png){: width="40%" height="40%"}{: .align-center}

* **Lagrangian Problem** 

  $$
  \min L_p(\mathbf w, b, \alpha_i)=\frac{1}{2}\lVert\mathbf w \lVert^2-\sum_{i=1}^N \alpha_i\big(y_i(\mathbf w^T \mathbf x_i+b)-1\big)\\s.t. \quad \alpha_i \ge0
  $$

* **KKT**

  $$
  \frac{\partial L_p}{\partial \mathbf w}=0 \Rightarrow \mathbf w=\sum_{i=1}^N \alpha_iy_i\mathbf x_i \quad \frac{\partial L_p}{\partial b}=0 \Rightarrow \sum_{i=1}^N \alpha_i y_i=0
  $$

* **From Primal to Dual**
  
  $$
  \min L_p(\mathbf w, b, \alpha_i)=\frac{1}{2}\lVert\mathbf w\lVert^2-\sum_{i=1}^N\alpha_i\big(y_i(\mathbf w_T\mathbf x_i+b)-1\big) \quad s.t. \ \alpha_i\ge0\\
  \max L_D(\alpha_i)=\sum_{i=1}^N\alpha_i-\frac{1}{2}\sum_{i=1}^N\sum_{j=1}^N\alpha_i\alpha_jy_iy_j\mathbf x_i\mathbf x_j \quad s.t\ \sum_{i=1}^N\alpha_iy_i=0 \ \text{and}\ \alpha_i \ge0
  $$
  
  $\mathbf w, b$에 대한 primal 식의 최소화 문제가 $\alpha$에 대한 최대화 문제로 바뀌면서 최적해를 갖는 $\alpha$ 찾기

* **Fimal Decision**
  
  $$
  f(\mathbf x_{new})=sign\Big(\sum_{i=1}^N\alpha_iy_i\mathbf x_i^T\mathbf x_{new}+b\Big)
  $$

* **From KKT condition**
  
  $$
  \alpha_i(y_i(\mathbf w^T\mathbf x_i+b)-1)=0
  $$
  
  최적해가 되려면 Lagrangian Multi ($\alpha_i$)나 제약식($y_i(\mathbf w^T\mathbf x_i+b)-1$) 둘 중에 하나만 0이 되어야 하고 

  $y_i(\mathbf w^T\mathbf x_i+b)-1\ge0$ 에서 

    * $\alpha_i=0$ 일 경우는 점이 마진 밖에 있다는 것을 의미 → $\mathbf w$를 결정하는데 영향을 미치지 못함 
    * $\alpha_i \gt 0$인 경우에는 $y_i(\mathbf w^T\mathbf x_i+b)=1$는 마진 위에 존재하는 것을 의미 → 마진을 만들어내는(마진을 지지하는) 점들을 나타냄 

  $$
  \mathbf w = \sum_{i=1}^N \alpha_i y_i \mathbf x_i=\sum_{i\in SV}^N\alpha_i y_i\mathbf x_i
  $$

![image-20230216001658223](https://user-images.githubusercontent.com/86525868/219071098-57677663-0748-4807-81be-0dcc635bf5dd.png){: width="40%" height="40%"}{: .align-center}

#### 정리 

1. 왜 SVM의 목적함수가 $\frac{1}{2}\lVert\mathbf w\lVert^2$인가? → **margin을 최대화하기 위해서**
2. 각각의 제약식이 의미하는 것은? → **점들이 margin 위, margin 밖 공간에 위치하도록 정의하기 위해서**
3. primal 문제에서 미지수를 1차 미분해서 최적화 조건을 만들고 그 최적화 조건을 통해 듀얼 문제를 만드는 이유는? → **오목한 형태의 Multiplier와 $\alpha$에 대한 이차함수의 형태로 항상 최적해가 만들어지기 때문에** 

### SVM Case 2 : Linear & Soft Marging 

데이터 상에 노이즈가 존재 → 해당하는 마진을 벗어나는 객체들에 대해 어쩔 수 없음을 이해하고 페널티를 부여하며 컨트롤 

* **목적함수**

  $$
  \min \frac{1}{2}\lVert\mathbf w\lVert^2+C\sum_{i=1}^N\xi_i
  $$
  
  **마진**($\frac{1}{2}\lVert\mathbf w\lVert^2$)을 **최대화**하지만 **마진 외부의 점에 대한 페널티**($C\sum_{i=1}^N\xi_i$)는 **최소화** 

* **제약식**

  $$
  s.t. \quad y_i(\mathbf w^T\mathbf x_i+b)\ge1-\xi_i, \xi_j\ge0, \forall i
  $$
  
  ![12](https://user-images.githubusercontent.com/86525868/221480473-7cdbf05b-dc6e-45df-8458-438919a3a8b1.png){: width="40%" height="40%"}{: .align-center}

* **Lagrangian Problem**

  $$
  \min L_p(\mathbf w, b, \alpha_i)=\frac{1}{2}\lVert\mathbf w \lVert^2+C\sum_{i=1}^N \xi_i-\sum_{i=1}^N\alpha_i\big(y_i(\mathbf w^T\mathbf x_i+b)-1+\xi_i\big)-\sum_{i=1}^N\mu_i\xi_i\\
  s.t\quad \alpha_i\ge0,\ \mu_i\ge0
  $$

* **KKT Conditions**

  $$
  \frac{\partial L_p}{\partial \mathbf w}=0 \Rightarrow \mathbf w=\sum_{i=1}^N \alpha_iy_i\mathbf x_i \quad \frac{\partial L_p}{\partial b}=0 \Rightarrow \sum_{i=1}^N \alpha_i y_i=0\\
  \frac{\partial L_p}{\partial \xi_i} \Rightarrow C-\alpha_i-\mu_i=0
  $$

* **From Primal to Dual**

  $$
  \min L_p(\mathbf w, b, \alpha_i)=\frac{1}{2}\lVert\mathbf w\lVert^2+C\sum_{i=1}^N\xi_i-\sum_{i=1}^N\alpha_i\big(y_i(\mathbf w_T\mathbf x_i+b)-1+\xi_i\big)-\sum_{i=1}^N\mu_i\xi_i \quad \\ 
  s.t. \ \alpha_i\ge0\\
  \max L_D(\alpha_i)=\sum_{i=1}^N\alpha_i-\frac{1}{2}\sum_{i=1}^N\sum_{j=1}^N\alpha_i\alpha_jy_iy_j\mathbf x_i^T\mathbf x_j \quad \\ 
  s.t\ \sum_{i=1}^N\alpha_iy_i=0 \ \text{and}\ 0\le\alpha_i \le C
  $$

* **서포트 백터란?** : 실제 모델의 분류 경계면을 찾는데 필요한 관측치 → 마진 위 또는 바깥에 존재하는 점들로 구성 

* **최적해 조건**

  $$
  \alpha_i\big(y_i(\mathbf w_T\mathbf x_i+b)-1+\xi_i\big)=0
  $$
  
  서포트 백터에 대해서만 $\alpha_i \ne 0$이 성립 & $C-\alpha_i-\mu_i$ & $\mu_i\xi_i=0$ 

  * Case 1 : $\alpha_i =0$ → 서포트 벡터가 아닌 객체 $\mu_i=C, \xi_i=0$
  * Case 2 : $0 \lt\alpha_i\lt C$ → 마진 위에 위치하는 서포트 백터 $\mu_i\gt0, \xi_i=0, y_i(\mathbf w^T\mathbf x_i+b)-1=0$
  * Case 3 : $\alpha_i=C$ → 마진 밖에 존재하는 서포트 벡터 $\mu_i=0, \xi_i \gt0$

  ![14](https://user-images.githubusercontent.com/86525868/221480494-c558e3db-6820-4e90-ac97-518efc9445a0.png){: .align-center}

* **오분류 비용 C**

  $$
  \min \frac{1}{2}\lVert\mathbf w\lVert^2+C\sum_{i=1}^N\xi_i
  $$

  * **Large C** : 페널티를 줄이는 방향으로 학습이 되므로 마진 폭이 좁고 $\alpha_i=C$ 인 서포트 벡터의 수가 상대적으로 적음
  * **Small C** : 페널티를 받더라도 마진을 넓게 잡도록 학습이 되므로 $\alpha_i=C$ 인 서포트 벡터의 수가 상대적으로 많음

  ![15](https://user-images.githubusercontent.com/86525868/221480497-4420b744-974c-4523-a4c6-adfb40c14191.png){: width="60%" height="60%"}{: .align-center}

#### 선형 모델의 한계 

분류 경계면이 비선형일 경우 이를 잘 찾아내지 못함 → **원래 공간이 아닌 선형 분류가 가능한 더 고차원의 공간으로 데이터를 보내서 모델을 학습**

$$
\Phi : (\mathbf x_1, \mathbf x_2) \rightarrow(\mathbf x_1^2, \mathbf x_2^2, \sqrt{2}\mathbf x_1, \sqrt{2}\mathbf x_2, \sqrt{2}\mathbf x_1\mathbf x_2, 1)
$$

![16](https://user-images.githubusercontent.com/86525868/221480498-876091ca-d9ee-42fd-8700-e05e8a0503f9.png){: .align-center}

$\Phi$ 라는 매핑 함수를 통해 $\mathbf x_1, \mathbf x_2$ 2차원 데이터를 6차원 데이터로 차원 확장 → **저차원 공간에서는 선형 분류가 불가능했지만 고차원으로 차원을 확장하면 선형분류가 가능해짐**

![17](https://user-images.githubusercontent.com/86525868/221480500-912fb34d-7fd1-42cd-ad23-ed9b965f2ce9.png){: .align-center}

**목적** : 마진을 최대화하고 일반화 성능확보, 유연성 확보하기 위해 고차원의 매핑을 통해 비선형 분류 경계면 생성 

### SVM Case 3 : Nonlinear & Soft Margin 

* **목적 함수** 

  $$
  \min \frac{1}{2}\lVert\mathbf w\lVert^2+C\sum_{i=1}^N \xi_i
  $$

* **제약식**

  $$
  s.t. \quad y_i(\mathbf w^T\Phi_i(\mathbf x_i)+b)\ge1-\xi_i, \xi_j\ge0, \forall i
  $$

* **Lagrangian Problem**

  $$
  \min L_p(\mathbf w, b, \alpha_i)=\frac{1}{2}\lVert\mathbf w\lVert^2+C\sum_{i=1}^N\xi_i-\sum_{i=1}^N\alpha_i\big(y_i(\mathbf w_T\Phi(\mathbf x_i)+b)-1+\xi_i\big)-\sum_{i=1}^N\mu_i\xi_i \quad
  $$

* **KKT Condition**

  $$
  \frac{\partial L_p}{\partial \mathbf w}=0 \Rightarrow \mathbf w=\sum_{i=1}^N \alpha_iy_i\Phi(\mathbf x_i) \quad \frac{\partial L_p}{\partial b}=0 \Rightarrow \sum_{i=1}^N \alpha_i y_i=0\\
  \frac{\partial L_p}{\partial \xi_i} \Rightarrow C-\alpha_i-\mu_i=0
  $$

* **Lagrangian Problem Dual**

  $$
  \max L_D(\alpha_i)=\sum_{i=1}^N\alpha_i-\frac{1}{2}\sum_{i=1}^N\sum_{j=1}^N\alpha_i\alpha_jy_iy_j\Phi(\mathbf x_i^T)\Phi(\mathbf x_j) \quad \\ 
  s.t\ \sum_{i=1}^N\alpha_iy_i=0 \ \text{and}\ 0\le\alpha_i \le C
  $$

#### 커널 트릭 아이디어의 시작점 

$$
\max L_D(\alpha_i)=\sum_{i=1}^N\alpha_i-\frac{1}{2}\sum_{i=1}^N\sum_{j=1}^N\alpha_i\alpha_jy_iy_j\Phi(\mathbf x_i^T)\Phi(\mathbf x_j) \quad
$$

$\mathbf x$를 $\Phi(\mathbf x)$로 변환하는 매핑 함수 $\Phi$를 잘 찾아야 함 

만약, 두 데이터의 고차원 공간상에서의 내적을 계산하는(저차원 데이터로 내적을 계산하는) 함수가 있다면 명시적으로 $\Phi$라는 매핑 함수를 사용하지 말고 커널함수 $K$를 사용하여 값을 계산하자 ! 

#### 대표적인 커널 함수 

* **Polynomial**
 
  $$
  K(x, y)=(x\cdot y+c)^d, \ c\gt0
  $$

* **Gaussian (RBF)**

  $$
  K(x, y)=\exp\Big(-\frac{\lVert x-y\lVert^2}{2\sigma^2}\Big),\ \sigma \ne0
  $$

* **Sigmoid**

  $$
  K(x, y)=\tanh(\alpha(x\cdot y)+b), \ a, b \ge0
  $$

##### Guassian (RBF) 커널

이론적으로 무한개의 점을 Shatter 할 수 있음 

If $K(\mathbf x, \mathbf x')$ is an inner product in some space $\mathcal Z$, we are good. 

$$
K(\mathbf x, \mathbf x')=\exp\Big(-\gamma\lVert\mathbf x-\mathbf x'\lVert^2\Big)\\
K(x, x')=\exp(-(x-x')^2)=\exp(-x^2)\exp(-x'^2)\sum_{k=0}^\infty\frac{2^k(x)^k(x')^k}{k!}
$$

$\sum\frac{2^k(x)^k(x')^k}{k!}$은 테일러 전개로 무한개로 늘릴 수 있음 → 무한 차원으로 확장 가능하지만 VC dimension이 무한이라는 뜻은 아님(마진이 정해지면 마진의 크기에 따라 VC dimension이 제약됨)

$$
K(x, y)=\exp \Big(-\frac{\lVert x_i-x_j\lVert^2}{2\sigma^2}\Big)
$$

* 위의 식에서 $\sigma$가 작다는 것은 두 객체 사이의 거리가 조금만 떨어져 있어도 차이를 0으로 만들 수 있음 → **객체 데이터의 위치 변화에 민감하게 반응**
* 많은 소프트웨어에서는 $-\frac{1}{\sigma^2}$을 사용하기 까다롭기 때문에 $\gamma$를 사용 

![21](https://user-images.githubusercontent.com/86525868/221480507-b4977a7e-f285-4f33-982a-19bf6f552258.png){: .align-center}

* $\gamma$ 가 작다 → $\sigma$가 크다 → 변화에 둔감하다, **마진이 큰 분류 경계면**

* $\gamma$ 가 크다 → $\sigma$ 가 작다 → 마진에 민감하게 반응, **$\sigma$가 작을 때는 분류경계면이 주어진 데이터 포인트 하나하나 감싸는 것 처럼 민감하게 반응**

![22](https://user-images.githubusercontent.com/86525868/221480508-a022e866-1fed-47b6-a24f-48ecb08106c3.png){: .align-center}

#### 커널 형태에 따른 분류 경계면

![18](https://user-images.githubusercontent.com/86525868/221480501-28beb28b-11f4-4769-84d8-48e88472bbbb.png){: width="60%" height="60%"}{: .align-center}

거듭제곱이 올라갈수록 차원도 늘어나기 때문에 조금 더 유연한 분류를 할 가능성이 올라감 

#### 오분류 페널티에 따른 분류 경계면 (Linear Kernel)

![19](https://user-images.githubusercontent.com/86525868/221480502-55c622cd-07ca-4ece-a6d0-ae655359a737.png){: width="40%" height="40%"}{: .align-center}

#### 커널 파라미터와 오분류 페널티에 따른 분류 경계면 (RBF Kernel)

![20](https://user-images.githubusercontent.com/86525868/221480504-9e801847-f417-4031-adda-05d2c0e1f324.png){: width="40%" height="40%"}{: .align-center}

#### 새로운 데이터 적용 

$$
f(\mathbf x)=\text{sign}\Big(\sum_{i=1}^N\alpha_i y_i K(\mathbf x_i, \mathbf x)+b\Big) 
$$

새로운 데이터가 들어왔을 때 해당하는 서포트 벡터들과의 커널 함수 값을 계산해서 $\alpha$와 $\mathbf x$값을 모두 곱하고 $b$를 더해서 그 값의 부호가 이보다 크면 positive 범주, 부호가 이보다 작으면 negative 범주 
