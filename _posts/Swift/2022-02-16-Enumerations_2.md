---
layout: article
title: Swift - Enumeration 2
excerpt: Swift의 Enumeration 정리 2 - switch문에서의 사용과 CaseIterable
mathjax: true
tags:
- [Swift, Enumeration, switch, CaseIterable]
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

#  Enumeration #2

---

### Switch뮨애서 Enumeration 값 매칭하기

- 각각의 enumeration 값을 switch 문에서 매치시킬 수 있다.

```swift
directionToHead = .south
switch directionToHead {
	case .north:
		print("Lots of planets have a north")
	case .south:
		print("Watch out for penguins")
	case .east
		print("Where the sun rises")
	case .west
		print("Where the skies are blue")
}

// "Watch out for penguins" 출력
```

- 제어 흐름에 설명된 바와 같이 switch문은 enumeration의 모든 case들을 사용해야 한다.
- 예를들어 위 코드에서 case .west를 생략하면 컴파일 오류가 발생한다.
- 모든 enumeration case를 제공하는 것이 적절하지 않다면 생략되는 case 처리를 위해 default 문을 반드시 사용해야 한다.

```swift
let somePlanet = Planet.earth
switch somePlanet {
	case .earth:
		print("Mostly harmless")
	default:
		print("Not a safe place for human")
}

// "Mostly harmless" 출력
```


### Enumeration Case들 반복시키기

- 때로는 Enumeration의 모든 case들을 컬렉션으로 만드는 것이 유용하다.
- enumeration 이름 뒤에 **CaseIterable**을 써서 이렇게 만들 수 있다.
- swift는 allCases 속성을 통해 모든 case를 컬렉션 형태로 노출시킨다.
- allCases 속성을 통해 얻은 컬렉션은 다른 컬렉션과 동일하게 사용할 수 있으며 각 요소는 enumeration 타입의 값이다.

```swift
enum Beverage: CaseIterable {
	case coffee, tea, juice
}

let numberOfChoices = Beverage.allCases.count
print("\(numberOfChoices) beverages available")
// "3 beverages available" 출력

for beverage in Beverage.allCases {
	print(beverage)
}
// 순서대로 coffee, tea, juice 출력
```
