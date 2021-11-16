# [실기 준비] Correlation

> 상관관계 구하기 주어진 데이터에서 상관관계를 구하고, quality와의 상관관계가 가장 큰 값과, 가장 작은 값을 구한 다음 더하시오! 단, quality와 quality 상관관계 제외, 소수점 둘째 자리까지 출력

```python
import pandas as pd
import numpy as np

df = pd.read_csv('.../winequality-red.csv')
```

```python
df_corr = df.corr()
df_corr = df_corr[:-1] # 'quality' 제외
# 위 'quality' 제거 전
# 아래 'quality' 제거 후

# 'quality'와 상관관계 가장 큰 값, 작은 값 
df_corr_max = df_corr['quality'].max()
df_corr_min = df_corr['quality'].min()

round(df_corr_max + df_corr_min,2)
```

![image-20211116145935913](C:/Users/jinsa/AppData/Roaming/Typora/typora-user-images/image-20211116145935913.png)

