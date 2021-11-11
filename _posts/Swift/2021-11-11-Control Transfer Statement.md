---
layout: article
title: Swift - Swift의 제어 이동 문
excerpt: Swift continue, break, fallthrough, labeled statement 정리
mathjax: true
tags:
- [Swift, continue, break, fallthrough, labeled statement]
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

# 제어 이동 문(Control Transfer Statement)

---

### continue

- 타 언어의 continue와 동일
- 반복문에서 continue를 만나면 이후 문장은 실행되지 않고 반복문 처음으로 돌아가 다음 반복을 수행

### break

- 타 언어의 break와 동일
- 반복문이나 switch문 내에 사용되며 break를 만나는 즉시 해당 반복문이나 switch문은 종료되고 다음 실행문으로 넘어간다.

### **fallthrough**

- swift의 switdh문은 break가 없어도 자동으로 다음 case로 넘어가지 않는다.
- 명시적으로 fallthrough를 사용해야 다음 case문을 실행한다.
- fallthrough를 사용하면 다음 case문의 코드는 조건을 확인하지 않고 무조건 실행한다.

### Labeled Statement

- 타 언어의 label문과 동일
- 반복문이나 조건문이 중첩되어 복잡한 구조를 이룰 경우 특정 반복문이나 조건문에 label name:을 코딩하여  break나 continue문을 만났을 break label name 또는 continue label name 형식으로 label name 위치로 제어를 이동하는 방법이다.
