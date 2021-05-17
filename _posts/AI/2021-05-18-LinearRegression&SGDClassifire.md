---
layout: article
title: AI/ML(패스트 캠퍼스) - Logistic Regression & SGDClassifire
excerpt: Logistic Regression과 SGD (확률적 경사 하강법)의 개요
mathjax: true
tags:
  - [AI, ML, Scikit-learn, Iris, Logistic Regression, SGD, SGDClassifire]
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

# Scikit-learn : 분류 - Logistic Regression (로지스틱 회귀)

- 독립 변수의 선형 결합을 이용하여 사건의 발생 가능성을 예측하는데 사용되는 통계 기법
- 이진 분류만 가능
- 3개 이상의 클래스를 분류하는 경우 one-vs-rest 또는 one-vs-one 전략을 사용

  . one-vs-rest(OvR) : K 개의 클래스가 존재할 때 1개의 클래스를 제외한 다른 클래스를 K개 만들어 각각의 이진 분류에 대한 확률을 구하고 총합을 통해 최종 클래스를 판별

  . one-vs-one(OvO) : 4개의 계절을 구분하는 클래스가 존재한다고 가정했을 때 Ovs1, Ovs2, Ovs3,…, 2vs3까지 NX(N-1)/2 개의 분류기를 만들어 가장 많이 양성으로 선택된 클래스를 판별
- OvsR을 선호함

### 모델 선언, 학습(fit), 예측(predict) 프로세스

---

``` python
from sklearn.linear_model import LogisticRegression

# 모델 선언
lr = LogisticRegression()

# 학습
lr.fit(x_train, y_train)

'''출력
LogisticRegression(C=1.0, class_weight=None, dual=False, fit_intercept=True,
                   intercept_scaling=1, l1_ratio=None, max_iter=100,
                   multi_class='auto', n_jobs=None, penalty='l2',
                   random_state=None, solver='lbfgs', tol=0.0001, verbose=0,
                   warm_start=False)
'''

# 예측
lr_pred = lr.predict(x_valid)
lr_pred[:5]

'''출력
array([2, 2, 1, 0, 1])
'''

# 평가
(lr_pred == y_valid).mean()

'''출력
0.9736842105263158
'''
```


---

# Scikit-learn : 분류 - stochastic gradient descent (SGD) : 확률적 경사 하강법
- 참고 : [머신러닝 reboot 개념 잡기 : 경사 하강법 1 - 특성의 scale :: 꿈을 위한 단상](https://mazdah.tistory.com/833?category=598657)

---

![img](https://machinelearningnotepad.files.wordpress.com/2018/04/yk1mk.png
)

``` python
from sklearn.linear_model import SGDClassifier

sgdc = SGDClassifier()
sgdc.fit(x_train, y_train)
sgdc_prediction = sgdc.predict(x_valid)
(sgdc_prediction == y_valid).mean()

'''출력
0.9210526315789473
'''
```
