---
layout: article
title: Swift - Enumeration 5
excerpt: Swift의 Enumeration 정리 5 - Recursive Enumerations
mathjax: true
tags:
- [Swift, Enumeration, switch, Recursive Enumerations]
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

# Enumeration #5

---

### 재귀 열거형 (Recursive Enumerations)

- 열거형의 케이스에 하나 이상의 연관 값으로 다른 열거형 인스턴스를 가지고 있는 경우
- indirect 키워드를 사용하여 지정한다.

```swift
enum ArithmaticExpression {
	case number (Int)
	indirect case addition(ArithmaticExpression, ArithmaticExpression)
	indirect case multiplication(ArithmaticExpression, ArithmaticExpression)
}
```

- 열거형 앞에 indirect를 사용하면 모든 케이스를 indirect로 만든다.

```swift
indirect enum ArithmaticExpression {
	case number (Int)
	case addition(ArithmaticExpression, ArithmaticExpression)
	case multiplication(ArithmaticExpression, ArithmaticExpression)
}
```

- 위 예제는 1 개의 숫자, 덧셈 식, 곱셈 식 3가지의 산술 표현식을 저장한다.
- addition과 multiflication 케이스는 산술 표현식이 연관값이다.
- (5 + 4) * 2와 같은 식을 보면 곱셈의 우항은 하나의 숫자이지만 좌항은 또다른 표현식이다.
- 이와 같은 표현을 위해 재귀 열거형이 필요하다.

```swift
let five = ArithmaticExpression.number(5)
let four = ArithmaticExpression.number(4)
let sum = ArithmaticExpression.addition(five, four)
let product = ArithmaticExpression.multiplication(sum, ArithmaticExpression.number(2))
```

- 재귀 함수를 이용한 응용 예제

```swift
func evaluate(_ expression: ArithmaticExpression) -> Int {
	// value, left, right는 열거형 case의 연관값을 받아오기위한 상수를 나타내는 것으로
  // 다른 이름으로 바꾸어도 된다.
	switch expression {
	case let .number(value):	// 세 번째 ~ 다섯 번째로 실행
		// 아래 case문의 첫 번째 재귀 호출로 3번으로 실행
		// 아래 case문의 두 번째 재귀 호출로 4번으로 실행됨
		// 마지막 case문의 두 번째 재귀 호출로 마지막 5번으로 실행됨
		return value
	case let .addition(left, right):	// 두 번째로 실행
		// 아래 케이스의 첫 번째 재귀 호출로 2번으로 실행됨
		// left는 number(5) 이므로 재귀 호출 시 바로 위의 case문 실행됨
		// right는 number(4) 이므로 재귀 호출 시 바로 위의 case문 실행됨
		return evaluate(left) + evaluate(right)
	case let .multiplication(left, right):  //	첫 번째로 실행
		// product가 multiplication이므로 1번으로 실행
		// left는 addition이므로 바로 위의 케이스문이 실행됨
		// right는 number(2) 이므로 재귀 호출 시 첫 번째 case문 실행됨
		return evaluate(left) * evaluate(right)
	}
}

print(evaluate(product))
// "18" 출력
```
