---
layout: article
title: Swift - Swift에서의 throw 처리
mathjax: true
tags:
- [Swift, throw, try, catch]
categories: Swift
comments: true
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

