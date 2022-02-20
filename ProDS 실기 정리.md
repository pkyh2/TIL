## 문제 01 유형

1. 결측값의 개수는 몇개 인가?

   ```python
   # 방법1
   data01.isna() #전체 df에서 결측치인 값은 TRUE
   data01.isna().sum() # 각 column별 결측치 개수를 series형태로 반환
   data01.isna().sum().sum() # 총 결측치의 개수
   
   # 방법2
   data01.isna().any(axis=1)	# 전체 행에서 결측치가 있는 행은 TRUE
   data01.isna().any(axis=1).sum() # TRUE(1)값의 총합
   ```

2. 각 변수들 간의 상관관계 분석

   ```python
   # TV, Radio, Social Media과 매출액 Sales와의 상관관계
   
   # 상관관계 분석 함수
   data01.corr()
   
   # 문제에서 필요없는 행과 제거 후 '매출액(Sales)' 컬럼 선택
   data01.corr().drop('Sales')['Sales']
   
   # 모든 값들을 절대값을 취하고 그중 가장 큰 값 선택
   data01.corr().drop('Sales')['Sales'].abs().max()
   
   # 문제에서 요구하는 소수점 5번째 자리에서 반올림
   round(data01.corr().drop('Sales')['Sales'].abs().max(), 4)
   
   # 추가 함수
   data01.corr().drop('Sales')['Sales'].abs().idxmax()				# max값의 index행의 값
   data01.corr().drop('Sales')['Sales'].abs().nlargest(2).index	# 상위 2개의 값
   data01.corr().drop('Sales')['Sales'].abs().argmax()				# max값의 위치
   
   # 조건으로 값 추출
   cor_dat = data01.corr().drop('Sales')['Sales'].abs()
   cor_dat > 0.6	# 0.6이 넘는 값들에 대해 boolean형태로 출력
   cor_dat.index[cor_dat > 0.6]	# 조건이 TRUE인 값의 index행의 값 출력 ['TV', 'Radio']
   ```

3. 회귀분석 수행

   ```python
   # 독립변수 : TV, Radio, Social Media
   # 종속변수 : 매출액
   # 회귀분석을 수행하였을때 3개의 독립변수 중 회귀계수가 큰 것에서 작은 것 순으로 기술
   # 결측치가 포함된 행은 제거하고, 회귀계수는 넷째 자리 이하는 버리고 기술
   
   # 결측치 행 제거
   q2 = data01.dropna()
   
   # ols함수로 회귀분석
   # ols함수 ols('종속변수~' + '+'.join(독립변수들), data=df_name).fit()
   from statemodels.formula.api import ols
   
   var_list = ['TV', 'Radio', 'Social Media']
   form1 = 'Sales~' + '+'.join(var_list)
   lr = ols(form1, data=q2).fit()
   lr.summary()
   
   # 회귀계수 추출
   coef_value = lr.params.drop('Intercept')	# lr에 params.drop('') 를통해 불필요한 행 제거
   coef_value.sort_values()	# sort_values(by='column_name') 설정 컬럼기준으로 오름차순
   coef_value.sort_values(ascending=False)		# ascending=False 내림차순 정렬
   coef_value.sort_values(ascending=False).index	# index행 이름 출력
   coef_value.sort_values(ascending=False).values	# 해당 정렬하는 컬럼 값 출력
   ```

   **ols().fit().summary()**

   ![image-20220219204451482](ProDS 실기 정리.assets/image-20220219204451482.png)

   - `Dep. Variable` : 종속변수
   - `Model` : 회귀분석 모델(OLS)
   - `Df Residuals` : 자유도(전체 표본 수 4546 - 독립변수 3개 - 종속변수 1개 = **4542**)
   - `Df Model` : 독립변수의 개수
   - `R squared` : 결정계수
   - `F-statistics` : F 통계량 = MSR/MSE
   - `Prob` : F통계량에 해당하는 P-value
   - `Intercept coef` : 회귀식의 절편 값(상수)
   - `독립변수 coef` : 각 독립변수의 회귀계수, 회귀식에서 기울기
   - 회귀식 : `y = 3.5626X1 - 0.0040X2 + 0.0050X3 - 0.1340`
   - `P > |t|` : p-value값

   **ols().fit().pvalues**

   - 독립변수들의 pvalues값
   - `lm2.pvalues.index[lm2.pvalues < 0.05]` : pvalues의 index행 값들중 0.05 미만인 index출력



## 문제 02 유형

- **EDA(Exploratory Data Analysis with Python)**
  - 탐색적 데이터 분석

1. 성별이 여성, 혈압이 High, Cholesterol이 Normal인 환자의 전체에 대한 비율

   ```python
   data02['Sex']	# Sex열 출력 series type
   data02['Sex', 'BP']		# Error
   data02[['Sex', 'BP']]	# Sex, BP열 출력 dataframe type
   
   # 'Sex', 'BP', 'Cholesterol' 컬럼만 추출
   q1 = data02[['Sex', 'BP', 'Cholesterol']]
   
   q1_len = len(q1[(q1['Sex']=='F') & (q1['BP']=='HIGH') & (q1['Cholesterol']=='NORMAL')])
   
   # 전체 환자 수에서 조건에 해당하는 환자수의 비율 계산
   q1_len / len(data02)
   
   # 강사님 방법
   # value_counts() 동일한 요소들이 몇개나 있는지 출력 (normalize=True)면 비율로 계산
   q1 = data02[['Sex', 'BP', 'Cholesterol']].value_counts(normalize=1)
   q1[('F', 'HIGH', 'NORMAL')]		# 해당요소를 만족하는 비율계산
   ```

   