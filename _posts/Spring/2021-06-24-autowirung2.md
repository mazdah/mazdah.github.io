---
layout: article
title: "Spring - 자동와이어링(Autowiring) 2"
excerpt: 애노테이션을 이용하여 자동와이어링하기와 자바 구성파일 이용하기
mathjax: true
tags:
  - [Spring, IoC, DI, Application Context, 자동와이어링, autowiring]
categories: Spring
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
    src: /spring.jpg
---

# bean 자동와이어링 하기 2

---

### 애노테이션을 이용하여 자동와이어링 하기

- 스테레오타입 에노테이션을 사용하는 경우 기본 자동와이어링 방식은 byTpe이다.
- @Autowired 애노테아션과 함께 @Qualifier 애노테이션을 사용하여 bean 이름을 사용할 수 있다.
- @Lazy 애노테이션을 사용하면 처음 접근이 일어날 때 인스턴스가 생성되도록 하는 애노테이션이다.

```java
...
@Component
@Lazy
public class TrickyTarget {
	Foo fooOne;
	Foo fooTwo;
	Bar bar;

	public TrickyTarget() [
		System.out.println("TrickyTarget.constructor()");
	}

	public TrickyTarget (Foo foo) {
		System.out.println("TrickyTarget(Foo) 호출");
	}

	public TrickyTarget (Foo foo, Bar bar) {
		System.out.println("TrickyTarget(Foo, Bar) 호출");
	}

	@Autowired
	public void setFooOne(Foo fooOne) {
		this.fooOne = fooOne;
		System.out.println("프로퍼티 fooOne 설정");
	}

	@Autowired
	public void setFooTwo(Foo fooTwo) {
		this.fooTwo = fooTwo;
		System.out.println("프로퍼티 fooTwo 설정");
	}

	@Autowired
	public void setBar(Bar bar) {
		this.bar = bar;
		System.out.println("프로퍼티 bar 설정");
	}

	public static void main(String... args) {
		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
		ctx.load("classpath:spring/app-context-04.xml");
		ctx.refresh();

		TrickyTarget t = ctx.getBean(TrickyTarget.class);

		ctx.close();
	}
}

/*
출력

TrickyTarget.constructor()
프로퍼티 fooOne 설정
프로퍼티 fooTwo 설정
프로퍼티 bar 설정
*/
```

---

### 애노테이션으로 bean 이름 지정하기
- @Component 애노테이션에 인자로 bean 이름을 전달한다.

```java
...
@Component(“gigi”)
@Lazy
public class TrickyTarget () {
	...
}
```

- 아래와 같이 Foo를 인터페이스로 변경하고 Foo 인터페이스를 구현한 클래스 2개를 만들면 **UnsatisfiedDependencyException**이 발생한다.

```java
...
public interface Foo {

}

public class FooImpl1 implements Foo {
	...
}

public class FooImpl2 implements Foo {
	...
}
```

- @Primary 애노테이션을 사용하면 문제를 해결할 수 있으나 이는 같은 타입의 클래스가 2개인 경우까지만 해당된다.
- @Primary는 속성을 갖지 않는 마커 애노테이션이다.

```java
...
@Component
@Primary
public class FooImpl1 implements Foo {
	...
}
```

- 3개 이상의 동일 타입 bean을 자동와이어링하기 위해서는 @Qualifire 애노테이션에 이름을 지정하는 것이 좋다.

```java
...
@Component(“gigi”)
@Lazy
public class TrickyTarget {
	...
	@Autowired
	// Foo 탸입 클래스에 별도로 이름을 지정하지 않았기 때문에 이름은 클래스 이름을 사용한다.
	@Qualifier("fooImpl1")
	public void setFoo(Foo foo) {
		this.foo = foo;
		System.out.println("Property fooOne set");
	}

	@Autowired
	@Qualifier("fooImpl2")
	public void setFooTwo(Foo fooTwo) {
		this.fooTwo = fooTwo;
		System.out.println("Property fooTwo set");
	}
	...
}
```

---

### 자바 구성 사용하기

- 주입될 클래스는 @Component 애노테이션 대신 @Bean 애노테이션이 사용된다.
- @Bean 애노테이션이 사용되는 경우 자동으로 애노테이션이 적용된 메소드 이름이 bean 이름으로 사용된다.

```java
...
@Configuration
static class TargetConfig {

	@Bean
	public Foo fooImpl1() {
		return new FooImpl1();
	}

	@Bean
	public Foo fooImpl2() {
		return new FooImpl2();
	}

	@Bean
	public Bar bar() {
		return new Bar();
	}
	...
}
```
