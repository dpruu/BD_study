# [실기 준비] Yeo-Johnson and Box–Cox

> 주어진 데이터에서 20세 이상인 데이터를 추출하고 'f1' 컬럼을 결측치를 최빈값으로 채운 후, f1 컬럼의 여-존슨과 박스콕스 변환 값을 구하고, 두 값의 차이를 절대값으로 구한다음 모두 더해 소수점 둘째 자리까지 출력(반올림)하시오
>
> 데이터셋 : basic1.csv 오른쪽 상단 copy&edit 클릭 -> 예상문제 풀이 시작

```python
# 라이브러리 및 데이터 불러오기
import pandas as pd

df = pd.read_csv('.../basic1.csv')
df = df[df['age'] >= 20]
```

```python
# 결측치 최빈값 보간
df['f1'] = df['f1'].fillna(df['f1'].mode()[0])
```



## box-cox와 Yeo-Joghnson 변환

> **box-cox**의 주된 용도는 데이터를 정규분포에 가깝게 만들거나 데이터의 분산을 안정화하는 것으로 양수 값에서만 작동,  **Yeo-Johnson Transformation**는 실수 전체에 대해 일반화

```python
from sklearn.proprecessing import power_transform

df['y'] = power_transform[df[['f1']], method = 'yeo_johnson', standardize = False]
df['b'] = power_transform[df[['f1']], method = 'box-cox', standardize = False]

round(sum(np.abs(df['y'] - df['b'])),2)
# 39.17
```



