---
layout: article
title: AI/ML(패스트 캠퍼스) - Scikit-learn 개요
excerpt: 간단한 Scikit-learn 사용 방법 - 학습과 예측
mathjax: true
tags:
- [AI, ML, Scikit-learn]
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

# Scikit-learn
- 모델 생성 : 사용하고자 하는 모델 import 후 아래아 같이 코딩
  model = 모델명
- 학습 : model.fit(x, y)
- 예측 : prediction = model.predict(x2)
- 예제 :

``` python
from sklearn.linear_model import LinearRegression

model = LinearRegression()
model.fit(x, y)
prediction = model.predict(x2)
```
실습 :

``` python
import numpy as np
from sklearn.linear_model import LinearRegression

x = np.arange(10).reshape(-1, 1)
y = (2*x + 1).reshape(-1, 1)

# 모델 선언
model = LinearRegression()

# 학습
model.fit(x, y)

# 예측
prediction = model.predict([[10.0]])
prediction

''' 출력
array([[21.]])
'''
```

