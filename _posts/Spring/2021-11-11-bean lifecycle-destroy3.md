---
layout: article
title: "Spring - bean Life Cycle - 소멸시 통지받기 3"
excerpt: "bean 소멸 시점에 통지 받기 - @PreDestroy 애노테이션 사용"
mathjax: true
tags:
- [Spring, IoC, DI, Application Context, bean, life cycle, descroy, PreDestroy]
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

# bean 소멸 시점에 통지 받기 - @PreDestroy 애노테이션 사용

---

- @PostConstruct와 마찬가지로 JSR-250 라이프사이클 애노테이션임
- 소멸 시점에 호출할 메서드에 @PreDestroy 애노테이션을 적용

---

```java
...

public class DestructiveBeanWithJSR250 {
	private File file;
	private String filePath

	@PostConstruct
	public void afterPropertiesSet() throws Exception {
		System.out.println("bean을 초기화 합니다.");

		if (filePath == null) {
			throw new IllegalArgumentException(
				DestructiveBeanWithJSR250.class +
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

		DestructiveBeanWithJSR250 bean =
			(DestructiveBeanWithJSR250) ctx.getBean("destructiveBean");

		System.out.println("destroy() 호출 시작");
		ctx.destroy();
		System.out.println("destroy() 호출 종료");
	}
}
```

- - - -

```xml
<beans...>

	<context:annotation-config />

	<bean id="destructiveBean"
        class="com.... DestructiveBeanWithJSR250"
        p:filePath="...text.txt" />
</beans>
```
