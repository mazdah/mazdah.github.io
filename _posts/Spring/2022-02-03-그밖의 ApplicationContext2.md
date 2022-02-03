---
layout: article
title: "그 외의 스프링 ApplicationContext 구성 살펴보기 - 2"
excerpt: "독립형 애플리케이션에서 MessageSource 사용하기"
mathjax: true
tags:
- [Spring, ApplicationContext, MessageSource, MessageSourceResolvable, ApplicationListener, ApplicationContextAware]
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

# 그 외의 스프링 ApplicationContext 구성 살펴보기 #2

---

### 독립형 애플리케이션에서 MessageSource 사용하기

- 의존성 주입을 통해 MessageSource를 사용하는 것이 가장 좋음


### MessageSourceResolvable 인터페이스

- MessageSource에서 메시지를 가져올 때 MessageSourceResolvable를 구현하면 getMessage() 메서드를 호출하여 가져올 수 있다.


### 애플리케이션 이벤트

- BeanFactory에는 없는 ApplicationContext만의 특징은 ApplicationContext가 중개자로서 이벤트를 발생하고 수신할 수 있다는 것이다.

#### 애플리케이션 이벤트 사용하기

	- 모든 bean은 ApplicationListener<T> 인터페이스를 구현해 이벤트를 받을 수 있다.
	- 이벤트를 발생하는 클래스는 반드시 ApplicationContext에 접근할 수 있어야 한다.
	- 독립형 애플리케이션은 이벤트를 발행할 bean이 ApplicationContextAware를 구현하면 이벤트를 발행할 수 있다.

```java
...
import org.springframework.context.ApplicationEvent;

// 기본 이벤트 클래스
// 생성자에서 부모인 ApplicationEvent에게 source를 전달해준다.
public class MessageEvent extends ApplicationEvent {
	private String msg;

	public MessageEvent(Object source, String msg) {
		super(source);
		this.msg = msg;
	}

	public String getMessage() {
		return msg;
	}
}
```

```java
...
import org.springframework.context.ApplicationListener;

// 이벤트 수신 클래스
// MessageEvent 타입의 이벤트만 수신한다.
public class MessageEventListener implements ApplicationListener<MessageEvent>
{
	@Override
	public void onApplicationEvent(MessageEvent event) {
		MessageEvent msgEvt = event;
		System.out.println("수신: " + msgEvt.getMessage());
	}
}
```

```java
...
import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;
import org.springframework.context.support.ClassPathXmlApplicationContext;

// 이벤트 밣행
// ApplicationContextAware를 구현하여 ApplicationContext에 접근
public class Publisher implements ApplicationContextAware {
	private ApplicationContext ctx;

	public void setApplicationContext(ApplicationContext applicationContext)
		throws BeanException {
		this.ctx = applicationContext;
	}

	public void publish(String message) {
		ctx.publicshEvent(new MessageEvent(this, message));
	}

	public static void main(String... args) {
		ApplicationContext ctx = new ClassPathXmlApplicationContext(
				"classpath:spring/app-context-xml.xml");

		Publisher pub = (Publisher) ctx.getBean("publisher");
		pub.publish("나는 세상에 SOS를 보낸다...");
		pub.publish("... 나는 누군가가 병에 담긴...");
		pub.publish("... 이 메시지를 받았으면 한다.");
	}
}
```

```xml
<beans...>

	<bean id="publisher" class="...Publisher" />

	<bean id="messageEventListener" class="...MessageEventListener" />
</beans>
```


#### 이벤트 사용에 대한 고려사항

	- 특정 컴포넌트가 특정 이벤트를 받아야 하는 경우에는 명시적으로 개별 컴포넌트에 통지를 하는 코드를 작성하거나 JMS같은 메시징 기술을 사용해 처리한다.
	- 보통 이벤트는 빠르게 실행되며 애플리케이션의 주 로직 일부가 아닌 반응 로직에서 사용된다.
	- 장시간 실행되며 주요 비지니스 로직의 일부를 담당하는 처리는 JMS나 RabbitMQ같은 메시징 시스템을 사용하는 것이 권장된다.
	- JMS는 장시간 실행되는 프로세스에 적합하며 추후 JMS 기반의 처리 작업을 별도의 서버로 분리할 수 있는 장점이 있다.
