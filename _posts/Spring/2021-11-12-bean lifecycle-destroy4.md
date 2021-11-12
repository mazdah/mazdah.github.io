---
layout: article
title: "Spring - bean Life Cycle - 소멸시 통지받기 4"
excerpt: "bean 소멸 시점에 통지 받기 - @Bean 애노테이션 사용과 Shutdown Hook"
mathjax: true
tags:
- [Spring, IoC, DI, Application Context, bean, life cycle, descroy, Bean, Shutdown Hook]
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

# bean 소멸 시점에 통지 받기 - @Bean을 이용한 소멸 메서드 정의

---

- @Bean 애노테이션에 destroyMethod 속성을 추가하고 그 값으로 메서드 이름을 지정한다.
- 이 방법은 자바 구성 클래스를 이용할 때 Bean 선언부에서 사용한다.

---

```java
...

public class DestructiveBeanConfigDemo {

	@Configuration
	static class DestructiveBeanDemo {

		@Lazy
		@Bean(initMethod="afterPropertiesSet", destroyMethod="destroy")
		DestructiveBean destructiveBean() {
			DestructiveBean destructiveBean = new DestructiveBean();
			destructiveBean.setFilePath(System.getProperty("java.io.tmpdir") +
				System.getProperty("file.seperator") + "text.txt");
			return destructiveBean;
		}
	}

	public static void main(String... args) {
		GenericXmlApplicationContext ctx =
			new AnnotationConfigApplicationContext(DestructiveBeanConfig.class);

		ctx.getBean(DestructiveBean.class);
		System.out.println("destroy() 호출 시작");
		ctx.destroy();
		System.out.println("destroy() 호출 종료");
	}
}
```

---

- 초기화와 마찬가지로 이식성이 중요한 경우에는 메서드 콜백을 그렇지 않다면 DisposableBean 인터페이스나 JSR-250 애노테이션을 사용한다.
- 해석 순서는 생성 시와 마찬가지로 @PreDestroy 애노테이션 - DisposableBean.destroy() - xml 구성파일에 정의된 소멸 메서드 순이다.

---

### 셧다운 후크 사용하기

- 소멸 콜백은 자동으로 호출되지 않는다. 애플리케이션 종료 전에 반드시 AbstractApplicationContext.destroy()를 호출해야 한다.
- 서블릿으로 작동하는 경우에는 단순히 destroy() 메서드에서 destroy()를 호출하면 된다.
- 단독 실행형인 경우, 특히 종료 지점이 여러곳인 경우 매우 복잡해진다.
- 자바는 애플리케이션 종료 직전에 수행되는 스레드인 셧다운 후크(Shutdown Hook)를 제공한다.
- AbstractApplicationContext.destroy의 registerShutdownHook() 호출을 통해 스프링은 내부 JVM에 셧다운 후크를 등록한다.

---

```java
...

public class DestructiveBeanWithHook {
	public static void main(String... args) throws Exception {
		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
		ctx.read("classpath:spring/app-context-xml.xml");
		ctx.refresh();

		ctx.getBean(DestructiveBeanWithHook.class);

		// registerShutdownHook()를 호출함으로써 명시적으로 destroy()를 호출하지 않아도 된다.
		ctx.registerShutdownHook();
	}
}
```

