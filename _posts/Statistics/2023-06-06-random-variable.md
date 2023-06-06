---
title: "[Statistics] 확률변수"
categories:
  - st
tags:
  - time series
  - statistics
toc: true
toc_sticky: true
toc_label: "On This Page"
toc_icon: "bookmark"
use_math: true
---

# 통계 정리 

## 확률변수

우리는 관찰할 수 있는 공간 안에서 어떠한 현상의 관찰 결과를 얻기 위해 **시행(실험)**을 진행하고 이때의 실험 결과를 수치적으로 표현한 것을 **확률변수(random variable)**라고 한다. 

확률변수는 미리 결정된 값이 아니지만, 관찰될 수 잇는 값들을 미리 생각해 볼 수 있다. 확률변수의 가능성을 표현한 것을 확률분포라고 한다. 

확률변수의 분포 형태를 나타낼 때는 **확률함수(probability function)**를 사용하는데, 확률변수가 이산형이라면 확률질량함수로 표현하고 연속형이라면 확률밀도함수라고 표현한다. 

여러 가지 확률함수들은 확률변수의 전체적인 성격을 설명하는데, 때로는 몇 개의 수치로 확률분포의 성질을 요약하고자 할 때도 있다. 

확률분포의 성질을 용약하는 수치들 중 **기댓값(expectation)**은 변수의 위치를 나타내는 중요한 모수로 평균(mean)이라고도 불린다. 

$$
E(X) = 
\begin{cases}
\sum_{all x_i}x_if(x_i) &\mbox{X가 이산형인 경우}\\
\int_{-\infty}^{\infty}xf(x)dx &\mbox{X가 연속형인 경우}
\end{cases}
$$

**분산(variance)**는 확률변수 $X$의 변동을 나타내는 지표로 평균 $E(X)$로부터 얼마나 흩어져있는지(밀집되어 있는지)를 나타낸다. 확률변수들의 평균 $E(X)$로부터 멀리 떨어져 있는 경향이 많을수록 분산은 증가한다. 분산의 제곱근을 **표준편차(standard deviation)**이라고 한다. 

$$
Var(X)=E[X-E(X)]^2\\
\sigma_X=\sqrt{Var(X)}
$$

분산의 정의를 보면 확률변수의 $X$의 단위의 제곱인 것을 볼 수 있다. 만약, $X$의 단위가 cm라면 $var(X)$의 단위는 $\text{cm}^2$이 되기 때문에 실제 문제에 있어서 변수 $X$의 흩어짐을 재는데 어려움이 있다. 이럴 때 분산의 제곱근의 표준편차를 통해 단위를 통일 시키고 변수의 흩어짐을 측정한다. 

표준편차와 기댓값을 통해 평균이 0이고 분산이 1인 $z$로 표준화 할 수 있다. 

$$
z=\frac{X-E(X)}{\sigma_x}
$$

분산이 하나의 확률변수가 흩어진 정도를 의미했다면, **공분산(covariance)**은 두 확률변수 $X$와 $Y$ 사이의 흩어진 관계를 측정하는 측도로 이해할 수 있다. (공분산은 $X$와 $Y$가 선형적으로 함께 움직이는 정도를 표현한 측도로 이해 가능)

$$
\begin{matrix}
Cov(X, Y) &=& E[(X-E[X])(Y-E[Y])]\\
&=& E(XY)-E(X)E(Y)
\end{matrix}
$$

### 이산형 확률분포 

#### 1. 베르누이 분포 

> 베르누이 분포는 [확률론](https://ko.wikipedia.org/wiki/확률론)과 [통계학](https://ko.wikipedia.org/wiki/통계학)에서 매 시행마다 오직 두 가지의 가능한 결과만 일어난다고 할 때, 이러한 실험을 1회 시행하여 일어난 두 가지 결과에 의해 그 값이 각각 0과 1로 결정되는 확률변수 X에 대해서
> $$
> P(X=0)=q, \quad P(X=1)=p, \quad 0\le p\le 0, \quad q=1-p
> $$
> 
> 를 만족하는 확률변수 X가 따르는 [확률분포](https://ko.wikipedia.org/wiki/확률분포)를 의미하며, [이항 분포](https://ko.wikipedia.org/wiki/이항_분포)의 특수한 사례에 속한다.

성공과 실패 딱 두가지의 결과만이 가능한 실험에서 결과가 성공이면 1의 값을 가지고 실패면 0의 값을 가지는 확률변수 $X$를 베르누의 확률변수라고 하고 그 분포를 베르누이 분포라고 한다. 

#### 2. 이항분포 

> 이항 분포는 연속된 *n*번의 독립적 시행에서 각 시행이 확률 p를 가질 때의 [이산 확률 분포](https://ko.wikipedia.org/wiki/이산_확률_분포)이다. 이러한 시행은 베르누이 시행이라고 불리기도 한다. 사실, *n*=1일 때 이항 분포는 [베르누이 분포](https://ko.wikipedia.org/wiki/베르누이_분포)이다.

![image](https://github.com/seoyun2/seoyun2.github.io/assets/86525868/afbcb534-c7a6-49b1-b9f9-eccadaf9108e){: width="50%" height="50%"}{: .align-center}

$$
\begin{matrix}
E(X) &=& np\\
Var(X) &=& np(1-p)\\
\sigma^2 &=& \sum_{k=1}^n \sigma^2=np(1-p)
\end{matrix}
$$


#### 3. 포아송 분포 

> **푸아송 분포**(Poisson distribution)는 [확률론](https://ko.wikipedia.org/wiki/확률론)에서 단위 시간 안에 어떤 사건이 몇 번 발생할 것인지를 표현하는 [이산 확률 분포](https://ko.wikipedia.org/wiki/이산_확률_분포)이다.

하지만 실생활에선 이항분포의 형태를 띄지만 정말 많이 관찰해야 하나의 사건을 발견하는 상황들이 있다. 어떤 고속도로 구간에서 일어나는 대형사고처럼 지나다니는 차는 많지만, 사고가 날 확률은 적은 경우 우리가 알고 있는 이항분포의 공식을 사용하면 확률을 구하기가 어렵다. 그렇기 때문에 확률분포를 근사적으로 계산할 수 있는 새로운 형태의 분포가 필요했고 포아송 분포가 생겨나게되었다. 

정해진 시간 안에 어떤 사건이 일어날 횟수에 대한 기댓값을 $\lambda$라고 했을 때, 그 사건이 $n$회 일어날 확률은 다음과 같다. 

![image](https://github.com/seoyun2/seoyun2.github.io/assets/86525868/7e6b66f2-982d-476d-9635-459ccde6ee6b){: width="50%" height="50%"}{: .align-center}

$$
f(n;\lambda)=\frac{\lambda^ne^{−\lambda}}{n!}
$$

#### 4. 기하분포 

> 성공확률 p인 [베르누이 시행](https://ko.wikipedia.org/wiki/베르누이_시행)에 대해, *k*번 시행 후 첫 번째 성공을 얻을 확률에 대한 분포이다. 

기하분포는 성공 혹은 실패의 두 가지 경우의 수로 구성된 시행을 연달아 수행 시 처음 성공할 때까지 시도한 횟수 $k$에 대한 분포이다. 확률값 $P(X=k)$는 매 항 계속 곱해나가는 수열의 형태를 가지고 있는 등비(기하)수열이 된다. 

![image](https://github.com/seoyun2/seoyun2.github.io/assets/86525868/fabb5d48-a0e0-4202-affb-2f09c57fae88){: width="50%" height="50%"}{: .align-center}


$$
Pr(X=k)=(1-p)^{k-1}p, \quad (k=1, 2, 3, \cdots)
$$


### 연속형 확률분포 

#### 1. 지수분포 

> 지수분포(exponential distribution)는 [연속 확률 분포](https://ko.wikipedia.org/wiki/연속_확률_분포)의 일종이다. 사건이 서로 독립적일 때, 일정 시간 동안 발생하는 사건의 횟수가 [푸아송 분포](https://ko.wikipedia.org/wiki/푸아송_분포)를 따른다면, 다음 사건이 일어날 때까지 대기 시간은 지수분포를 따른다. 이는 [기하분포](https://ko.wikipedia.org/wiki/기하분포)와 유사한 측면이 있다.

지수분포는 포아송 분포와 관련이 있다. 포아송분포는 단위 시간동안 어떤 사건이 평균적으로 $\lambda$회 발생한다고 했을 때, 단위시간동안 사건이 $k$번 일어날 확률에 대한 분포를 뜻한다. 처음 이런 사건이 일어날 때까지 걸리는 시간에 대한 분포를 지수분포로 나타낼 수 있다. 

![image](https://github.com/seoyun2/seoyun2.github.io/assets/86525868/230ecf3c-3498-4e8f-bd4d-588f5d2b0fdd){: width="50%" height="50%"}{: .align-center}


$$
f(x;\lambda)=
\begin{cases}
\lambda e^{-\lambda x} & x \ge 0\\
0 & x \lt0
\end{cases}
$$



#### 2. 정규분포 

> 정규분포는 수집된 자료의 분포를 [근사](https://ko.wikipedia.org/wiki/근사)하는 데에 자주 사용되며, 이것은 [중심극한정리](https://ko.wikipedia.org/wiki/중심극한정리)에 의하여 독립적인 [확률변수](https://ko.wikipedia.org/wiki/확률변수)들의 평균은 정규분포에 가까워지는 성질이 있기 때문이다. 정규분포는 2개의 매개 변수 [평균](https://ko.wikipedia.org/wiki/평균_(통계학)) $\mu$과 [표준편차](https://ko.wikipedia.org/wiki/표준편차) $\sigma$에 대해 모양이 결정되고, 이때의 분포를 $N(\mu, \sigma^2)$로 표기한다. 특히, 평균이 0이고 표준편차가 1인 정규분포 $N(0,1)$을 **[표준 정규 분포](https://ko.wikipedia.org/wiki/표준_정규_분포)**(standard normal distribution)라고 한다.


확률변수 X의 확률밀도함수가

$$
f(x)=\frac{1}{\sqrt{2\pi}\sigma}\exp[-(x-\mu)^2/2\sigma^2]\quad(-\infty\lt x\lt\infty)
$$

로 주어지면 X가 **정규분포(normal distribution)**을 따른다고 하고 $X\sim N(\mu, \sigma^2)$로 표기하기로 한다. 정규확률변수는 선형변환을 해도 정규분포를 따른다. 

![image](https://github.com/seoyun2/seoyun2.github.io/assets/86525868/90eafab5-f601-4d24-9c25-6fbf4c6c7393){: width="50%" height="50%"}{: .align-center}

--

$Reference$

> 수리통계학(송성주, 전명식-2015), 위키피디아 