# Pandas

## import pandas as pd

### 판다스란?

- 파이썬에서 사용할 수 있는 데이터 전처리용 패키지
  - 엑셀처럼 파일을 다룰 수 있다. 큰 용량의 엑셀파일도 처리 가능
- 데이터 분석
  - 분석 업무의 80%는 전처리

#### Series

- 1차원 구조를 표현

#### DataFrame

- 2차원 구조
- 여러개의 series가 모여서 하나의 데이터 프레임이 된다.
- `read_csv` 로 파일을 읽어올 수 있다.
- CSV(Comma Seperated Value) - 텍스트가 콤마로 구분된 형태의 파일
- type은 DataFrame

### 데이터 프레임 사용하기

#### 출력

- `head()` 상위 5개의 자료를 출력
- `tail()` 하위 5개의 자료를 출력, 인자값으로 출력 개수 조정
- `info()` 데이터의 요약된 정보
- `describe()` 데이터의 숫자타입에 대한 통계자료 출력, 인자값으로 (include='all') 설정하면 문자값도 반영

#### 열 인덱싱(indexing)

- 넘파이의 배열 인덱스와 유사
- 판다스의 데이터프레임은 기본적으로 **열우선 인덱스**
- `rawData['column name']` 으로 불러 올 수 있다.

#### 행(row) 인덱싱

- loc
- iloc
- `rawData.loc[0]` 으로 출력하고 해당 행을 series형태로 출력
- 판다스에서 슬라이스는 마지막 인덱스를 포함 [0:3] -> 0, 1, 2, 3 출력
- `.loc[[1, 3, 5]] or .reindex([1, 3, 5])` 처럼 해당 행 인덱스만 가져와서 출력

#### 행, 열 인덱싱

- `rawData.loc[0:3, '이름':'몸무게']`로 사용

#### loc Vs iloc

- iloc를 사용하면 column 인덱스에 정수를 사용할 수 있다.
- `.loc[0:3, '이름':'몸무게'] 대신 .iloc[0:3, 1:4]`로 사용 가능

#### 조건 검색

- boolean 인덱스 사용
- `rawData['지점'] == '여의도'` '지점' column이 '여의도'인 경우 True출력
- `rawData[ rawData['지점'] == '여의도' ]` DF형태로 출력
- 판다스의 논리연산자 사용 &(and), |(or), ~(not)로 사용
- `.isin(['원소1', '원소2', ...])` 를 통해 나열된 원소들이 속한 것들만 출력

#### 문자열 출력

- `rawData['담당'].str.contains('김')` 으로 '담당'에서 '김'이 있는 원소 출력
- `rawData['담당'].str.startswith('김')` 은 **첫 문자**가 '김'이 있는 원소 출력(성출력)

#### 결측치 처리

- 결측치를 채우는 방법
  - 평균, 중앙값 등으로 대체하는 경우
- 결측치를 확인하는 방법
  - `rawData['몸무게'].isnull()` 결측치의 boolean type
  - `rawData['몸무게'].isna()` 결측치의 boolean type
  - `rawData['몸무게'].isna().sum()` 결측치의 개수
  - `rawData['몸무게'].notna()` 결측치가 없는 데이터 출력

#### 이상치(outlier)

- 특별히 크거나, 작은 값 기준은 없다..
- .describe()로 확인하거나 육안으로 확인했을때 개발자가 주관적으로 판단

#### 중목 데이터 검사

- `.duplicated(subset=['지점'])` '지점'에서 중복된 데이터 검사

#### 컬럼의 추가와 수정

- `rawData['new column'] = '원소'` 를 입력하면 새로운 컬럼이 생성되고 '원소'값이 추가
- drop을 사용하는 경우 특정 컬럼이 삭제된 **새로운 데이터프레임**을 반환

#### apply

- 모든 행, 열에 대해서 동일한 함수를 반복적으로 적용

- 판다스에서 사용 가능한 반복문 정도로 이해

- ```python
  def func(x):
      print('함수가 호출되었습니다.')
      print('x : {}'.format(x))
      # apply를 사용할때는 꼭 return 값을 넣어줘야한다. -> return은 출력값의 맨 마지막에 나온다.
      return
  
  rawData.apply(func) # 인자값으로 함수가 들어간다.
  ```

#### 프레임 합치기

- INNER JOIN - 교집합
  - 자료의 크기가 훨씬 줄어든다.
  - 240개, 270개인 df를 join했을 때
  - `pd.merge(left=user_usage, right=user_device, on='use_id')` 'use_id'컬럼을 기준으로 왼쪽에는 useage, 오른쪽에는 device값들이 추가되고 두 df에서 'use_id'컬럼에 동일한 데이터가 있는 값들만 합쳐진다. 총 159개
- FULL JOIN - 합집합
  - `how='outer'`
- LEFT JOIN - 차집합
  - `how='left'` 매개변수 수정
- RIGHT JOIN - 차집합
  - `how='right'`
- 데이터 전처리 작업의 핵심은, 합치고, 나누고, 붙이고, 자르는 작업 - 무한 반복



## import maplotlib as plt

## import seaborn as sns

