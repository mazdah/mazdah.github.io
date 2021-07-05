---
layout: article
title: AI/ML(패스트 캠퍼스) - Boosting 2
excerpt: XGBoost와 LightGBM
mathjax: true
tags:
  - [AI, ML, Scikit-learn, Regression, Ensemble, 앙상블, Boosting, XGBoost, LightGBM]
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

# Ensemble - XGBoost와 LightGBM

---

### XGBoost (eXtreme Gradient Boosting)
- scikit-learn 패키지가 아님
- 성능이 우수함
- GBM보다는 빠르고 성능도 향상되었음
- 학습시간은 느림

주요 Hyperparameter

- random_state : 랜덤 시드 고정 값. 고정해두고 튜닝할 것!
- n_jobs : CPU 사용 개수
- learning_rate : 학습률. 너무 큰 학습률은 성는 저하를, 너무 작은 학습률은 속도 저하를 일으킴. n_estimator와 연계하여 튜닝. default=0.1
- n_estimator : 부스팅 스테이지 수 (랜덤포레스트의 트리 개수 설정과 비슷한 개념). default=100
- max_depth : 트리의 깊이. 과대적합 방지용. default=3
- subsample : 샘플 사용 비율. 과대적합 방지용. default=1.0
- max_features : 최대로 사용할 feature의 비율. 과대적합 방지용. default=1.0

---

``` python
from xgboost import XGBRegressor

xgb = XGBRegressor(random_state=42, learning_rate=0.01, n_estimators=1000, subsample=0.8, max_features=0.8, max_depth=7)
xgb.fit(x_train, y_train)
xgb_pred = xgb.predict(x_test)

```

---

### LightGBM

- scikit-learn 패키지가 아님
- 성능이 우수함
- 속도가 매우 빠름

주요 Hyperparameter

- random_state : 랜덤 시드 고정 값. 고정해두고 튜닝할 것!
- n_jobs : CPU 사용 개수
- learning_rate : 학습률. 너무 큰 학습률은 성는 저하를, 너무 작은 학습률은 속도 저하를 일으킴. n_estimator와 연계하여 튜닝. default=0.1
- n_estimator : 부스팅 스테이지 수 (랜덤포레스트의 트리 개수 설정과 비슷한 개념). default=100
- max_depth : 트리의 깊이. 과대적합 방지용. default=3
- colsample_bytree : 샘플 사용 비율(max_features와 비슷한 개념). 과대적합 방지용. default=1.0

---

``` python
from lightgbm import LGBMRegressor

lgbm = LGBMRegressor(random_state=42, learning_rate=0.01, n_estimators=2000, colsample_bytree=0.8, subsample=0.8, max_depth=7)
lgbm.fit(x_train, y_train)
lgbm_pred = lgbm.predict(x_test)
```
