---
layout: article
title: AI/ML(패스트 캠퍼스) - Stacking과 Blending 그리고 정리
excerpt: 모델이 아닌 기법으로써의 Ensemble
mathjax: true
tags:
  - [AI, ML, Scikit-learn, Regression, Ensemble, 앙상블, Stacking, Blending, Weighted Blending]
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

# Ensemble - Stacking과 Blending 기법

---

### 스태킹 (Stacking)
- 개별 모델이 예측한 데이터를 기반으로 final_estimator가 종합하여 예측을 수행
- 성능을 극대화 할 때 활용
- 과대적합을 유발할 수 있음(특히 데이터 세트가 적은 경우)

---

``` python
from sklearn.ensemble import StackingRegressor

stack_models = [
                ('elasticnet', poly_pipeline),
                ('randomforest', rfr),
                ('gbr', gbr),
                ('lgbm', lgbm)
]

stack_reg = StackingRegressor(stack_models, final_estimator=xgb, n_jobs=-1)
stack_reg.fit(x_train, y_train)
stack_pred = stack_reg.predict(x_test)
```

---

### 블렌딩 (Weighted Blending)
- 각 모델의 예측 값에 대하여 weight를 곱하여 최종 output 계산
- 모델에 대한 가중치를 조절하여 최종 output을 산출한다.
- 가중치의 합은 1.0이 되도록 한다.
- 주로 성능이 좋은 모델에 대해 가중치를 높게 준다.

---

``` python
final_outputs = {
    'elasticnet' : poly_pred,
    'randomforest' : rfr_pred,
    'gbr' : gbr_pred,
    'xgb' : xgb_pred,
    'lgbm' : lgbm_pred,
    'stacking' : stack_pred
}

final_prediction = \
final_outputs['elasticnet'] * 0.1\
+ final_outputs['randomforest'] * 0.1\
+ final_outputs['gbr'] * 0.2\
+ final_outputs['xgb'] * 0.2\
+ final_outputs['lgbm'] * 0.2\
+ final_outputs['stacking'] * 0.2\
```

---

### Ensemble 정리

1. 앙상블은 대체적으로 단일 모델 대비 성능이 좋다.
2. 앙상블 모델 외에 앙상블 기법인 Stacking과 Blending도 성능 향상에 효과가 있다.
3. 앙상블 모델은 적절한 Hyperparameter 튜닝이 중요하다.
4. 앙상블 모델은 대체적으로 학습 시간이 오래걸린다.
5. 모델을 튜닝하는데 걸리는 시간이 길다.
