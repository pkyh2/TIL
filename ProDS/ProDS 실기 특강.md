# ProDS 실기 특강

- 스파이더 사용
- 비즈니스 관점

- F9번 키로 실행



- 합격률이 좋을때가 30 ~ 40% (입문)



- cov(공분산) = E[(X - m)(Y - m)]
- cor(상관계수)



- y입장에서 가장 영향력을 가장 많이 주는 변수
  - 절대치가 높은 변수를 찾아라

```
ValueError: Expected 2D array, got 1D array instead:
array=[16. 13. 41. ... 44. 71. 42.].
Reshape your data either using array.reshape(-1, 1) if your data has a single feature or array.reshape(1, -1) if it contains a single sample.

shape형태 변경
```



### 결측치 확인 함수들

- isna()
- fillna()



- summary() 함수 내용
  - dep 종속변수 target 변수