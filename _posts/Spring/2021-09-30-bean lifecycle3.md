---
layout: article
title: "Spring - bean Life Cycle - 3"
excerpt: bean 생성 시점에 통지 받기 - InitializingBean 구현
mathjax: true
tags:
- [Spring, IoC, DI, Application Context, bean, life cycle, InitializingBean, afterPropertiesSet]
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

# bean 생성 시점에 통지 받기 - InitializingBean 인터페이스 구현
- - - -
- bean의 구성이 유효한지 확인 가능
- 특정 기본값을 설정할 수 있음
- init 메서드와 동일한 기능을 하는 **afterPropertiesSet** 메서드를 정의
- xml 구성 파일의 <bean> 태그에 init-method 속성을 지정할 필요가 없다.
- init 메서드가 afterPropertiesSet 메서드로 바뀌었다는 것 외에 init 메서드를 사용할 때와 차이가 없다.

---

```java
...
public Class SingerWithInterface implements InitializingBean {
	private static final String DEFAULT_NAME = "Eric Clapton";

	private String name;
	private int age = Integer.MIN_VALUE;

	public void setName(String name) {
		this.name = name;
	}

	public void setAge(int age) {
		this.age = age;
	}

	/*
		init 메서드 대신 InitializingBean 인터페이스에 정의된 afterPropertiesSet 함수를
		구현한다.
	*/
	public void afterPropertiesSet() throw Exception {
		System.out.println("빈 초기화");

		if (name == null) {
			System.out.println("기본 가수 이름 설정");
			name = DEFAULT_NAME;
		}

		if (age == Integer.MIN_VALUE) {
			throw new IllegalArgumentException(
					SingerWithInterface.class +
					" 빈 타입에는 반드시 age 프로퍼티를 설정해야 합니다.");
		}
	}

	public String toString() {
		return "\t이름 : " + name + "\n\t나이 : " + age);
	}

	public static void main(String... args) {
		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
		ctx.load("classpath:spring/app-context-xml.xml");
		ctx.refresh();

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
<!--
	InitializingBean 인터페이스 구현의로 초기화 콜백으로 어떤 메서드를 호출해야 할지 알기 때문에
	init-method 속성을 지정할 필요가 없다.
-->
<beans...
	default-lazy-init="true">

	<!-- bean에 2개의 속성을 모두 지정하였으므로 정상적으로 값이 주입되어 출력된다. -->
	<bean id="singerOne"
		class="com.apress...SingerWithInterface"
		p:name="John Mayer" p:age="39" />

	<!-- bean에 이름 속성이 주입되지 않아 이름은 DEFAULT_NAME값인 Eric Clapton이 출력된다. -->
	<bean id="singerTwo"
		class="com.apress... SingerWithInterface"
		p:age="39" />

	<!-- bean에 age 속성이 주입되지 않았으므로 IllegalArgumentException을 던진다. -->
	<bean id="singerThree"
		class="com.apress... SingerWithInterface"
		p:name="John Butler" />
</beans>

```
