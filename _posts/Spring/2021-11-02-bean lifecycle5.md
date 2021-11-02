---
layout: article
title: "Spring - bean Life Cycle - 5"
excerpt: "bean 생성 시점에 통지 받기 - @Bean 애노테이션의 initMethod 속성 사용"
mathjax: true
tags:
- [Spring, IoC, DI, Application Context, bean, life cycle, annotation, Bean, initMethod]
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

# bean 생성 시점에 통지 받기 - @Bean으로 초기화 메서드 선언

---

- 자바 구성 클래스에서 bean을 선언할 때 사용
- @Bean 애노테이션에 initMethod 속성을 추가하여 초기화 메서드 이름을 지정한다.

---

```java
...

public class SingerConfigDemo {

	@Configuration
	static class SingerConfig {

		@Lazy  // xml 구성파일의 default-lazy-init="true"에 해당
		@Bean(initMethod="init")
		Singer singerOne() {
			Singer singerOne = new Singer();
			singerOne.setName("John Mayer");
			singerOne.setAge(39);
			return singerOne;
		}

		@Lazy
		@Bean(initMethod="init")
		Singer singerTwo() {
			Singer singerTwo = new Singer();
			singerTwo.setAge(72);
			return singerTwo;
		}

		@Lazy
		@Bean(initMethod="init")
		Singer singerThree() {
			Singer singerThree = new Singer();
			singerThree.setName("John Butler");
			return singerThree;
		}
	}

	public static void main(String... args) {
		GenericApplicationContext ctx =
			new AnnotationConfigApplicationContext(SingerConfig.class);

		getBean("singerOne", ctx);
		getBean("singerTwo", ctx);
		getBean("singerThree", ctx);

		ctx.close();
	}
}
```

---

### 초기화 메서드 해석 순서

- bean 인스턴스 하나에 모든 초기화 메커니즘을 다 사용할 수 있다.
- 이 때 실행 순서는 @PostConstruct, afterPropertiesSet(), 구성파일에 기재한 최기화 메서드 순이다.
- bean 생성 시의 단계는 다음과 같다.
  1. 생성자 호출
  2. 의존성 주입(수정자 호출)
  3. bean의 생성 및 의존성 주입을 마친 후 사전 초기화를 담당하는 BeanPostProcessor 기반의 bean들에게 호출해야 하는 메서드가 있는지 확인. 이 때 CommonAnnotationBeanPostProcessor bean에 등록된 @PostConstruct 애노테이션이 적용된 메서드를 호출. bean이 서비스 되기 전에 실행됨
  4. 의존선 주입이 끝난 직후 InitializingBean의 afterPropertiesSet 메서드가 실행됨. bean에 모든 속성들이 주입되고 BeanFactoryAware, ApplicationContextAware가 처리된 후 호출
  5. Init-method 속성으로 지정된 최기화 메서드가 호출됨
