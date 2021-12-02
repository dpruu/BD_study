# 시험 전에 한번 봐두기




## 실기형 2 라이브러리

### Scaling, Transforming and Encoding

> **Scaling**
>
> Scaling이란 변수의 단위를 조정하기 위해 변수를 변환하는 작업을 말합니다. 대표적인 scaling에는 표준화(Standardization)와 정규화(Normalization)가 있는데, 각각의 용어는 다음의 차이가 있다.
>
> - 정규화 : 변수의 범위를 [0,1] 사이로 옮기는 작업
> - 표준화 : 변수로부터 평균값을 빼고, 다시 변수의 표준편차로 나누어 결과적으로 변수가 평균 0, 표준편차 1을 갖도록 맞춰주는 작업
>
> Scaler로 변환시 train 데이터에는 **fit_transform**, test 데이터에는 **transform** 사용.

```python
# StandardScaler : 평균 0, 분산 1로 조정
from sklearn.proprecessing import StandardScaler
## 예시
scaler = StandardScaler()
df['col'] = scaler.fit_transform(df[['col']])

# LabelEncoder : 문자를 0부터 시작하는 정수형 숫자로 바꿔주는 기능
from sklearn.proprecessing import LabelEncoder
## 예시
encoder = LabelEncoder()
df['col'] = encoder.fit_transfrom(df['col'])

# MinMaxScaler : 모든 값을 0 ~ 1사이의 값으로 변경, 정규화 방법 중 원데이터 분포를 유지하면서 정규화
from sklearn.proprecessing import minmax_scale
from sklearn.proprecessing import MinMaxScaler
## 예시
df['col'] = minmax_scale(df['col'])

scaler = MinMaxScaler()
df['col'] = scaler.fit_transform(df[['col']])
```
#### 알아두기

##### 1. QuantileTransformer

```python
# QuantileTransformer : 1000개 분위를 사용해 데이터를 균등 분포
from sklearn.preprocessing import QuantileTransformer
quantile = QuantileTransformer(n_quantiles=100, random_state=42, output_distribution='normal')
df = quantile.fit_transform(df)
```

> **n_quantiles** : 계산할 분위 수. 샘플 개수보다 큰 경우 샘플 개수로 설정
>
> **output_distribution**  : 변환된 데이터의 한계 분포, {uniform, normal} 둘 중 하나로 설정 가능. 이때 normal로 지정시 정규 분포로 변환

##### 2. PowerTransformer

```python
from sklearn.preprocessing import PowerTransformer
power = PowerTransformer(method = 'yeo-johnson')
df = power.fit_transform(df)
```

> method = {yeo-johnson, box-cox}
>
> - yeo-johnson : 양수 및 음수 값으로 작업
> - box-cox : 양수 값에서만 작동





### 모델 선정
```python
from sklearn.model_selecion import train_test_split
# training, test 데이터를 나누는 라이브러리
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25, shuffle=True, stratify=X['col'])
# 하이퍼파라미터
# test_size : 테스트 셋 구성의 비율, default = 0.25
# shuffle : split이전 데이터 섞을건지 여부, default = True
# stratify : classification을 다룰 때 중요, class 비율을 train, 
# 			 validation에 유지, default = None

from sklearn.model_selecion import cross_val_score
# 교차 검증 
model = RandomForestClassifier()
scores = cross_val_score(model, X=X_train, y=y_train, cv=5, scoring='neg_mean_squared_error', n_jobs=-1)
# 파라미터
# estimator : 학습할 모델
# cv : Fold의 수를 의미
# scoring : 평가한 점수를 각각 교차 검증을 반복 시마다 기록해 돌려준다.
# n_jobs : 병렬로 실행할 작업 수 (-1은 전부)

from sklearn.model_selecion import GridSearchCV
# 분류, 회귀 알고리즘에 사용되는 하이퍼파라미터를 순차적으로 입력해 학습 하고 측정을 하면서 가장 좋은 파라미터를 확인.
gbr_search = GridSearchCV(gbr, param_grid=gbr_params, cv=5, scoring='neg_mean_squared_error', n_jobs=-1)
# 파라미터
# param_grid : 튜닝을 위해 파라미터, 사용될 파라미터를 dict형태로 만들어서 넣는다
# cv : 교차 검증에서 몇개로 분할되는지 지정
# scoring : 예측 성능 평가 방법
gbr_search.fit(X_train, y_train)
best_mse = (-1) * gbr_search.best_score_
best_rmse = np.sqrt(best_mse)

gbr_final = GradientBoostingRegressor(**gbr_search.best_params_)
gbr_final.fit(X_train, y_train)
y_pred_gbr = gbr_final.predict(X_test)
```

> **cross_val_score**를 이용해 모델 선정 후 **GridSearchCV**를 이용해 파라미터 값 선정



### 분류 모델

> 각 모델의 하이퍼파라미터 생각이 안 날 경우 **help()** 함수 사용

```python
# (1) LogisticRegression
from sklearn.linear_model import LogisticRegression
model = LogisticRegression()
model.fit(X,Y)
pred = model.predict(X_test)
# 확률 확인 시 model.predict_proba()
# 하이퍼파라미터
# C : 규칙의 강도의 역수 값, C의 값이 클수록 overfitting 위험
# random_state : 랜덤 시드 생성

# (2) RandomForestClassifier
from sklearn.ensemble import RandomForestClassifier
model = RandomForestClassifier(random_state = 2022, n_estimators = 100, max_depth = 10)
md.fit(X,Y)
# 하이퍼파라미터
# n_estimators : 모델에서 사용할 트리 개수
# max_depth : 트리의 최대 깊이

# (3) DecisionTree
from sklearn.tree import DecisionTreeClassifier
md = DecisionTreeClassifier(random_state = 0, max_depth = 20)
md.fit(x_train,y_train)
# 하이퍼파라미터
# max_depth : 트리의 최대 깊이 (값이 클수록 모델의 복잡도가 올라간다.)

# (4) SVC
from sklearn.svm import SVC
md = SVC(C = 10, gamma = 1, random_state = 0, probability =True)
# predict_proba 사용시, probability = True는 반드시 추가해야한다.
md.fit(x_train,y_train)
# 하이퍼파라미터
# C : decision boundary에의 굴곡을 조절, 값이 크면 굴곡, 작으면 직선에 가깝
# gamma : decision boundary 굴곡에 영향을 주는 데이터의 범위
# 값을 조절해 Overfitting 방지해야 한다.


# (5) 신경망
from sklearn.neural_network import MLPClassifier
md = MLPClassifier(random_state = 0,alpha = 0.02, hidden_layer_sizes = [100])
md.fit(x_train,y_train)
# 하이퍼파라미터
# hidden_layer_sizes : 은닉층(hidden layer)의 개수
# alpha : 정규화 파라미터
# learning_rate : 가중치를 업데이트 할때 크기 제어
# solver : 가중치 최적화를 위해 사용하는 함수 default = 'adam'
```



### 회귀 모델

```python
from sklearn.linear_model import LinearRegression
from sklearn.tree import DecisionTreeRegressor
# 파라미터
# 'max_depth': 최대 깊이,  default : None, ex) [3, 5, 7, 9],
# 'min_samples_split': 내부 노드를 분할하는 데 필요한 최소 샘플 수, default : 2, ex)[2, 3, 4, 5]
# 'min_samples_leaf': 리프 노드에 있어야하는 최소 샘플 수 default : 2 ,ex)[1, 2, 3, 5],

from sklearn.ensemble import RandomForestRegressor
# 파라미터
# 'n_estimators': 트리의 개수
# 'max_depth': 트리 최대 깊이[3, 5, 7, 9],
# 'min_samples_split': 내부 노드를 분할하는 데 필요한 최소 샘플 수
# 'min_samples_leaf': 리프 노드에 있어야하는 최소 샘플 수


from sklearn.ensemble import GradientBoostingRegressor
# 파라미터
# loss : 최적화활 손실함수 선택(default = 'ls')
# 'learning_rate': 각 트리 기여도,(default = 0.1)[0.01, 0.05, 0.1],
# 'n_estimators': 부스팅 간계의 수[50, 80, 100, 200, 300],

from xgboost import XGBRegressor
# 파리미터
# n_estimators : 트리 개수
# objective : "binary:logistic“, “reg:linear”“, “multi:softmax” = 이항 / 연속 / 다항 
# max_depth : 트리의 최대 깊이
```

