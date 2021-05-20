---
layout: article
title: AI/ML(패스트 캠퍼스) - SVM과 Decision Tree
excerpt: 서포트 벡터 머신(SVM)과 의사 결정 트리(Decision Tree)에 대한 개요
mathjax: true
tags:
  - [AI, ML, Scikit-learn, Iris, SVM, Support Vector Machine, Decision Tree]
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

# Scikit-learn : 분류 - 서포트 벡터 머신 (SVM)

- 새로운 데이터가 어느 카테고리에 속할지 판단하는 비확률적인 ****이진 선형 분류**** 모델을 만든다.
- 경계로 표현되는 데이터들 중 ****가장 큰 폭을 가진 경계****를 찾는 알고리즘
- LogisticRegression과 같이 이진분류만 가능. OvsR 전략 사용
- decision_function()을 통해 각 클래스별 확률값을 확인할 수 있다.

---

![img](https://csstudy.files.wordpress.com/2011/03/screen-shot-2011-02-28-at-5-53-26-pm.png)

``` python
from sklearn.svm import SVC

svc = SVC()
svc.fit(x_train, y_train)

'''출력
SVC(C=1.0, break_ties=False, cache_size=200, class_weight=None, coef0=0.0,
    decision_function_shape='ovr', degree=3, gamma='scale', kernel='rbf',
    max_iter=-1, probability=False, random_state=None, shrinking=True,
    tol=0.001, verbose=False)
'''

svc_pred = svc.predict(x_valid)
(svc_pred == y_valid).mean()

'''출력
0.9736842105263158
'''

svc_pred[:5]

'''출력
array([1, 0, 0, 2, 2])
'''

svc.decision_function(x_valid)[:5]

'''출력
array([[-0.21848985,  2.22024458,  0.98563486],
       [ 2.23664164,  1.11287254, -0.24914482],
       [ 2.23353772,  1.13999373, -0.25131722],
       [-0.23436596,  1.08672695,  2.22282749],
       [-0.2159257 ,  0.84120058,  2.24441978]])
'''
```

---

# Scikit-learn : 분류 - 결정 트리(Decision Tree)

- tree 형태의 구조에서 가지치기를 통해 소그룹으로 나누어 판별하는 것

---

![img](https://www.researchgate.net/profile/Ludmila-Aleksejeva-2/publication/293194222/figure/fig1/AS:669028842487827@1536520314657/Decision-tree-for-Iris-dataset.png)

``` python
from sklearn.tree import DecisionTreeClassifier

dtc = DecisionTreeClassifier()
dtc.fit(x_train, y_train)

'''출력
DecisionTreeClassifier(ccp_alpha=0.0, class_weight=None, criterion='gini',
                       max_depth=None, max_features=None, max_leaf_nodes=None,
                       min_impurity_decrease=0.0, min_impurity_split=None,
                       min_samples_leaf=1, min_samples_split=2,
                       min_weight_fraction_leaf=0.0, presort='deprecated',
                       random_state=None, splitter='best')
'''

dtc_pred = dtc.predict(x_valid)
(dtc_pred == y_valid).mean()

'''출력
0.9736842105263158
'''

dtc = DecisionTreeClassifier(random_state=0)
dtc.fit(x_train, y_train)
dtc_pred = dtc.predict(x_valid)
(dtc_pred == y_valid).mean()

'''출력
0.8947368421052632
'''
```
