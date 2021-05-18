---
layout: article
title: Swift - Collection type (Array)
excerpt: Collection type의 개요와 Array에 대하여
mathjax: true
tags:
  - [Swift, Collection, Array]
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

# Collection type

---

- array : 같은 type의 값들을 정렬된 형태로 담는 collection
- set : 정렬되지 않고 고유한 값만을 갖는 collction
- dictionary : 정렬되지 않고 key-value 쌍을 갖는 collection
- 이러한 collection type들은 잘못된 type의 값이 들어가지 않도록 type에 명확하다.
- Swift의 collection type들은 모두 generic collections를 구현하고 있다.
- collection type은 생성시에 가변 인스턴스로 생성된다. 하지만 상수에 할당되면 수정할 수 없다.

> collection type을 생성할 때 수정할 일이 없다면 상수로 생성하는 것이 코드를 이해하거나 최적화 하는데 좋다.

- - - -

### Array

- 배열을 비우더라도 초기화 시 참조된 값의  type은 그대로 유지된다.

``` swift
var someInts = [Int]()
someInts.append(3)
someInts = []
// someInts는 여전히 Int type의 값들을 갖는 배열이다.
```


- 초깃값을 가지고 초기화 시키기

``` swift
var threeDoubles = Array(repeating: 0.0, count: 3)
// threeDoubles는 Double 타입의 요소를 갖는 배열이며 0.0이라는 값이 3개 들어있도록 초기화된다.
```

-  `+`  기호를 이용하여 2개의 다른 배열로 부터  새로운 배열을 만들 수 있다. 이때 새로 만들어지는 배열의 값 type은 합쳐진 2개의 배열의 값 type을 따른다.

``` swift
var anotherThreeDoubles = Array(repeating: 2.5, count: 3)
// anotherThreeDoubles는 Double 타입의 요소를 갖는 배열이며 2.5이라는 값이 3개 들어있도록 초기화된다.

var sixDoubles = threeDoubles + anotherThreeDoubles
// sixDoubles는 더해지는 두 배열과 같이 Double 타입의 요소를 갖는 배열이며, 포함되는 값은  [0.0, 0.0, 0.0, 2.5, 2.5, 2.5] 이다.
```

- += 연산자를 이용하여 한 개 이상의 값을 배열에 추가할 수 있다.

``` javascript
var shoppingList = ["Eggs", "Milk"]

shoppingList += ["Baking Powder"]
// shoppingList에는 2개의 값이 추가되어 총 3개의 값이 들어있다.
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList에는 3개의 값이 추가되어 총 6개의 값이 들어있다.
```

- range 연산자를 이용하여 배열의 인덱스에 접근할 수 있다.

``` swift
shoppingList[2...4] = ["Bananas", "Apples"]
// shoppingList의 2, 3, 4 인덱스에 있는 3개의 값을 2개의 값으로 대체하여 총 5개의 값이 남아있다.
```

- 마지막 인덱스의 값을 제거하는 removeLast() 함수가 별도록 존재한다.
- for-in 문을 이용하여 반복 루프를 통해 순차적으로 값을 가져올 수 있으며 enumerated() 함수를 이용하면 (index, value) 튜플을 리턴받을 수 있다.

``` swift
for item in shoppingList {
    print(item)
}
// Eggs
// Milk
// Bananas
// Apples
// Butter

for (index, value) in shoppingList.enumerated() {
    print("Item \(index + 1): \(value)")
}
// Item 1: Eggs
// Item 2: Milk
// Item 3: Bananas
// Item 4: Apples
// Item 5: Butter
```
