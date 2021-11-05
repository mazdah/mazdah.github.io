---
layout: article
title: "Spring - bean Life Cycle - 소멸시 통지받기 2"
excerpt: "bean 소멸 시점에 통지 받기 - DisposableBean 인터페이스 구현"
mathjax: true
tags:
- [Spring, IoC, DI, Application Context, bean, life cycle, destroy, DisposableBean]
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

# bean 소멸 시점에 통지 받기 - DisposableBean 인터페이스 구현

---

- 초기화 시와 마찬가지로 소멸 시의 처리를 위한 **DisposableBean** 인터페이스를 구현할 수 있다.
- **DisposableBean** 인터페이스는**destroy()** 메서드 하나를 정의한다.
- bean 소멸 직전에 이 **destroy()** 메서드가 실행된다.

---

```java
...

public class DescructiveBeanWithInterface
			implements InitializingBean, DisposableBean {

	private File file;
	private String filePath;

	@Override
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

	@Override
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
		ctx.read("classpath:spring/app-context-xml.xml");
		ctx.refresh();

		DescructiveBeanWithInterface bean =
			(DescructiveBeanWithInterface) ctx.getBean("destructiveBean");

		System.out.println("destroy() 호출 시작");
		ctx.destroy();
		System.out.println("destroy() 호출 종료");
	}
}
```

---

```xml
<beans...>
	<!-- DisposableBean 인터페이스를 사용하면서 destroy-method 속성이 사라졌다. -->
	<bean id="destructiveBean"
        class="com.... DescructiveBeanWithInterface"
        p:filePath="...text.txt" />
</beans>
```
