# [실기 준비 ] 작업형 1형 문제

파일 예시

```python
import pandas as pd

df = pd.read_csv('.../basic1.csv')
df.head()
```

![image-20211102145528356](markdown-images/image-20211102145528356.png)

## 연습 문제 1

- 데이터 셋의 `f5` 칼럼을 기준으로 상위 10개의 데이터 구하고

- `f5` 칼럼 10개 중 최소값으로 데이터를 대체 후

- 'age' 칼럼에서 80 이상인 데이터의 `f5` 칼럼 평균값 구하기

```python
df = df.sort_values('f5',ascending = False)
# 'f5' 기준 상위 10개의 데이터
min = df['f5'][:10].min()
# 10개중 최소값
df['f5'][:9] = min
```

<img src="markdown-images/image-20211102145806632.png" alt="image-20211102145806632" style="zoom:80%;" />   <img src="markdown-images/image-20211102145816590.png" alt="image-20211102145816590" style="zoom:80%;" />   

```python
mean = df[df['age']>=80]['f5'].mean()
# 62.497747125217394
```



## 연습 문제 2

- 데이터셋의 앞에서 순서대로 70% 데이터만 활용
- 'f1' 컬럼 결측치를 중앙값으로 채우기 전후의 표준편차를 구하고
- 두 표준편차 차이 계산하기

```python
# 데이터 나누기
df.shape() # (100,8)
data70 = df.iloc[:70]
# data70, data30 = np.split(df, [int(.7 * shape(df))])
```

```python
# 결측치 채우기 전 표준편차
std1 = np.std(data70['f1'])
# 17.794892986121205
```

```python
# 결측치 채운 후 표준편차
med = data70['f1'].median()
data70['f1'] = data70['f1'].fillna(med)

std2 = np.std(data70['f1'])
# 14.585462429615124
```

```python
std1 - std2
# 3.2094305565060814
```



## 연습 문제 3

- 데이터셋의 `age` 이상치를 더하기
- 단 평균에서 표준오차 1.5를 벗어나는 영역을 이상치라고 판단

```python
std = np.std(df['age']) * 1.5
mean = df['age'].mean()

max_out = mean + std
min_out = mean - std

age = df[(df['age'] > max_out)|(df['age'] < min_out)]['age'].sum()
# age = 473.5
