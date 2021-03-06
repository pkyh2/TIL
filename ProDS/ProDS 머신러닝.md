# 머신러닝

## 머신러닝의 기초

### 지도학습

- 라벨이 있는 훈련용 데이터에서, <u>여러 특성변수</u>를 이용하여 목표인 라벨을 예측하도록 모델을 학습함.
  - 훈련용 데이터는 원인에 따른 결과값이 주어진 형태의 데이터
- 라벨의 데이터 타입에 따라 연속형이면 **회귀(regression)**, 범주형이면 **분류(classification)**
  - 회귀 : 중고차의 연식, 모델명, 색상, 마모 정도 등등의 특성변수를 이용하여 그 중고차의 가격이 얼마인지 예측하는 알고리즘을 회귀라고한다. (가격은 숫자로 표현되는 연속형)
  - 분류 : 어떤 사람의 여러가지 정보(소득, 나이, 자녀의 수 등등)로 연체를 할 것이냐 말 것이냐와 같은 범주형을 예측하는 문제를 분류라고 한다.

### 비지도 학습

- 라벨이 없는(y값이 없는) 훈련용 데이터에서 특징 변수들 간의 관계나 유사성을 기반응로 의미 있는 패턴을 추출.

#### 군집화

- 거리가 가까운 애들끼리 그룹을 만들어 준다. (클러스터링)

#### 차원축소

- 10만개의 데이터
- 데이터가 너무 많아 특성 변수들만 모아서 차원 축소를 한다.
- 소수의 특성변수만 남긴다.

#### 추천시스템

#### 강화학습



### 모델 기반 지도학습 알고리즘의 일반적인 분석 절차

- 주어진 데이터 전처리 탐색
- 적절한 모델을 선택
- 주어진 데이터로 모델을 훈련시킴
- 훈련된 모델을 적용하여 새로운 데이터에 대한 예측을 수행



#### 과대적합 : Test Error가 Training Error보다 너무 높은 경우

#### 그러나 모형이 너무 단순화 되면 과소적합이 된다. fitting이 안좋은 경우

### k-fold 교차검증

- 3-fold CV(주로 5 ~ 10-fold)
  - train 1, 2 or 1, 3 or 2, 3
  - test 3 or 2 or 1
  - 성능1, 성능2, 성능3 의 Avg

### 과대 적합을 막기 위한 방법

1. 훈련데이터를 많이 확보
2. 모델의 복잡도를 낮춤
   - 특성 변수의 수를 줄이거나 차원 축소
   - 파라미터에 규제를 적용.



### 지도학습 모델의 평가지표

- 레이블이 범주형인 경우 - 분류 모델
  - y가 범주로 표현
- 연속형 숫자형인 경우 - 회귀 모델
  - y가 숫자로 표현

- 평가 : test 데이터의 y값을 train 데이터로 훈련한 모델로 test의 x값을 넣어 출력되는 y값과 얼마나 비슷한지 비교하는 작업

- R-square
  - 선형회귀 모델에서 계산된 경우 0 ~ 1사이의 값을 가지고
  - 0이면 모형이 굉장히 안 좋은 상태, 1이면 모델이 완벽한 피팅을 한 상태 = 오차가 0인 상태
  - 그래서 결정계수 값은 클수록 1에 까까울 수록 좋다.

#### 분류 모델 평가 지표

- |                | Negative    | Positive    |
  | -------------- | ----------- | ----------- |
  | Negative(실제) | A<br />(TN) | B<br />(FP) |
  | Positive(실제) | C<br />(FN) | D<br />(TP) |

  - 정확도 - 전체 관찰치 중 정 분류된 관찰치의 비중 
    (TN + TP) / TN + FP + FN + TP
  - 정밀도(Precision) - Positive로 예측한 것 중에서 실제 범주도 Positive인 데이터의 비율
    TP / FP + TP
  - 재현율(Recall) - 실제 범주가 Positive인 것 중에서 Positive로 예측된 데이터의 비율.
    TP / FN + TP



## Feture Engineering

### 특성공학

- 특성 선택 : 훈련 데이터들 중에서 y를 잘 예측하는 x값들만 뽑는것
- 특성 추출 : 훈련 데이터들을 결합하거나 변형을 통해 새로운 변수를 만들어 낸다.

#### 특성선택

1. Filter 방식 :  각 특성변수를 독립적인 평가 함수로 평가 (1 대 1)
   - 특성하나와 y값 하나를 평가해서 rank를 선정한 뒤 높은 순위의 특성을 선택
   - 실제로 모델을 학습하진 않음, ttest검정이나 카이검정

2. Wrapper 방식 : 학습 알고리즘을 이용. (다 대 1)
   - 다양한 특성 변수의 조합을 통해 그 조합끼리 학습을 시킨다.
   - 특성이 k개 있을때 조합은 2**k - 1개의 조합 생성가능
3. Embedded 방식 : 알고리즘자체에서 변수선택을하고 최적화를 한다.



## 단순회귀분석

- 독립변수(x), 종속변수(y)
- 단순 회귀 : 독립변수가 1개
- 다중 회귀 : 독립변수가 2개 이상
- 독립변수는 데이터 타입에 제한이 없고 <u>종속변수가 연속형이어야 한다.</u>
- ex) 매출액 = f(광고비), 아파트가격 = f(면적) -> 단순회귀분석
- 매출액 = f(광고비, 가격, 품질) -> 다중 회귀
- 다변량 회귀 : 종속변수가 2개 이상일 때

**모형 식** - y의 기대값을 x의 직선함수로 표현

**모수 추정** - 최소제곱법

**모수에 관한 검정** - 모수 B(베타)가 0인지 여부에 관한 t검정

**모형의 적합도** - 결정계수(R-squared)

### 단순선형회귀모형 사례

- 주행속도(x), 정지거리(y), 50쌍



## 다중회귀분석

- 여러개의 변수들 중에 한 변수의 계수B(베타)의 해석은 나머지 모든 변수들을 상수로 고정시킨 상태에서 측정한다.
- 다중회귀분석의 변수의 계수가 단순회귀분석에서의 변수의 계수보다 더 영향이 크다
- 회귀계수의 추정
  - 최소제곱법 - 자료들의 근처를 지나가는 평면을 찾을 수 있는가?
  - 각 자료점과 평면과의 수직거리의 제곱합이 최소가 되는 평면을 찾겠다.
- 독립변수가 두 개 이상인 선형회귀모형
- <u>오차가정 : 정규분포, 등분산, 독립</u>
- 회귀계수 베타 해서: Partial effect -> 나머지 변수들은 고정하고 한 단계증가

### 범주형 독립변수가 포함된 회귀분석

- 범주형 독립변수를 회귀모형에 포함하기 위해서는 더미변수 기법을 사용
- 더미변수의 개수 = 범주의 개수 - 1

### 결정계수(R-square)

- 다중선형회귀모형을 적합한 결과 총제곱합(SST) = 60, 오차제곱합(SSE) = 15 일때, 적합된 회귀모형의 결정계수(r-square)는?
  - `1 - SSE / SST = 1- 15 / 60 = 0.75`

### 다중회귀모형의 변수 선택

wrapper방법

가능한 적은 수의 설명변수로 좋은 예측력을 가지는 모형을 찾고자 함.

- 전진선택법 - 절편만 있는 모델에서 출발하여 중요한 변수를 하나씩 추가하는 방식
- 후진제거법
- 단계선택법 - 전진하고 후진하고
- 회귀모형의 적합도 지표
  - 종속변수를 잘 설명하지 못하는 독립변수가 추가되는 경우, <u>결정계수는 항상 증가하지만</u>, 수정결정계수의 경우는 그렇지 않다.
  - 결정계수는 `1-(SSE/SST)`이고, 수정결정계수는 `1-(n-1)/(n-k-1)*(SSE/SST)` 이므로 동일한 조건에서 결정계수보다 항상 값이 조금 더 작다.



### 다중회귀모형의 가정 위반 검토 및 해결

- 잔차 분석방법
  - 오차의 정규성 위반 : 히스토그램, QQ플롯
  - 오차의 등분산성 : 잔차산점도
  - 오차의 독립성 : 잔차산점도
- 가정 위반 시 해결방안
  - 오차의 정규성 위반 : 변수변환
  - 오차의 등분산성 : 가중최소제곱회귀
  - 오차의 독립성 : 시계열 분석

### 다중공선성 진단 및 해결

- 다중공선성
  - 독립변수들 간에 **강한 선형관계**가 존재
  - 다중회귀모형 분석 시 자주 발생하는 문제
  - 다중회귀모형에서 회귀계수 추정에 부정적인 영향을 미침.
- 다중공선성의 해결책
  - 변수선택으로 **중복된 변수를 제거**
  - 주성분 분석 등을 이용하여 중복된 변수를 변환하여 **새로운 변수 생성**
  - 릿지, 라쏘 등으로 중복된 변수의 영향력을 일부만 사용
- 다중공선성 문제를 진단하기 위한 지표로 적절한 것은? **분산팽창요인(VIF)**
  - 5나 10등으로 큰 값이 나오면 다중공선성이 있는 것으로 진단
- **선형관계** : 두 데이터 값 사이에 직선식의 형태가 있으면 선형관계가 있다고 한다.

### 규제가 있는 선형회귀모델

- 모델의 과대 적합을 방지
- 



## 로지스틱 회귀

- 분류알고리즘
- 지도학습 알고리즘
- Y가 범주형

### 로지스틱 회귀모형



### 이항 로지스틱 회귀모형

- 반응변수 Y는 1 또는 0의 값을 가지는 이진 변수





## 의사결정트리

### 회귀 나무

#### 의사결정나무의 과적합 방지 방법

- 많은 마디를 가지는 의사결정나무는 새로운 자료에 적용할 때 예측오차가 매우 커지는 **과적합** 생태가 됨.

- 정지규칙(stopping rule) - 다음의 경우 분리를 하지않고 나무가 성장을 멈춤
  - 모든 자료의 목표변수 값이 동일할 때
  - 마디에 속하는 자료의 개수가 일정 수준보다 적을 때
  - 뿌리마디로부터의 깊이가 일정 수준 이상일 때
  - 불순도의 감소량이 지정된 값보다 적을 때
- 가지치기(pruning) - 성장이 끝난 나무의 가지를 제거

#### 의사결정나무모형의 특징

#### 장점

- if-then-else 방식 : 이해하기 쉬움
- 특성, 목표변수 둘다 연속형, 범주형 자료 모두 취급
- 전처리가 거의 필요없음
- 이상치에 덜 민감
- 모형에 가정이 필요없는 **비모수적**

#### 단점

- 훈련결과가 불안정
- 모든 분할은 축에 수직
- 나무가 깊어질수록 과적합으로 예측력이 저하되며, 해석이 어려워짐
