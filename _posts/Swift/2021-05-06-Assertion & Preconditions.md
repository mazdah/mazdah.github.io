---
layout: article
title: Swift - Assertion & Preconditions
excerpt: Assertion과 Preconditions의 간단한 특징
mathjax: true
tags:
- [Swift, Assertion, Preconditions]
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

# Assertion & Preconditions
- Assertion과 Preconditions는 런타임 시 발생하는 상황을 체크한다.
- 코드가 진행되기 전에 필수적인 조건이 안전한지 검사하기 위해 사용된다.
- true를 반환하면 코드가 계속 진행되고 false가 반환되면 코드는 멈추고 앱이 종료된다.
- Assertion은 개벌 중 잘못된 예측을 잡아내는데, Preconditions은 제품화된 앱에서 문제를 감지하는데 사용할 수 있다.
- try~catch와는 달리 복구 가능하거나 예측 가능한 에러를 처리하는데는 사용할 수 없다.
- Assertion과 Preconditions는 유효하지 않은 상태가 예기치 않게 발생하는 상황을 대처하기 위해 사용하면 안된다.
- 하지만 유효한 데이터나 상태를 강제하여 앱이 조금 더 예측 가능한 상태로 종료되게 하거나 디버깅하기 쉽게 도와준다.
- Asserton은 디버깅 시만 사용 가능하나 Preconditions는 디버깅과 프로덕션에서 모두 사용 가능하다.
- 프로덕션에서는 Assertion 내의 조건이 전혀 계산되지 않기 때문에 Assertion을 얼마든지 사용해도 프로덕션의 성능에 영향을 주지 않는다.
