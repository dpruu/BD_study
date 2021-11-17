# [실기 준비] Variance

> 주어진 데이터 셋에서 f2가 0값인 데이터를 age를 기준으로 오름차순 정렬하고
>
> 앞에서 부터 20개의 데이터를 추출한 후
>
> f1 결측치(최소값)를 채우기 전과 후의 분산 차이를 계산하시오 (소수점 둘째 자리까지)

```python
# 데이터 및 라이브러리 불러오기
import pandas as pd
import numpy as np
df = pd.read_csv('.../basic1.csv')

# 'f2'가 0인 데이터 중, 'age' 기준 오름차순 정렬 후 상위 20개 추출
df = df[df['f2']==0].sort_values('age', ascending = True).head(20)
display(df)
```

```python
# 결측치 처리 전 분산
df_var1 = df['f1'].var()
display(df.head())
print(df_var1) # 351.7636363636363

# 결측치 처리 후 분산
df['f1'] = df['f1'].fillna(df['f1'].min())
df_var2 = df['f1'].var()
display(df)
print(df_var2) # 313.32631578947377

# 분산 차이
print(round(df_var1 - df_var2,2)) # 38.44
```