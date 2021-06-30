---
layout: article
title: "Spring - bean Life Cycle"
excerpt: bean의 life cycle 개요
mathjax: true
tags:
  - [Spring, IoC, DI, Application Context, bean, life cycle]
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

# bean의 라이프 사이클 관리

---

일반적으로 스프링은 다음 2가지의 라이프 사이클 이벤트가 있다.

- 초기화 이후(post-initialization)
  스프링이 bean에 모든 프로퍼티 값을 설정하고 의존성 점검을 마치자 마자 발생

- 소멸 이전(pre-destruction)
  스프링이 bean 인스턴스를 소멸시키기 바로 직전에 발생

> 프로토타입 bean(호출할 때마다 새로운 인스턴스가 생성되는)에는 소멸 전 이벤트를 통지하지 않는다.

스프링은 인터페이스 기반, 메서드 기반, 애노테이션 기반의 3가지 방법으로 bean에게 이벤트를 전달한다.

- 인터페이스 기반
  수신을 원하는 통지 유형에 해당하는 인터페이스를 bean에 구현
  라이프 사이클 이벤트를 받아야 하는 동일 타입이 bean이 다수 필요하거나 여러 개의 스프링 기반 프로젝트에서
  재활용하기 위해서 사용

- 메서드 기반
  bean이 초기화 될 때와 소멸될 때 호출할 메서드 이름을 ApplicationContext 구성에 지정
  이식성이 중요한 상황이거나 특정 종류의 bean 한두 개만을 정의하는 경우

- 애노테이션 기반
  bean 초기화 후나 소멸 전에  호출될 메서드에 애노테이션을 지정
  사용 중인 IoC 컨테이너가 JSR-250을 지원하는 경우

> 스프링에 특화된 인터페이스를 작성할 필요가 없으므로 메서드 기반이나 애노테이션 기반의 방식을 사용하는
> 것이 더 바람직하다.

> 라이프 사이클 통지가 필요한 동일 타입의 빈을 대량으로 정의하는 경우에는 인터페이스 기반의 방식을 사용하는
> 것이 더 효율적이다.

![Image](https://raw.githubusercontent.com/mazdah/mazdah.github.io/master/_posts/Spring/images/bean-lifecycle.png)
