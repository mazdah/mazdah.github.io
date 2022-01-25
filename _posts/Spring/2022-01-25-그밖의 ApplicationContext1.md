---
layout: article
title: "그 외의 스프링 ApplicationContext 구성 살펴보기 - 1"
excerpt: "MessageSource를 이용한 국제화"
mathjax: true
tags:
- [Spring, ApplicationContext, MessageSource]
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

# 그 외의 스프링 ApplicationContext 구성 살펴보기

---

- ApplicationContext의 가장 큰 장점은 스프링과 스프링이 관리하는 리소스를 100% 선언적 방식으로 구성하고 관리할 수 있다는 점이다(애플리케이션에서 ApplicationContext에 접근하기 위한 별도의 코드를 작성할 필요가 없다).
- 웹 애플리케이션에서는 배포 기술자(web.xml)에서 ApplicationContext를 초기화 할 수 있다.
- ApplicationContext는 다음의 기능들도 제공한다.
  - 국제화
  - 이벤트 발생
  - 리소스 관리 및 접근
  - 추가적인 라이프사이클 인터페이스
  - 개선된 스프링 인프라스트럭처 컴포넌트 자동 구성

### MessageSource를 사용한 국제화

- MessageSource를 사용하여 메시지라고 불리는 다양한 언어로 저장된 String 리소스에 접근할 수 있다.
- ApplicationContext 인터페이스가 MessageSource를 상속해 메시지를 읽어 들이고 애플리케이션 환경에서 읽어 들인 메시지에 자동으로 접근하는 특수한 기능을 제공한다.
- 메시지에 접근하는 기능은 MVC 프레임워크로 웹 애플리케이션을 만드는 경우와 같은 특정한 시나리오에서만 가능하다.

#### MessageSource를 사용한 국제화

- 스프링은 ApplicationContext외에 3가지 MessageSource 구현체를 제공한다.
  - ResourceBundleMessageSource : 자바의 ResourceBundle을 이용하여 메시지를 읽어 들임
  - ReloadableResourceBundleMessageSource: 위와 동일하나 주기적으로 메시지를 다시 읽어들일 수 있음
  - StaticMessageSource (외부 구성을 할 수 없기에 상용 애플리케이션에는 사용하지 말아야 함)
- 세 가지 구현체 모두 HierarchicalMessageSource 인터페이스를 구현하여 여러 MessageSource 인스턴스를 내장할 수 있다.
- 사용하기 위해서는 messageSource라는 이름으로 MessageSource 타입의 bean을 구성에 정의해야 한다.

```java
...

import java.util.Locale;

public class MessageSourceDemo {
	public static void main(String... args) {
		GenericXmlApplicationContext ctx =
				new GenericXmlApplicationContext();
		ctx.load("classpath:spring/app-context-xml.xml");
		ctx.refresh();

		Locale english = Locale.ENGLISH;
		Locale korean = new Locale("ko", "KR");

		System.out.println(ctx.getMessage("msg", null, english));
		System.out.println(ctx.getMessage("msg", null, korean));

		System.out.println(ctx.getMessage("nameMsg", new Object[] {"John", 					"Mayer" }, english));
		System.out.println(ctx.getMessage("nameMsg", new Object[] {"John", 					"Mayer" }, korean));

		ctx.close();
	}
}
```


```xml
<beans...>

<!--구성 파일에 messageSource라는 이름으로 MessageSource bean을 정의하기만 하면ApplicationContext를 MessageSource로 사용할 수 있다.-->
	<bean id="messageSource" class="...ResourceBundleMessageSource"
		p:basenames-ref="basenames" />

	<util:list id="basenames">
		<value>buttons</value>
		<value>labels</value>
	</util:list>
</beans>
```

- ResourceBundle은 지정된 로케일 이름이 붙은 프로퍼티 파일에서 해당 메시지를 검색한다.

```properties
# labels_en.properties
msg=My stupid mouth has got me in trouble
nameMsg=My name is {0} {1}
```


```properties
# labels_ko.properties
msg=내 멍청한 입이 나를 곤경에 빠뜨렸지
nameMsg=내 이름은 {0} {1}
```


#### getMessage() 메서드 사용하기

- 3개의 Overload 메서드
  - getMessage(String, Object[], Locale)
  - getMessage(String, Object[], Locale), String, Locale)
  - getMessage(MessageSourceResolvable, Locale)

#### ApplicationContext를 MessageSource로 사용하는 이유

- 스프링 MVC 프레임워크를 사용하여 웹 애플리케이션을 만들 때만 ApplicationContext를 이용하여 메시지를 가져와야 한다.
- 스프링의 컨트롤러를 구성하는 구조상 ApplicationContext를 MessageSource로 사용하면 모든 컨트롤러에 MessageSource 프로퍼티를 노출하지 않아도 된다.
- 스프링이 뷰 레이어에도 ApplicationContext를 가능한한 MessageSource로 노출한다.
  - \<spring:message\>
  - \<fmt:message\>
