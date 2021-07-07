---
layout: article
title: "Spring - bean Life Cycle - 2"
excerpt: bean 생성 시점에 통지 받기 - 초기화 콜백 메서드 이용
mathjax: true
tags:
  - [Spring, IoC, DI, Application Context, bean, life cycle, init-method, default-init-method, 초기화 콜백]
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

# bean 생성 시점에 통지 받기 - 초기화 메서드 이용

---

- bean의 초기화 시점을 알면 의존성의 주입 여부를 확인 가능하다.
- 스프링은 bean 생성 시점에 의존성을 주입할 수 없기 때문에 bean은 생성자에서 의존성 주입 여부를 확인할 수 없다.
- 초기화 콜백은 스프링의 의존성 제공이 완료된 이후 호출되어 초기화 콜백에서는 의존성 주입 여부를 확인할 수 있다.
- 초기화 콜백은 의존성을 검사하고 초깃값을 제공하는 용도로 쓰는 것이 가장 적합하다.
- 초기화 콜백 메커니즘은 같은 타입의 bean이 몇 개 안 되거나 애플리케이션이 스프링과 결합되지 않게 할 때 유용하다.
- xml 구성 파일의 <bean> 태그에 init-method 속성으로 초기화 콜백 메서드를 지정한다.
- 초기화 메서드가 가진 유일한 단점은 인자를 받을 수없다는 것이다(무엇을 반환하든 상관 없고 정적 메서드로 만들어도 된다. 하지만 정적 메서드로 만들 경우 상태정보에 접근할 수가 없어 장점이 사라진다).



```java
...
public class Singer {

	private static final String DEFAULT_NAME = "Eric Clapton";

	private String name;
	private int age = Integer.MIN_VALUE;

	public void setName(String name) {
		this.name = name;
	}

	public void setAge(int age) {
		this.age = age;
	}

	// 초기화 콜백 메서드. 주입받을 속성(name, age)에 대한 검증 및 초기값 설정을 처리한다.
	private void init() {
		System.out.println("bean 초기화");

		if (name == null) {
			System.out.println("기본 이름 사용");
			name = DEFAULT_NAME;
		}

		if (age == Integer.MIN_VALUE) {
			thorow new IllegalArgumentException(
				Singer.class + "bean 타입에는 반드시 age 프로퍼티를 설정해야 합니다.");
		}
	}

	public String toString() {
		return "\t이름: " + name + "\n\t나이: " + age;
	}

	public static void main(String... args) {
		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
		ctx.load("classpath:spring/app-context-xml.xml");
		ctx.regresh();

		getBean("singerOne", ctx);
		getBean("singerTwo", ctx);
		getBean("singerThree", ctx);

		ctx.close();
	}

	public static Singer getBean(Sting beanName, ApplicationContext ctx) {
		try {
			Singer bean = (Singer) ctx.getBean(beanName);
			System.out.println(bean);
			return bean;
		} catch(BeanCreationException ex) {
			System.out.println("빈 구성 도중 에러 발생: " + ex.getMessage());
			return null;
		}
	}
}
```


```xml
<!-- default-lazy-init="true" 속성으로 인해 스프링은 애플리케이션이 -->
<!-- 스프링에 bean을 요청할 때만 해당 빈을 초기화 한다. -->
<!-- 많일 이 속성을 false로 하게 되면 스프링은 ApplicationContext을 기동할 때 모든 -->
<!-- bean을 초기화 하게 되고 이 과정에서 singerThree에 예외가 걸리므로 애플리케이션 -->
<!-- 기동에 실패하게 된다 -->
<beans...
	default-lazy-init="true">

	<!-- bean에 2개의 속성을 모두 지정하였으므로 정상적으로 값이 주입되어 출력된다. -->
	<bean id="singerOne"
		class="com.apress...Singer"
		init-method="init" p:name="John Mayer" p:age="39" />

	<!-- bean에 이름 속성이 주입되지 않아 이름은 DEFAULT_NAME값인 Eric Clapton이 출력된다. -->
	<bean id="singerTwo"
		class="com.apress...Singer"
		init-method="init" p:age="39" />

	<!-- bean에 age 속성이 주입되지 않았으므로 IllegalArgumentException을 던진다. -->
	<bean id="singerThree"
		class="com.apress...Singer"
		init-method="init" p:name="John Butler" />
</beans>
```

- 구성 파일 내의 모든 bean에서 사용하는 초기화 메서드 이름이 같다면 <beans> 태그에 default-init-method 속성을 사용하여 초기화 메서드를 지정할 수 있다. 이 때 각각의 bean type은 서로 달라도 된다.

```xml
<beans...
	default-lazy-init="true" default-init-method="init">

	<!-- bean에 2개의 속성을 모두 지정하였으므로 정상적으로 값이 주입되어 출력된다. -->
	<bean id="singerOne"
		class="com.apress...Singer"
		p:name="John Mayer" p:age="39" />

	<!-- bean에 이름 속성이 주입되지 않아 이름은 DEFAULT_NAME값인 Eric Clapton이 출력된다. -->
	<bean id="singerTwo"
		class="com.apress...Singer"
		p:age="39" />

	<!-- bean에 age 속성이 주입되지 않았으므로 IllegalArgumentException을 던진다. -->
	<bean id="singerThree"
		class="com.apress...Singer"
		p:name="John Butler" />
</beans>
```
