---
title: "[Data Analysis] 6. K-Nearest Neighbor Learning"
categories:
  - Data Analysis
  - K-Nearest Neighbor Learning
tags:
  - Data Analysis
  - K-Nearest Neighbor Learning
  - K-Nearest Neighbor Regression
  - K-Nearest Neighbor Classification
toc: true
toc_sticky: true
toc_label: "On This Page"
toc_icon: "bookmark"
use_math: true
---

# Chapter 6. k-인접 이웃 기법 

💡 Data business <br>
**강필성** 교수님의 강의를 보고 정리하였습니다.
{: .notice--info}

## K-인접 이웃 기법 : K-Nearest Neighbor Learning

**Model-based Learning**

학습데이터로부터 설명변수 X와 종속변수 Y의 관계를 찾아내는 모델을 학습한 뒤, 새로운 객체의 설명변수 X를 학습된 모델에 투입하여 Y를 예측하는 방식 (Linear Regression, Logistic Regreesion, Decision Tree, Neural Network, $\cdots$)

**Instance-based Learning**

**별도의 모델을 학습하지 않고** 새로운 객체의 설명변수 X와 **유사한 학습 데이터 객체(이웃)**들을 찾아서 이들의 종속변수 Y의 정보들을 이용하여 새로운 객체의 Y를 예측하는 방식

### 특징

1. **Instance-based Learning** : 각각의 관측치만을 이용하여 새로운 데이터에 대한 예측을 시도 
2. **Memory-based Learning** : 모든 참조 데이터를 메모리에 저장한 후, 새로운 객체가 들어오면 이의 이웃을 탐색
3. **Lazy Learning** : 모델을 별도로 학습하지 않고, 새로운 테스트 데이터가 투입되는 시점에서 비로소 작동하는 게으른 알고리즘으로 학습 데이터라는 용어가 아닌 참조 데이터라는 용어를 사용 

### K-Nearest Neighbor Classification

#### **1. 참조 데이터 준비**

* 분류 목적 : 한 사람의 키, 몸무게, 체지방률을 이용해서 그 사람의 성별을 분류 

* 입력 변수 X : 키, 몸무게, 체지방률
* 종속변수 Y : 성별 (M/F)
* 각 범주로부터 충분한 수의 데이터 수집 

![1](https://user-images.githubusercontent.com/86525868/214059828-011c7d86-314e-4c60-938e-4de354567510.png){: width="50%" height="50%"}{: .align-center}



#### 2. 유사도 / 거리 측정 지표 정의 

**유사한 객체 = 거리가 가까운 객체** 

![2](https://user-images.githubusercontent.com/86525868/214059838-e915ff43-4a0f-4989-aaf4-44de36543a5b.png){: .align-center}

**고려 사항 : 정규화(Normalization) 또는 스케일링(Scaling)**

* 두 객체 사이의 거리를 계산할 때 정규화를 수행하지 않을 경우 측정 단위가 큰 변수가 측정 단위가 작은 변수보다 거리 계산에 더 큰 영향을 줌 → 예 : 은행 고객 데이터에서 두 개의 변수만 존재 (나이, 월 소득)

![3](https://user-images.githubusercontent.com/86525868/214059841-991ebcae-bebe-4890-a278-b938b45831c3.png){: width="70%" height="70%"}{: .align-center}



* [나이]와 [월 소득] 두 변수를 사용한 고객 A와 고객 B의 Euclidean Distance 

  $$
  \sqrt{(30-40)^2+(5,000,000-5,200,000)^2}=200,000
  $$

* [월 소득] 변수만 사용한 고객 A와 고객 B의 Euclidean Distance 

  $$
  \sqrt{(5,000,000-5,200,000)^2}=200,000
  $$

* 정규화 and Min-Max Scaling 

  $$
  Normalization \ z=\frac{x-\bar{x}}{s} \quad , \quad Min-Max \ Scaling=\frac{x-\min(x)}{\max(x)-\min(x)}
  $$
  
  예 : 나이의 평균이 35, 표준편차가 2.5, 월 소득의 평균이 5,100,000원, 표준편차가 100,000원

![4](https://user-images.githubusercontent.com/86525868/214059842-bfbf94f2-f407-442a-a6b2-a41e6f516646.png){: width="70%" height="70%"}{: .align-center}



  고객 A와 B의 Euclidean Distance를 측정할 때 정규화 후에는 [나이] 변수가 [월 소득] 변수보다 더 큰 영향을 미침 

* 두 객체 사이의 거리를 계산할 때 정규화를 수행하지 않을 경우 측정 단위가 큰 변수가 측정 단위가 작은 변수보다 거리 계산에 더 큰 영향을 줌 

![5](https://user-images.githubusercontent.com/86525868/214059844-7cf452a5-ba71-4644-81d5-9289e8cd1b72.png){: .align-center}

#### 3. k의 후보 집합 생성 

k 값의 변화에 따른 분류 경계면 예시 

![6](https://user-images.githubusercontent.com/86525868/214059846-537f7860-b4b2-4209-9adc-1e8983c5f7a4.png){: width="40%" height="40%"}{: .align-center}



* k가 매우 작게 설정되면 분류 경계면이 노이즈에 민감하게 되어 과적합의 우려가 있음
* k가 매우 크게 설정되면 적절한 지역적 구조를 파악하는 능력을 잃고 부적합의 경향성을 보임
* 적절한 k값을 찾아내는 것이 K-NN의 성능 최적화에 가장 중요한 요소이며, 일반적으로 충분한 범위의 k값들 중에서 검증 데이터 오류가 가장 낮은 값을 선택 

![7](https://user-images.githubusercontent.com/86525868/214059851-68098311-9d35-4fbd-a857-7663b01ce436.png){: width="70%" height="70%"}{: .align-center}



**최적의 k값을 선택하는 기준** : 설정된 k값의 탐색 범위에서 검증 데이터에 대한 오분류율이 가장 낮은 k값을 최종적으로 선택 

![8](https://user-images.githubusercontent.com/86525868/214059859-c9047913-94b7-4370-bd52-14f5d8f207a0.png){: width="40%" height="40%"}{: .align-center}



#### 4. k개의 이웃들로부터 범주 정보를 취합하여 예측 수행

이웃들의 거리정보 반영 유무에 따라 다수결방식과 가중합 방식이 있음 

**다수결 방식** : k개의 이웃들은 최종 예측에 동등한 영향력을 발휘 

![9](https://user-images.githubusercontent.com/86525868/214059862-a4b5afce-452d-4a33-b3e2-a1bf0e645c73.png){: width="40%" height="40%"}{: .align-center}



| Neighbor | 거리 | 성별 | 1 / 거리 | 가중치 |
| -------- | ---- | ---- | -------- | ------ |
| ●        | 0.55 | 남성 | 1.80     | 0.367  |
| ●        | 0.83 | 남성 | 1.20     | 0.244  |
| ▲        | 1.19 | 여성 | 0.84     | 0.172  |
| ▲        | 1.46 | 여성 | 0.68     | 0.139  |
| ▲        | 2.62 | 여성 | 0.38     | 0.078  |

↳ 객체의 가중치는 거리의 역수에 비례한다고 가정 

  남성일 확률 : $0.367 +0.244=0.611$ / 여성일 확률 : $0.192+0.139+0.078=0.389$ → **가중합 방식에 의하면 새로운 객체는 남성으로 판별**

### K-Nearest Neighbor Regression

이웃들의 거리정보 반영 유무에 따라 다수결 방식과 가중합 방식이 있음 

Step 4의 예측 결과물 산출 과정에서 연속형의 종속변수를 사용하므로 단순평균 혹은 가중합 평균을 사용 

대표적으로 추천 시스템과 협업 필터링에 사용 

![10](https://user-images.githubusercontent.com/86525868/214059867-308cf682-4cbb-40a0-a08b-6a992953d686.png){: width="50%" height="50%"}{: .align-center}



| 3-NN       | 거리 | 평점 | 1/거리 | 가중치 |
| ---------- | ---- | ---- | ------ | ------ |
| Neighbor 1 | 1    | 10   | 1.00   | 0.46   |
| Neighbor 2 | 1.5  | 9    | 1.67   | 0.31   |
| Neighbor 3 | 2    | 8    | 0.50   | 0.23   |

↳ 세 명의 이웃이 영화 D에 대하여 각각 10점, 9점, 8점 부여 → 가중 평균을 적용할 경우 이서윤은 영화 D에 9.23점을 부여할 것으로 예상하고 이 영화를 추천 

