---
layout: article
title: Swift - Enumeration 1
excerpt: Swift의 Enumeration 정리 1 - 개요
mathjax: true
tags:
- [Swift, Enumeration, case]
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

# Enumeration

---

- 관션성 있는 값들을 하나의 공통 타입으로 만들어 코드에서 type-safe하게 사용할 수 있도록 만든 것
- C에서의 enumeration보다 유연하며, 각 케이스를 위한 값을 제공할 필요가 없다.
- 만일 enumeration의 각 케이스에 대한 값(raw 값)을 제공할 경우에는 문자열, 단일 문자, 정수형, 실수형만을 사용해야 한다.
- 각 케이스들에 특별히 연관된 어떠한 타입의 값이라도 각각의 서로 다른 케이스에 사용할 수 있다.
- Enumerations는 1급 클래스 타입으로 computed 속성, 인스턴스 메서드 사용이 가능하며 값의 초기화를 위한 initializer 정의, 기능의 확장, 표준 기능 제공을 위한 프로토콜 준수 등을 구현할 수 있다.


### Enumeration 문법

- enum 키워드를 사용하며 중괄호로 묶는다.

```swift
enum SomeEnumeration {
	// case를 정의한다.
}
```


- 나침반의 방향 표시를 위한 예

```swift
enum CompassPoint {
	case north
	case south
	case east
	case west
}
```

- enumeration 안에 정의된 값들을 *enumeration cases*라고 한다.
- 새로운 케이스를 만들 때는 case 키워드를 사용한다.

> Swift의 enumeration은 기본 값으로 정수를 설정하지 않아도 된다. 위 예에서 각 case들은 정수 0~3을
> 나타내는 것이 아니고 그 자체로 CompassPoint라는 타입의 서로 다른 값들이다.

- 여러 개의 케이스드들은 ,로 구분하여 한 줄에 코딩할 수 있다.

```swift
enum Planet {
	case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

- 각각의 enumeration은 Swift의 다른 타입들과 마찬가지로 새로운 타입들이며, 그 이름은 대문자로 시작해야 한다.
- 이름은 단수형으로 작성하여 보다 명료하게 표현한다.

```swift
var directionToHead = CompassPoint.west
```

- directionToHead 변수는 CompassPoint의 값으로 초기화 될 때 타입이 추론된다.
- 한번 초기화된 변수는 같은 enumeration 타입의 다른 값을 할당할 때 축약된 dot(.) 문법을 사용할 수 있다.

```swift
var directionToHead = .east
```
