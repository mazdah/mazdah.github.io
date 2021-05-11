---
layout: article
title: Swift - Assertion & Preconditions 2
excerpt: 예제로 보는 Assertion과 Preconditions
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

# Assertion & Preconditions2

---

- Assertion 예제

``` swift
let age = -3
assert(age >= 0, "A person's age can't be less than zero.")
// This assertion fails because -3 is not >= 0.
```

- 위 코드에서 조건이 참일 경우는 이후 코드가 실행이 되지만 현재 age가 -3으로 false가 되어 메시지를 출력한 후 멈춘다.
- 단순한 조건이 반복되는 경우 아래와 같이 메시지를 생략할 수도 있다.

``` swift
assert(age >= 0)
```

- 이미 조건에 대한 확인 끝난 경우 실패에 대한 명확한 표현을 위해 assertionFailuer함수를 사용한다.

``` swift
// assertionFailure(_:file:line:)

if age > 10 {
    print("You can ride the roller-coaster or the ferris wheel.")
} else if age >= 0 {
    print("You can ride the ferris wheel.")
} else {
    assertionFailure("A person's age can't be less than zero.")
}
```

- Preconditions는 조건이 거짓일 가능성이 있지만 반드시 유효한 값을 넘겨 다음 코드가 진행되도록 하고자 할 때 사용할 수 있다.
- 아래와 예제와 같이 조건문과 함께 사용하며, 거짓일 경우에는 메시지를 출력하고 코드 실행을 중지한다.

``` swift
// In the implementation of a subscript…
// precondition(_:_:file:line:)
precondition(index > 0, “Index must be greater than zero.”)’
```

- 오류가 발생했음을 명확히 하기 위해 `preconditionFailure(_:file:line:)`를 사용할 수 있다(switch문의 default 케이스같은 경우).
- 초기 개발 단계에서는 `fatalError(_:file:line:)`  함수를 사용하여 치명적인 에러 발생 시 앱을 종료시켜 확인할 수 있다.
