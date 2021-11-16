# [실기 준비] Min-Max Scaling

> 주어진 데이터에서 'f5' 컬럼을 min-max 스케일 변환 후, 상위 5%와 하위 5% 값의 합을 구해라

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import MinMaxScaler

df = pd.read_csv('.../basic1.csv')
```

```python
scaler = MinMaxScaler()
df['f5'] = scaler.fit_transform(df[['f5']])
```

```python
higer = df['f5'].quantile(0.95)
lower = df['f5'].quantile(0.05)

print(higer+lower) # 1.0248740983597389
```



## Sklearn 없이 스케일 변환

```python
df2['f5'] = df2['f5'].transform(lambda x : (x - min(x)) / (max(x)-min(x)))
display(df2.head())

higer2 = df2['f5'].quantile(0.95)
lower2 = df2['f5'].quantile(0.05)

print(higer2+lower2) # 1.0248740983597389
```

