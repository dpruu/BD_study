# [실기 준비] Slicing & Condition

> 주어진 데이터 셋에서 age컬럼 상위 20개의 데이터를 구한 다음
>
> f1의 결측치를 중앙값으로 채운다.
>
> 그리고 f4가 ISFJ와 f5가 20 이상인 f1의 평균값을 출력하시오!

```python
# 라이브러리 및 데이터 불러오기
import pandas as pd
import numpy as np

df = pd.read_csv('/content/drive/MyDrive/분석기사 자격증/basic1.csv')
df = df.sort_values('age', ascending = False).head(20)
display(df.head())
```

```python
# 'f1' 결측치를 중앙값으로 채우는 작업
df['f1'] = df['f1'].fillna(df['f1'].median())
display(df.head())
```

```python
# f4가 ISFJ와 f5가 20 이상인 f1의 평균값을 출력하시오!
answer = df[(df['f4'] == 'ISFJ') & (df['f5'] >= 20)]['f1'].mean()
print(answer)	# 73.875
```

