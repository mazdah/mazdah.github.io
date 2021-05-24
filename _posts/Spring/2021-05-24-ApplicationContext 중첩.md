---
layout: article
title: Spring - ApplicationContext의 중첩
excerpt: 여러 개의 ApplicationContext를 사용하는 방법
mathjax: true
tags:
  - [Spring, IoC, DI, Application Context, 중첩]
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

### ApplicationContext의 중첩

- Spring은 ApplicationContext의 중첩을 지원함
- bean이 많은 대형 프로젝트에서는 여러개의 구성 파일이 사용 가능
- GenericXmlApplicationContext 중첩 방법
- local 속성을 사용하는 경우 부모 context를 참조할 수 없다.

```java
public class HierarchicalAppContextUsage {

	GenericXmlApplicationContext parent = new GenericXmlApplicationContext();
	parent.load("classpath:spring/parent-context.xml");
	parent.refresh();

	GenericXmlApplicationContext child = new GenericXmlApplicationContext();
	child.load("classpath:spring/child-context.xml");
	// parent context가 부모가 된다.
	child.setParent(parent);
	child.refresh();

	...
}
```

```xml
<!-- parent-context.xml -->
<beans ...>
	<bean id="childTitle" class="java.lang.String" c:_0="Daughters" />

	<bean id="parentTitle" class="java.lang.String" c:_0="Gravity" />
</beans>
```

```xml
<!-- child-context.xml -->
<beans ...>
	<!-- parentTitl은 부모(parent-context.xml)에만 정의 되어 있으므로 부모를 참조한다. -->
	<bean id="song1" class="com.apress...Song"
		  p:title-reg="parentTitle" />
	<!-- childTitle은 부모(parent-context.xml)와 자식(현재 파일)에 모두 정의되어 있다. -->
	<!-- 이 경우 현재 파일을 우선 참조한다. -->
	<bean id="song2" class="com.apress...Song"
		  p:title-reg="childTitle" />

	<!-- childTitle은 부모(parent-context.xml)와 자식(현재 파일)에 모두 정의되어 있다. -->
	<!-- 하지만 ref 태그에서 parent 속성을 사용하고 있으므로 부모(parent-context.xml)에 있는 childTitle를 참조한다. -->
	<bean id="song3" class="com.apress...Song">
		<property name="title">
			<ref parent="childTitle" />
		</property>
	</bean>

	<bean id="childTitle" class="java.lang.String" c:_0="해당 값이 없습니다." />
</beans>
```
