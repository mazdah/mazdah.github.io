---
layout: article
title: AI/ML(패스트 캠퍼스) - LabelEncoding
mathjax: true
tags:
  - [AI, ML, Scikit-learn, LabelEncoding]
categories: AI
comments: true
---

---
# 전처리의 기본 - Label Encoder

- 문자(categorical) 데이터를 수치(numerical) 데이터로 변환

- NaN 값이 포함되어있으면 정상 동작하지 않음

---

``` python
# 타이타닉 데이터
train['Sex'].value_counts()
'''출력
male      577
female    314
Name: Sex, dtype: int64
'''

def convert(data):
    if data == 'male':
        return 1
    elif data == 'female':
        return 0

train['Sex'].apply(convert)
'''출력
0      1
1      0
2      0
3      0
4      1
      ..
886    1
887    0
888    0
889    1
890    1
Name: Sex, Length: 891, dtype: int64
'''

from sklearn.preprocessing import LabelEncoder

le = LabelEncoder()
train['Sex_num'] = le.fit_transform(train['Sex'])
train['Sex_num'].value_counts()
'''출력
1    577
0    314
Name: Sex_num, dtype: int64
'''

le.classes_
'''출력
array(['female', 'male'], dtype=object)
'''

# inverse_transform을 통해 다시 카테고리형으로 변환 가능
le.inverse_transform([0, 1, 1, 0, 0, 1, 1])
'''출력
array(['female', 'male', 'male', 'female', 'female', 'male', 'male'],
      dtype=object)
'''

# NaN 값이 포함된 컬럼 처리, Embarked에는 NaN 값이 포함되어 아래 문장은 오류 발생
le.fit_transform(train['Embarked'])

# 결측치 처리 후 정상 변환
train['Embarked'] = train['Embarked'].fillna('S')
le.fit_transform(train['Embarked'])

```
