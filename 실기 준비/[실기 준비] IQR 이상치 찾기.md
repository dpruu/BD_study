# 이상치 찾기

- IQR 활용해 'Fare' 칼럼 이상치 찾기 *<u>(자료 kaggle 타이타닉 예제)</u>*
- 이상치 값중 여성의 수를 구하라

```python
import pandas as pd
import numpy as np

df = pd.read_csv('.../train.csv')
display(df.head())
```

![image-20211104151616010](C:/Users/jinsa/AppData/Roaming/Typora/typora-user-images/image-20211104151616010.png)

```python
Q1 = np.percentile(df['Fare'],25)
Q3 = np.percentile(df['Fare'],75)
IQR = Q3-Q1
```

```python
out_data = df[(df['Fare'] < Q1 - 1.5*IQR)|(df['Fare'] > Q3 + 1.5*IQR)]
print(len(out_data)) # 116
print(sum(out_data['Sex']=='female'))
```

