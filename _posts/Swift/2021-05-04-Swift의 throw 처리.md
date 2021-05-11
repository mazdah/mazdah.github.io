---
layout: article
title: Swift - Swift에서의 throw 처리
excerpt: Swift에서의 예외 처리 방법에 대한 간단한 코드
mathjax: true
tags:
- [Swift, throw, try, catch]
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

# Swift에서의 throw 처리
### 함수 선언부

``` swift
func canThrowAnError() throws {
    // this function may or may not throw an error
}
```

### 함수 호출부

```swift
do {
	try canThrowAnError()
	// no error was thrown
} catch {
	// an error was thrown
}
```

