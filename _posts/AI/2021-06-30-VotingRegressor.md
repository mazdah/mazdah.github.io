---
layout: article
title: AI/ML(패스트 캠퍼스) - VotingRegressor와 VotingClassifier
excerpt: VotingRegressor 사용 예제와 VotingClassifier의 두 종류
mathjax: true
tags:
  - [AI, ML, Scikit-learn, Regression, Ensemble, 앙상블, VotingRegressor, VotingClassifier]
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

### 보팅(Voting) - 회귀(Regression)

- 투표를 통해 결정하는 방식
- 배깅(Bagging)과의 차이점 : 보팅은 다른 모델, 배깅은 같은 모델 다른 샘플 조합을 사용

---

``` python
from sklearn.ensemble import VotingRegressor, VotingClassifier

# 사용할 모델들을 이름, 모델 쌍의 튜플 형태의 배열로 만든다.
single_models = [
    ('linear_reg', model),
    ('ridge', ridge),
    ('lasso', lasso),
    ('elasticnet_pipeline', elastic_no_pipeline),
    ('poly_pipeline', poly_pipeline)
]

voting_regressor = VotingRegressor(single_models, n_jobs=-1)
voting_regressor.fit(x_train, y_train)
voting_pred = voting_regressor.predict(x_test)
```

---

### Voting Classifire에서의 중요한 파라미터

- voting = {‘hard’, ‘soft’}
- 사용 예 : VotingClassifier(voting=‘soft’)

---

### hard voting
- 단순히 가장 많은 표를 얻은 결과를 선택

### soft voting
- 사용되는 모든 모델이 확률 예측이 가능한 경우 사용됨
- 각 모델에서 예측한 확률을 평균하여 확률이 높은쪽을 선택
