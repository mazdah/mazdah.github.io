---
layout: article
title: "Spring - 자동와이어링(Autowiring) 3"
excerpt: 자동와이어링은 특별한 경우가 아니면 사용하지 않는 것이 좋다!
mathjax: true
tags:
  - [Spring, IoC, DI, Application Context, 자동와이어링, autowiring]
categories: Spring
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
    src: /spring.jpg
---

# bean 자동와이어링 하기 3
- - - -

### 자동와이어링의 장점

- 소규모 애플리케이션에서 시간을 절약할 수 있음

### 자동와이어링의 단점

- 대규모 애플리케이션에서 유연성이 떨어짐
- byName을 이용하는 경우 클래스에 인위적으로 프로퍼티 이름을 지정하게 되어 종속성이 생긴다.
- byType을 이용하는 경우 서로 다른 구성으로 동일한 타입의 빈을 유지할 때 문제가 된다.

### 결론

중요한 애플리케이션이라면 자동와이어링을 사용하지 말아야 한다.
