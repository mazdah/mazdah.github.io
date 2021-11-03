---
layout: article
title: "Spring - bean Life Cycle - 소멸시 통지받기 1"
excerpt: "bean 소멸 시점에 통지 받기 - 소멸 콜백 지정"
mathjax: true
tags:
- [Spring, IoC, DI, Application Context, bean, life cycle, destroy-method]
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

# bean 소멸 시점에 통지 받기

---

- DefaultListableBeanFactory 인터페이스를 래핑한 AppliccationContext 구현체를 이용하는 경우ConfigurableBeanFactory.destroySingletons()를 호출하여 모든 싱글턴 인스턴스를 소멸시키도록 통지할 수 있다.
- 소멸 통지를 통해 애플리케이션이 사용중인 모든 리소스를 정리한 후 애플리케이션을 안전하게 종료할 수 있다.
- 소멸 통지 콜백은 메모리의 데이터를 안전하게 저장하고 오래 실행 중인 bean을 종료하는 최적의 장소이다.
- 일반적으로 소멸 콜백은 초기화 콜백과 쌍으로 사용되며 초기화 콜백에서 생성된 리소스를 소멸 콜백에서 반납한다.

---

### Bean이 소멸될 때 메서드를 실행하기

- 구성파일의 <bean> 태그에 destroy-method 속성을 추가하여 호출될 메서드 이름을 지정한다.
- 스프링은 bean의 싱글턴 인스턴스가 소멸하기 직전에 이 메서드를 호출한다.
- 프로토타입 스코프에 있는 소멸 콜백 메서드는 호출하지 않는다.

---

```java
...

public class DestructiveBean {
	private File file;
	private String filePath;

	public void afterPropertiesSet() throws Exception {
		System.out.println("bean을 초기화합니다.");

		if (filePath == null) {
			throw new IllegalArgumentException(
				DestructiveBean.class + "에 filePath 속성을 지정해야 합니다.");
		}

		this.file = new File(filePath);
		this.file.createNewFile();

		System.out.println("파일 존재 여부: " + file.exists());
	}

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

		DestructiveBean bean = (DestructiveBean) ctx.getBean("destructiveBean");

		System.out.println("destroy() 호출 시작");
		ctx.destroy();
		System.out.println("destroy() 호출 종료");
	}
}
```

---

```xml
...

<beans...>
	<bean id="destructiveBean"
		  class="com....DestructiveBean"
		  descroy-mathod="destroy"
		  init-method="afterPropertiesSet"
		  p:filePath="...text.txt" />
</beans>
```
