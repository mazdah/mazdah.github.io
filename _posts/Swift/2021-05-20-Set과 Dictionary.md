---
layout: article
title: Swift - Collection type (Set과 Dictionary)
excerpt: Collection type의 Set과 Dictionary에 대하여
mathjax: true
tags:
 - [Swift, Collection, Set, Dictionary]
categories: Swift
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
    src: /swift.jpg
---

### Sets

- Set은 같은 type의 중복되지 않은 값을 정렬되지 않은 상태로 저장한다.
- Set에 저장될 수 있는 type은 hash값을 가질 수 있는 타입이며, Swift의 기본 type들은 모두 hash 값을 가질 수 있다.
- 배열과 마찬가지로 이미 생성된 set을 비우더라도 초기화 시 지정된 type은 그대로 유지된다.
- 기본적으로 정렬되어있지 않으나 sorted() 함수를 이용하여 정렬할 수 있다.
- 두 set 간에 다음과 같은 연산 수행이 가능하다.

![img](https://raw.githubusercontent.com/mazdah/mazdah.github.io/master/_posts/Swift/images/Set1.png)


---

### Dictionary

- 같은 type의 키들과 같은 type의 값들을 정렬되지 않은 상태로 저장한다.
- 키들은 고유한 값이어야 한다.
- 키는 hash값을 가질 수 있는 type이어야 한다.
- 배열이나 Set과 마찬가지로 특정 type의 key-value로 생성된 Dictionary는 그 값을 비우더라도 type은 유지된다.
- updateValue(_:forKey:) 함수는 만일 해당하는 키-값이 존재하면 해당 키의 값을 업데이트하고 존재하지 않으면 새로 키-값을 저장한다. 리턴 값은 해당 키의 이전 값이거나 해당 키가 없는 경우 nil을 반환하며 optional 값을 반환한다.
- for-in 루프에 사용할 경우 (키, 값)의 튜플을 리턴한다.
