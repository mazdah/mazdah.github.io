---
layout: article
title: "Bean이 Spring 알게 하기"
excerpt: "BeanNameAware와 ApplicationContextAware 사용"
mathjax: true
tags:
- [Spring, IoC, DI, Application Context, Bean, Shutdown Hook, BeanNameAware, ApplicationContextAware]
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

# Bean이 Spring을 알게 하기

---

- 의존성 주입의 장점 중 하나는 Bean이 자신을 관리하는 컨테이너의 구현에 대해 알 필요가 없다는 점이다.
- 하지만 특정한 경우에는 bean이 컨테이너에 접근할 필요가 있으며 이에 필요한 의존성을 주입받아야 한다.
- 특정한 경우의 예
  - 셧다운 후크를 자동으로 구성하는 bean
  - bean이 컨테이너에서 어떤 이름으로 사용되는지 알아내어 부가적인 처리 작업을 수행해야 하는 경우
- 이러한 기능들은 스프링이 내부적으로 사용하기 위해 만들어진 기능들이다.
- 일반적으로 bean의 이름에 비지니스 의미를 부여하면 안되나, 로그를 남길 경우에는 bean 이름을 알아낼 수 있다는 것이 매우 유용하다.

---

### BeanNameAware 인터페이스 사용

- 자신에게 부여된 이름을 알고자 할 때 사용
- setBeanName (String) 메서드 하나가 있음
- 스프링은 bean 구성을 마친 후 라이프사이클 콜백(초기화나 소멸 시점)을 호출하기 전에 setBeanName 메서드를 호출한다.
- BeanNameAware 인터페이스를 사용하기 위한 특별한 구성은 필요 없다.

---

```java
...

public class NamedSinger implements BeanNameAware {
	private String name;

	// 인스턴스가 애플리케이션에 주입되기 전에 호출된다.
	public void setBeanName(String beanName) {
		this.name = beanName;
	}

	// 인스턴스가 애플리케이션에 주입되기 전에 setBeanName이 호출되므로 name의 사용 가능 여부는
	// 확인할 필요가 없다.
	public void sing() {
		System.out.println("Singer [" + name + "] - sing()");
	}
}
```

---

```xml
<!-- 특별한 구성은 필요 없다. -->
<beans...>
	<bean id="johnMayer" class="com...NamedSinger" />
</beans>
```

---

```java
...

public class NamedSingerDemo {
	public static void main(String... args) {
		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
		ctx.load("classpath:spring/app-context-xml.xml");
		ctx.refresh();

		NamedSinger bean = (NamedSinger) ctx.getBean("johnMayer");
		bean.sing();

		ctx.close();
	}
}
```

---

- bean에 내부적으로 사용할 이름을 부여하기 위해서는 setName() 메서드를 가진 인터페이스를 만들고, 이 인터페이스를 bean에서 구현한 후 이름을 의존성 주입으로 전달해야 한다.

---

### ApplicationContextAware 인터페이스 사용

- setApplicationContext(ApplicationContext) 메서드를 구현해야 한다.
- 의존성 주입이 가능한데 ApplicationContextAware를 이용하여 bean을 가져오는 것은 복잡해지기만 하고 아무 이득이 없다.
- ApplicationContextAware 인터페이스를 사용하면 ApplicationContext에서 bean이 구성될 때 셧다운 후크를 자동으로 생성하고 구성할 수 있다.

---

```java
...

publuc class ShutdownHookBean implements ApplicationContextAware {
	private ApplicationContext ctx;

	public void setApplicationContext(ApplicationContext ctx)
		throws BeanException {

		if (ctx instanceof GenericApplicationContext) {
			((GenericApplicationContext) ctx).registerShutdownHook();
		}
	}
}
```

---

```xml
<beans...>
<!-- bean 등록 외에는 특별히 할 것이 없다. -->
	<context:annotation-config />

	<bean id="destructiveBean"
		  class="com...DestructiveBeanWithInterface"
		  p:filePath="text.txt" />

	<bean id="shutdownHook"
		  class="com...ShutdownHookBean" />
</beans>
```

---


```java
...

public class DescructiveBeanWithInterface {

	private File file;
	private String filePath;

	@PostConstruct
	public void afterPropertiesSet() throws Exception {
		System.out.println("bean을 초기화 합니다.");

		if (filePath == null) {
			throw new IllegalArgumentException(
				DescructiveBeanWithInterface.class +
				"에 filePath 속성을 지정해야 합니다.");
		}

		this.file = new File(filePath);
		this.file.createNewFile();

		System.out.println("파일 존재 여부: " + file.exists());
	}

	@PreDestroy
	public void destroy() {
		System.out.println("bean을 소멸합니다.");

		if (!file.delete()) {
			System.out.println("에러: 파일 삭제에 실패하였습니다.");
		}

		System.out.println("파일 존재 여부: " + file.exists());
	}

	public void setFilePath(String filePath) {
		this.filePath = filePath;
	}

	public static void main(String... args) throws Exception {
		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
		ctx.road("classpath:spring/app-context-xml.xml");
		ctx.refresh();
		ctx.getBean("destructiveBean", DestructiveBeanWithInterface.class);

		/*
		명시적으로 destroy()를 호출하지 않아도 shutdownHook bean이 셧다운 후크를 등록하여
		애플리케이션 종료 시 등록한 셧다운 후크가 destroy()를 호출하게 된다.
		*/
	}
}
```

