---
layout: article
title: AI/ML(패스트 캠퍼스) - train_test_split의 stratify 옵션
excerpt: 훈련 샘플의 분포를 균등하게 하기 위한 전략
mathjax: true
tags:
  - [AI, ML, Scikit-learn, Iris, train_test_split, stratify]
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

# Scikit-learn : 분류 - 데이터의 불균형(imbalance), 그리고 stratify 옵션

---

- stratify : label의 클래스 분포를 균등하게 배분

``` python
import warnings
import pandas as pd
from sklearn.datasets import load_iris

# Iris 데이터 세트 로드
iris = load_iris()

# 로드된 원본 데이터에서 피쳐 데이터 부분만 추출
data = iris['data']

# 피쳐 이름 부분 추출
feature_names = iris['feature_names']

# label 부분 추출
target = iris['target']

# Iris 데이터 세트 데이터 프레임 만들기
df_iris = pd.DataFrame(data, columns=feature_names)

# 데이터 프레임에 label 합치기
df_iris['target'] = target

# 훈련 데이터와 테스트 데이터 분리
from sklearn.model_selection import train_test_split

x_train, x_valid, y_train, y_valid = train_test_split(df_iris.drop('target', 1), df_iris['target'])
x_train.shape, y_train.shape
'''출력
((112, 4), (112,))
'''

x_valid.shape, y_valid.shape
'''출력
((38, 4), (38,))
'''

# 시각화 해보기
sns.countplot(y_train)
```

![Image](https://raw.githubusercontent.com/mazdah/mazdah.github.io/master/_posts/AI/images/iris4.png)

- 위와 같이 훈련 샘플이 불균등한 경우 모델의 성능이 나빠진다.
- stratify 옵션을 사용하여 클래스간 불균형 보정

``` python
x_train, x_valid, y_train, y_valid = train_test_split(df_iris.drop('target', 1), df_iris['target'], stratify=df_iris['target'])
sns.countplot(y_train)
```

![Image](https://raw.githubusercontent.com/mazdah/mazdah.github.io/master/_posts/AI/images/iris5.png)
