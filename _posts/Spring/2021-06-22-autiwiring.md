---
layout: article
title: "Spring - 자동와이어링(Autowiring) 1"
excerpt: 구성 파일을 이용하여 자동와이어링하기
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

# bean에 자동와이어링 하기

---
### 자동와이어링(autowiring)을 수행하는 5가지 방법

1. **byName** : 대상 bean 내의 프로퍼티와 이름이 같은 bean을 찾아서 연결
2. **byType** : 스프링은 ApplicationContext 내에서 대상 bean의 프로퍼티와 동일한 타입의 bean을 찾아 연결한다.
3. **Constructor(생성자)** : 생성자를 이용한 주입이라는 점을 제외하면 **byType**과 같다. 스프링은 생성자에서 가능한한 많은 인수들을 일치시키려고 한다. 생상저의 인수 타입과 동일한 타입의 bean들이 ApplicationContext에 존재한다면 인수가 많은 생성자가 우선 사용된다.
4. **default** : 스프링은 **Constructor** 방식과 **byType** 방식을 자동으로 선택한다. 대상 bean에 기본 생성자가 있다면 **byType** 방식을, 그렇지 않다면 **Constructor** 방식을 사용한다.
5. **no** : 스프링의 기본값

> #### 동일한 ApplicationContext 인스턴스에 타입이 동일한 bean이 둘 이상 있으면 스프링은 자동와이어링에 사용할 타입을 결정할 수 없고, org.springframework.beans.factory.NosuchBeanDefinitionException 예외를 던진다.


### 구성파일 예제

```xml
<beans...>

	<bean id="fooOne" class="com.apress...Foo" />
	<bean id="barOne" class="com.apress...Bar" />

	<bean id="targetByName" autowire="byName"
		class="com.apress...Target" lazy-init="true" />
	<bean id="targetByType" autowire="byType"
		class="com.apress...Target" lazy-init="true" />
	<bean id="targetConstructor" autowire="constructor"
		class="com.apress...Target" lazy-init="true" />
</beans>
```


```java
...
public class Target {

	private Foo fooOne;
	private Foo fooTwo;
	private Bar bar;

	public Target() {

	}

	public Target (Foo foo) {
		System.out.println("Target(Foo) 호출");
	}

	public Target (Foo foo, Bar bar) {
		System.out.println("Target(Foo, Bar) 호출");
	}

	public void setFooOne(Foo fooOne) {
		this.fooOne = fooOne;
		System.out.println("프로퍼티 fooOne 설정");
	}

	public void setFooTwo(Foo foo) {
		this.fooTwo = fooOne;
		System.out.println("프로퍼티 setFooTwo 설정");
	}

	public void setBar(Bar bar) {
		this. bar = bar;
		System.out.println("프로퍼티 bar 설정");
	}

	public static main(String...args) {
		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
		ctx.load("classpath:spring/app-context-03.xml");
		ctx.regresh();

		Target t = null;

		System.out.println("byName을 사용: \n");
		t = (Target) ctx.getBean("targetByName");

		System.out.println("\nbyType을 사용: \n");
		t = (Target) ctx.getBean("targetByType");

		System.out.println("\nConstructor를 사용: \n");
		t = (Target) ctx.getBean("targetConstructor");

		ctx.close();
	}
}

/*
실행 결과
byName을 사용:			// byName을 사용하는 경우 프로퍼터 이름과 구성 파일의 bean 이름이
						   유일하게 같은 fooOne만 주입됨
프로퍼티 fooOne 설정

byType을 사용:			// byType을 사용하는 경우 bar 프로퍼티는 구성파일의 같은 타입인 barOne
						   으로부터, fooOne과 fooTwo는 구성파일의 fooOne으로부터 주입된다.
프로퍼티 bar 설정
프로퍼티 fooOne 설정
프로퍼티 fooTwo 설정

Constructor를 사용:		// Constructor를 사용하는 경우 인수가 많은 Target(Foo, Bar)가
						   사용된다.
Target(Foo, Bar) 호출
*/
```

### byType을 이용할 경우 주의할 점

같은 인터페이스를 구현한 클래스가 여러개이며, 자동와이어링을 원하는 프로퍼티가 byType을 이용하는 경우.

```java
...
public interface Foo {

}

public class FooImpl1 implements Foo {

}

public class FooImpl2 implements Foo {

}
```

```xml
<beans...>

	<bean id="fooOne" class="com.apress...FooImple1" />

	<bean id="fooTwo" class="com.apress...FooImple2" />

	<bean id="bar" class="com.apress...Bar" />

	<bean id="targetByType" autowire="byType"
		class="com.apress...CTarget" lazy-init="true" />
</beans>
```

```java
public class CTarget {
	...
	public static void main(String...args) {
		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
		ctx.load("classpath:spring/app-context-04.xml");
		ctx.regresh();

		System.out.println("\nUsing byType:\n");
		CTarget t = (CTarget) ctx.getBean("targetByType");
		ctx.close();
	}
}

/*
실행 결과

Using byType :

Exception in thread "main"
	org.springframework.beans.factory.UnsatisfiedDependencyException
...
		org.springframework.beans.factory.NoUniqueBeanDefinitionException:
...
		expected single matching bean but found 2: fooOne, fooTwo

> UnsatisfiedDependencyException : bean을 발견했지만 어떤 bean을 어디에 사용해야 할지
> 모르는 경우 발생
*/
```


### UnsatisfiedDependencyException 해결책
- primary 속성 사용 : bean 정의에서 primary 속성을 사용하여 우선 사용할 bean을 지정한다. 이 방법은 같은 타입의 bean이 꼭 2개만 존재할 때 사용 가능하다.

```xml
<beans...>

	<bean id="fooOne" class="com.apress...FooImple1" primary="true" />

	<bean id="fooTwo" class="com.apress...FooImple2" />
<beans>
```

- bean 이름을 지정하고 XML로 주입할 위치를 구성하는 방법
