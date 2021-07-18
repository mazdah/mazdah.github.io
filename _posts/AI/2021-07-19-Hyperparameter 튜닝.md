---
layout: article
title: AI/ML(패스트 캠퍼스) - Hyperparameter 튜닝
excerpt: Hyperparameter 튜닝 - RandomizedSearchCV
mathjax: true
tags:
  - [AI, ML, Scikit-learn, Regression, Ensemble, 앙상블, Cross Validation, 교차검증, Hyperparameter, RandomizedSearchCV]
categories: AI
comments: true
header:
  theme: dark
  background: 'linear-gradient(135deg, rgb(34, 139, 87), rgb(139, 34, 139))'
article_header:
  type: overlay
  theme: dark
  background_color: '#203028'
  background_image:
    gradient: 'linear-gradient(135deg, rgba(34, 139, 87 , .4), rgba(139, 34, 139, .4))'
    src: /ai.jpg
---

# 하이퍼파라미터 튜닝

- Hyperparameter의 개수가 많은 경우 조합할 경우의 수가 너무 많으므로 자동화가 필요.

### sklearn의 튜닝 자동화 클래스
- **RandomizedSearchCV**
- **GridSearchCV**

### 적용 방법
1. 사용할 search 방법을 선택한다.
2. hyperparameter 도메인을 설정한다(max_depth, n_estimators 등).
3. 학습을 시킨다.
4. 도출된 결과 값을 모델에 적용하고 성능을 비교한다.

### RandomizedSearchCV
- 모든 매개 변수 값이 시도되는 것이 아니라 지정된 분포에서 고정된 수의 매개 변수 설정이 샘플링 됨
- 시도 된 매개 변수 설정의 수는 n_iter에 의해 제공됨

### 주요 Hyperparameter (LGBM)
- random_state : 랜덤 시드 고정 값. 고정해두고 튜닝할 것.
- n_jobs : CPU 사용 개수
- learning_rate :  학습률. default=0.1, n_estimators와 여관지어 튜닝.
- n_estimators : 부스팅 스테이지 수(랜덤포레스트의 트리 개수 설정과 비슷한 개념). default=100
- max_depth : 트리의 깊이. 과대적합 방지용. default=3
- colsample_bytree : 샘플 사용 비율(max_features와 비슷한 개념). 과대적합 방지용. default=1.0

- - - -

``` python
params= {
    'n_estimators' : [200, 500, 1000, 20000],
    'learning_rate' : [0.1, 0.05, 0.01],
    'max_depth' : [6, 7, 8],
    'colsample_bytree' : [0.8, 0.9, 1.0],
    'subsample' : [0.8, 0.9, 1.0]
}

from sklearn.model_selection import RandomizedSearchCV
```

- - - -

- n_iter 값을 조절하여 총 몇회의 시도를 진행할 것인지 정의한다.
- 회수가 늘어나면 더 좋은 파라미터를 찾을 확률은 높아지나 시간이 오래 걸린다.

- - - -

``` python
clf = RandomizedSearchCV(LGBMRegressor(), params, random_state=42, cv=3, n_iter=25, scoring='neg_mean_squared_error')
clf.fit(x_train, y_train)

clf.best_score_
'''
출력
-13.642938938945603
'''

clf.best_params_
'''
출력
{'colsample_bytree': 0.8,
 'learning_rate': 0.1,
 'max_depth': 6,
 'n_estimators': 500,
 'subsample': 1.0}
'''

lgbm = LGBMRegressor(random_state=42, learning_rate=0.1, n_estimators=500, colsample_bytree=0.8, subsample=1.0, max_depth=6)
lgbm.fit(x_train, y_train)
lgbm_pred = lgbm.predict(x_test)

```
