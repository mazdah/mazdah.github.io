---
layout: article
title: "Spring - bean Life Cycle - 4"
excerpt: "bean 생성 시점에 통지 받기 - @PostConstruct  애노테이션 사용"
mathjax: true
tags:
- [Spring, IoC, DI, Application Context, bean, life cycle, annotation, @PostConstruct]
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

# bean 생성 시점에 통지 받기 - @PostConstruct  애노테이션 사용

---

- 스프링 2.5부터 사용 가능
- bean 라이프 사이클과 관련된 JSR-250 애노테이션 중 하나
- 초기화 시 호출할 메서드 앞에 붙임

---

```java
...

public class SingerWithJSR250 {
	private static final String DEFAULT_NAME = "Eric Clapton";

	private String name;
	private int age = Integer.MIN_VALUE;

	public void setName(String name) {
		this.name = name;
	}

	public voic setAge(int age) {
		this.age = age;
	}

	// 메서드 이름은 임의로 만들 수 있음
	@PostConstruct
	private void init() throws Exception {
		System.out.println("bean 초기화");

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

	public static void main(String... args) {
		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
		ctx.load("classpath:spring/app-context-xml.xml");
		ctx.refresh();

		getBean("singerOne", ctx);
		getBean("singerTwo", ctx);
		getBean("singerThree", ctx);

		ctx.close();
	}

	public static SingerWithJSR250 getBean(Sting beanName, ApplicationContext ctx) {
		try {
			SingerWithJSR250 bean = (SingerWithJSR250) ctx.getBean(beanName);
			System.out.println(bean);
			return bean;
		} catch(BeanCreationException ex) {
			System.out.println("빈 구성 도중 에러 발생: " + ex.getMessage());
			return null;
		}
	}

}
```

---

```xml
<beans...
	default-lazy-init="true">

	<!-- 애노테이션을 적용하기 위해서는 아래 코드가 반드시 필요함 -->
	<context:annotation-config />
	<bean id="singerOne"
		  class="com...SingerWithJSR250"
		  p:name="John Mayer" p:age="39" />

	<bean id="singerTwo"
		  class="com...SingerWithJSR250"
		  p:age="72" />

	<bean id="singerThree"
		  class="com...SingerWithJSR250"
		  p:name="John Butler" />
</beans>
```

---

### 초기화 방법의 장단점

- 초기화 메서드를 사용하는 경우 Spring과의 결합도는 낮출 수 있으나 초기화가 필요한 모든 bean을 구성에 등록해야 한다.
- InitializingBean 인터페이슬 구현하는 경우 모든 인스턴스 초기화 콜백을 한번에 지정할 수 있으나 Spring과의 결합도가 높아진다.
- 애노테이션을 사용하는 경우 IoC 컨테이너가  JSP-250을 확실히 지원해야 한다.
- 대체로 이식성이 중요하다면 초기화 메서드나 애노테이션을 사용하고 그렇지 않다면  InitializingBean 인터페이스를 사용하는 것이 좋다.
- init-method나 애노테이션을 사용하는 경우에는 초기화 메서드의 접근 권한을 다르게 설정할 수 있다. 하지만 초기화 메서드는 반드시 private여야 한다.
