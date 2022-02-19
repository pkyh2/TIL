### 문제 01 유형

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

   