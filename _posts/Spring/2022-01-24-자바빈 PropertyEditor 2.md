---
layout: article
title: "자바 Bean Property - 2"
excerpt: "커스텀 PropertyEditor 사용하기 - PropertyEditorSupport"
mathjax: true
tags:
- [Spring, JAVA Bean Property, PropertyEditor, Custom PropertyEditor, PropertyEditorSupport]
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

#  자바 Bean Property 2

---

### 커스텀 PropertyEditor 만들기

- 자바의 일반적인 클래스가 아닌 개발 중인 애플리케이션의 클래스나 클래스 집합 전용으로 필요한 경우
- java.beans.PropertyEditor 인터페이스 구현 시 구현해야 할 메서드가 불필요하게 많다.
- java 5 이후 버전에서 PropertyEditorSupport를 사용할 경우 setAsText() 메서드 하나만 구현하면 됨

```java
...

public Class FullName {
	private String firstName;
	private String lastName;

	public FullName(String firstName, String lastName) {
		this.firstName = firstName;
		this.lastName = lastName;
	}

	public String getFirstName() {
		return firstName;
	}

	public void setFirstName(String firstName) {
		this. firstName = firstName;
	}

	public String getLastName() {
		return lastName;
	}

	public void setLastName(String lastName) {
		this.lastName = lastName;
	}

	public String toString() {
		return "이름: " + firstName + " - 성: " + lastName;
	}
}
```


```java
...
import java.beans.PropertyEditorSupport;

public class NamePropertyEditor extends PropertyEditorSupport {
	@Override
	public void setAsText(String text) throws IlligalArgumentException {
		String[] name = text.split("\\s");

		//변환된 값이 반환될 수 있도록 처리함
		setValue(new FullName(name[0], name[1]));
	}
}
```


```xml
<beans...>

	<bean name="customEditorConfigurer"
		class="...CustomEditorConfigurer"
		<property name="customEditors">
			<map>
				<entry key="...FullName"
					value="...NamePropertyEditor" />
			</map>
		</property>
	</bean>

	<bean id="exampleBean" class="...CustomEditorExample"
		p:name="John Mayer" />
</beans>
```

- 구성 파일에서 주의할 내용
  - 커스텀 PropertyEditor가 Map 타입의 속성으로 CustomEditorConfigurer에 주입됨
  - Map에 담긴 각각의 Entry는 PropertyEditor 하나를 의미하며 key는 PropertyEditor가 사용할 클래스 이름이다.
- 위 구성파일의 내용은 NamePropertyEditor가 key로 지정된 FullName를 사용해야 함을 나타낸다.

```java
...

public class CustomEditorExample {
	private FullName name;

	public FullName getName() {
		return name;
	}

	public void setName(FullName name) {
		this.name = name;
	}

	public static void main(String... args) {
		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
		ctx.load("classpath:spring/app-context-02.xml");
		ctx.refresh();

		CustomEditorExample bean =
			(CustomEditorExample) ctx.getBean("exampleBean");

		System.out.println(bean.getName());
		ctx.close();
	}
}
```
