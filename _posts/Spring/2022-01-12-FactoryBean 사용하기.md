---
layout: article
title: "Bean이 Spring 알게 하기 - 2"
excerpt: "FactoryBean 사용"
mathjax: true
tags:
- [Spring, IoC, DI, Application Context, Bean, FactoryBean]
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
# FactoryBean 사용하기

---


- new 연산자로 생성할 수 없는(생성자가 private인) 클래스의  의존성 주입을 위해 FactoryBean 인터페이스를 제공
- 다른 빈을 생성하는 팩터리 역할을 담당하는 bean
- FactoryBean을 이용하여 의존성 요청이나 검색 요청을 하는 경우 스프링은 FactoryBean.getObject() 메서드를 호출하여 반환받은 결과를 반환
- 가장 큰 역할은 트랜잭션 프록시와 JNDI 컨텍스트에서 자동으로 리소스를 가져오는 것
- FactoryBean을 사용하면 기본으로 제공되는 것보다 더 많은 리소스를 IoC를 사용해 관리 가능(생성자가 private이거나 클래스 정보를 미리 알 수 없는 클래스 등)


### FactoryBean(s) 예제: MessageDigestFactoryBean

- 일반적으로 암호화를 위해 Hash 값을 얻으려고 하는 경우

```java
MessageDigest md5 = MessageDigest.getInstance("MD5");
```


- FactoryBean을 구현하여 사용하는 경우

```java
package ...

import java.security.MessageDigest;
import org.springframework.beans.factory.FactoryBean;
import org.springframework.beans.factory.InitializingBean;

public class MessageDigestFatoryBean implements
				FactoryBean<MessageDigest>, InitializingBean {

	private String algorithmName = "MD5";
	private MessageDigest messageDigest = null;

	public MessageDigest getObject() throws Exception {
		return messageDigest;
	}

	public Class<MessageDigest> getObjectType() {
		return MessageDigest.class;
	}

	public boolean isSingleton() {
		return true;
	}

	public void afterPropertiesSet() throws Exception {
		messageDigest = MessageDigest.getInstance(algorithmName);
	}

	public void setAlgorithmName(String algorithmName) {
		this.algorithmName = algorithmName;
	}
}
```

- 스프링은 FactoryBean.getObject() 메서드를 호출하여 FactoryBean이 생성항 객체를 얻는다.
- 위 코드에서 MessageDigestFatoryBean은 InitializingBean의 afterPropertiesSet() 메서드에서 생성한 MessageDigest 인스턴스를 getObject() 메서드를 통해 반환한다.
- FactoryBean.getObjectType() 메서드를 통해 지정된 타입을 반환할 수 있다면 스프링은 FactoryBean 인스턴스를 자동와이어링 할 수 있다(**가급적 인터페이스 타입으로 반환해야 한다**).
- FactoryBean.isSingleton() 메서드는 FactoryBean 자신이 아니라 FactoryBean이 관리하는 인스턴스를 싱글턴으로 사용할 것인지 알려준다.

```java
...

public class MessageDigester {
	private MessageDigest digest1;
	private MessageDigest digest2;

	public void setDigest1(MessageDigest digest1) {
		this.digest1 = digest1;
	}

	public void setDigest1(MessageDigest digest2) {
		this.digest1 = digest2;
	}

	public void digest(String msg) {
		System.out.println("digest1 사용");
		digest(msg, digest1);

		System.out.println("digest2 사용");
		digest(msg, digest2);
	}

	private void digest(String msg, MessageDigest digest) {
		System.out.println("사용하는 알고리즘 : " + digest.getAlgorithm());
		digest.reset();
		byte[] bytes = msg.getBytes();
		byte[] out = digest.digest(bytes);
		System.out.println(out);
	}
}
```


```xml
<beans...>

	<bean id="shaDigest" class="...MessageDigestFactoryBean"
		p:algorithmName="SHA1" />
	<bean id="defaultDigest" class="...MessageDigestFactoryBean" />

	<bean id="digester" class="...MessageDigester"
		p: digest1-ref="shaDigest"
		p: digest2-ref="defaultDigest" />
</beans>
```


```java
...

public class MessageDigestDemo {
	public static void main(String... args) {
		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
		ctx.load("classpath:spring/app-context-xml.xml");
		ctx.refresh();

		MessageDigester digester = ctx.getBean("digester", MessageDigest.class);
		digester.digest("Hello World!");
		ctx.close();
	}
}
```

- 이와 같이 직접 MessageDigest 인스턴스를 생성하지 않고 스프링으로부터 의존성을 주입받아 사용할 수 있다.
