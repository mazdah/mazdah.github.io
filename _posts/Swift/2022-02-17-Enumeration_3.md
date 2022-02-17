---
layout: article
title: Swift - Enumeration 3
excerpt: Swift의 Enumeration 정리 3 - Associated Values
mathjax: true
tags:
- [Swift, Enumeration, switch, Associated Values]
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

# Enumeration #3

---

### Associated Values

- Enumeration의 케이스 옆에 이 케이스와는 다른 타입의 값을 저장하는 것이 유용할 때가 있다.
- 이렇게 저장되는 값을 *associated value*라고 하며 코드에서 값으로 사용할 때마다 바꿀 수 있다.
- Swift의 Enumeration을  주어진 타입의 associated value들을 저장하기 위한 용도로 선언할 수 있다.
- associated value들은 각 enumeration 케이스마다 서로 다를 수 있다.
- 아래 예제는 1차원 바코드와 QR코드 형식의 2차원 바코드의 서로 다른 형식을 저장하는 enumeration이다.

```swift
enum Barcode {
	case upc(Int, Int, Int, Int)
	case qrCode(String)
}
```

- 이 enumeration이 실제로 Int나 String 타입의 값을 제공해주지는 않는다.
- 다만 associated value의 타입만을 지정한 것으로 상수나 변수가 Barcode.upc나 Barcode.qrCode와 같은 경우에 값을 저장할 수 있을 뿐이다.

```swift
var productBarcode = Barcode.upc(8, 85909, 51226, 3)
```

- 위 예제는 productBarcode 변수에 associated value로 튜플 값인 (8, 85909, 51226, 3)를 가지고 있는 Barcode.upc를 할당한 것이다.
- 같은 상품의 다른 바코드를 할당할 수도 있다.

```swift
productBarcode = .qrCode("ABCDEFGHIJKLMLOP")
```

- 이렇게 새롭게 할당되는 순간 Barcode.upc와 연관된 integer 값들은 새로운 Barcode.qrCode와 문자열 값으로 대체된다.
- associated value가 붙은 enumeration도 switch문에 사용 가능하며 switch문에 사용할 경우 associated value의 값을 상수로 추출하여 사용할 수 있다.

```swift
switch productBarcode {
	case .upc(let numberSystem, let manufacturer, let product, let check):
		print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
	case .qrCode(let productCode):
		print("QR code: \(productCode).")
}

// "ABCDEFGHIJKLMLOP." 출력
```

- 일단 모든 associated value가 상수나 변수로 추출되고 나면 이후에는 간단하게 var나 let 키워드를 한번만 사용할 수 있다.


```swift
switch productBarcode {
	case let .upc(numberSystem, manufacturer, product, check):
		print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
	case let .qrCode(productCode):
		print("QR code: \(productCode).")
}

// "ABCDEFGHIJKLMLOP." 출력
```
