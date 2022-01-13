---
layout: article
title: "FactoryBean 사용하기 - 2"
excerpt: "FactoryBean 직접 접근하지와 <bean> 속성 사용"
mathjax: true
tags:
- [Spring, IoC, DI, Application Context, Bean, FactoryBean, factory-bean, factory-method]
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

# FactoryBean 사용하기 2

---

### FactoryBean에 직접 접근하기

- FactoryBean에 접근하기 위해서는 bean 이름 앞에 & 문자를 붙여 getBean() 메서드를 호출한다.
- 하지만 애플리케이션 개발 시에는 권장하지 않음(불필요할 뿐만 아니라 특정 구현에 의존성을 만들게 된다)

```java
...

public class AccessingFactoryBean {
	GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
	ctx.load("classpath:spring/app-context-xml.xml");
	ctx.refresh();
	ctx.getBean("shaDigest", MessageDigest.class);

	MessageDigestFactoryBean factoryBean =
			(MessageDigestFactoryBean) ctx.getBean("&shaDigest");

	try {
		MessageDigest shaDigest = factoryBean.getObject();
		System.out.println(shaDigest.digest("Hello World!".getBytes()));
	} catch (Exception ex) {
		ex.printStacktrace();
	}

	ctx.close();
}
```


### factory-bean과 factory-method 어트리뷰트 사용하기

- 스프링을 사용하지 않는 서드파티 애플리케이션을 사용하게 되는 경우 actory-bean과 factory-method 속성을 사용한다.

```java
public class MessageDigestFactory {
	private String algorithmName = "MD5";

	public MessageDigest createInstance() throws Exception {
		return MessageDigest.getInstance(algorithmName);
	}

	public void setAlgorithmName(String algorithmName) {
		this.algorithmName = algorithmName;
	}
}
```


```xml
<bean...>

	<bean id="shaDigestFactory" class="...MessageDigestFactory"
		p:algorithmName="SAH1" />

	<bean id="defaultDigestFactory" class="...MessageDigestFactory" />

	<bean id="shaDigest"
		factory-bean="shaDigestFactory"
		factory-method="createInstance" />

	<bean id="defaultDigest"
		factory-bean="defaultDigestFactory"
		factory-method="createInstance" />

	<bean id="digester" class="...MessageDigester"
		p:digest1-ref="shaDigest"
		p:digest2-ref="defaultDigest" />
</bean>
```


```java
...

public class MessageDigestFactoryDemo {
	public static void main(String... args) {
		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
		ctx.load("classpath:spring/app-context-xml.xml");
		ctx.refresh();

		MessageDigester digester = (MessageDigester) ctx.getBean("digester");
		digester.digest("Hello World!");

		ctx.close();
	}
}
```
