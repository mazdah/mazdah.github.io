---
layout: article
title: AI/ML(패스트 캠퍼스) - Boosting 1
excerpt: Boosting - GradientBoost
mathjax: true
tags:
  - [AI, ML, Scikit-learn, Regression, Ensemble, 앙상블, Boosting, GradientBoost]
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

# Ensemble - Boosting

---

# 부스팅 (Boosting)

- 약한 학습기를 순차적으로 학습을 하되, 이전 학습에 대하여 잘못 예측된 데이터에 가중치를 부여해 오차를 보완해 나가는 방식
- 장점 : 성능이 매우 우수함 (Lgbm, XGBoost)
- 단점 : 알고리즘의 특성상 계속 약점(오분류/잔차)을 보완하려고 하기 때무에 잘못된 레이블링이나 아웃라이어에 필요 이상으로 민감할 수 있으며, 다른 앙상블 대비 학습 시간이 오래걸림
- 대표적인 Boosting 앙상블
  1. AdaBoost
  2. GradientBoost
  3. LightGBM (LGBM)
  4. XGBoost

---

![Image](https://keras.io/img/graph-kaggle-1.jpeg)

---

### 그라디언트 부스트 (Gradient Boost)
- 성능이 우수함
- 학습 시간이 매우 느림

#### 주요 Hyperparameter
- random_state : 랜덤 시드 고정 값, 고정해두고 튜닝할 것
- n_jobs : CPU 사용 개수
- learning_rate : 학습률. 너무 큰 학습률은 성능을 떨어뜨리고 너무 작은 학습률은 학습이 느리므로 적절한 값을 찾아야 함. n_estimator와 같이 튜닝하며 default=0.1
- n_estimator : 부스팅 스테이지 수(랜덤포레스트의 트리 개수 설정과 비슷한 개념). default=100
- subsample : 샘플 사용 비율(max_fetures와 비슷한 개념). 과대적합 방지용
- min_samples_split : 노드 분할 시 최소 샘플의 개수. default=2. 과대적합 방지용

---

``` python
from sklearn.ensemble import GradientBoostingRegressor

gbr = GradientBoostingRegressor(random_state=42, learning_rate=0.01, n_estimators=1000, subsample=0.8)
gbr.fit(x_train, y_train)
gbr_pred = gbr.predict(x_test)
```
