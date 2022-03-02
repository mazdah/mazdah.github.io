---
layout: article
title: Swift - Enumeration 4
excerpt: Swift의 Enumeration 정리 4 - Raw Values
mathjax: true
tags:
- [Swift, Enumeration, switch, Raw Values]
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

# Enumeration #4
- - - -
### 원시값 (Raw Values)

- 연관값 대신 enumeration 케이스 모두에 같은 타입이 기본값을 미리 할당할 수 있다.

```swift
enum ASCIIControlCharacter: Character {
	case tab = "\t"
	case lineFeed = "\n"
	case carriageReturn = "\r"
}
```

- ASCIIControlCharacter는 Character 타입으로 원시값이 설정되어 있다.
- 원시값은 Character 뿐만 아니라 문자열, 정수, 부동소숫점 숫자 타입도 가능하다.
- 각 케이스의 원시 값은 enumeration 안에서 유일한 값이어야 한다.

> 원시값은 enumeration 처음 생성시 설정하는 값으로 항상 동일한 값으로 유지되나 연관값은 각 케이스에 타입만을 지정하는 것으로 상수나 변수를 생성할 때 값이 설정되며 그 값은 달라질 수 있다.

#### 암시적으로 할당된 원시값 (Implicitly Assigned Raw Values)

- 원시값의 타입이 정수 또는 문자열인 경우 enumeration의 각 케이스에 명시적으로 원시값을 할당할 필요가 없다(Swift가 자동으로 할당).
- 예를 들어 원시값의 타입이 정수형인 경우 각 케이스는 순서대로 이전 케이스보다 1이 증가된 값을 가지며, 만일 첫 번째 케이스에 값이 지정되지 않은 경우 0으로 설정한다.

```swift
enum Planet: Int {
	case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

- 위의 예에서 Planet.mercury는 명시적으로 1을 설정해주었고, 따라서 Planet.venus는 암시적으로 1이 증가한 2의 값을 갖는다.

```swift
enum CompassPoint: String {
	case north, south, east, west
}
```

- 위 예제의 경우 원시값의 타입이 문자열이고 명시적으로 설정된 값이 없다. 이런 경우 각 케이스는 암시적으로 자신의 이름을 원시값으로 갖는다.
- CompassPoint.south의 원시값은 “south”이다.
- 원시값에 접근하기 위해서는 rawValue라는 속성을 사용한다.

```swift
let earthsOrder = Planet.earth.rawValue
// earthsOrder는 3의 값을 갖는다.

let southDirection = CompassPoint.west.rawValue
// southDirection는 "west" 값을 갖는다.
```


#### 원시값으로 초기화 (Initializing from a Raw Value)

- rawValue라는 키워드를 이용하여 enumeration을 초기화 하면 파라미터 값에 해당하는 케이스 또는 해당 케이스가 없는 경우 nil을 반환한다.

```swift
let possiblePlanet = Planet(rawValue: 7)
// possiblePlanet은 Planet? 타입이고 Planet.uranus의 값을 갖는다.
```

> 원시값 초기화는 파라미터에 해당하는 케이스가 없을 수 있기 때문에 초기화에 실패할 수 있다.

- 위 예제에서 만일 11 값을 파라미터로 사용하는 경우 초기화 시 반환되는 옵셔널 Planet의 값은 nil이다.

```swift
let positionToFind = 11
if let somePlanet = Planet(rawValue: positionToFind) {
	switch somePlanet {
	case .earth:
		print("Mostly harmless")
	default:
		print("Not a safe place for humans")
	}
} else {
	print("There isn't a planet at position \(positionToFind)")
}

// "There isn't a planet at position 11"이 출력된다.
```
