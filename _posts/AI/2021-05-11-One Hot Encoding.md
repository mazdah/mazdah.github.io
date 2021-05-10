---
layout: article
title: AI/ML(패스트 캠퍼스) - One Hot Encoding
mathjax: true
tags:
  - [AI, ML, Scikit-learn, One Hot Encoding]
categories: AI
comments: true
---

---
# 전처리 기본 - One Hot Encoding

- 데이터 내에서 불필요한 관계를 배제하기 위해 One Hot Encoding을 사용한다.
- 예를들어 Embarked 컬럼을 수치형으로 바꾸어 S = 2, C = 0, Q = 1로 변환된 경우 Q + Q = S로 학습하는 경우가 발생하는데 이런 상황을 배제해야 한다.
- 카테고리형 데이터에 주로 사용된다.

---


``` python
# 타이타닉 데이터
train['Embarked'].value_counts()
'''출력
S    646
C    168
Q     77
Name: Embarked, dtype: int64
'''

train['Embarked_num'] = le.fit_transform(train['Embarked'])
train['Embarked_num'].value_counts()
'''출력
2    646
0    168
1     77
Name: Embarked_num, dtype: int64
'''

train['Embarked'][:6]
'''출력
0    S
1    C
2    S
3    S
4    S
5    Q
Name: Embarked, dtype: object
'''

train['Embarked_num'][:6]
'''출력
0    2
1    0
2    2
3    2
4    2
5    1
Name: Embarked_num, dtype: int64
'''

# Pandas의 get_dummies 함수를 이용하여 One Hot Encoding 처리
pd.get_dummies(train['Embarked_num'][:6])
'''출력
  0  1  2
0 0  0  1
1 1  0  0
2 0  0  1
3 0  0  1
4 0  0  1
5 0  1  0
'''

one_hot = pd.get_dummies(train['Embarked_num'][:6])
one_hot.columns = ['C', 'Q', 'S']
one_hot
'''출력
  C  Q  S
0 0  0  1
1 1  0  0
2 0  0  1
3 0  0  1
4 0  0  1
5 0  1  0
'''
```
