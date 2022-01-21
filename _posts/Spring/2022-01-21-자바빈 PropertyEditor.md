---
layout: article
title: "자바 Bean Property - 1"
excerpt: "Spring이 기본 제공하는 PropertyEditor 사용"
mathjax: true
tags:
- [Spring, JAVA Bean Property, PropertyEditor]
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

# 자바 Bean Property

---

- 프로퍼티 값을 원래 자료 타입에서 String으로 변환하거나 반대로 String을 원래 타입으로 변환하는 인터페이스
- java.beans.PropertyEditorSupport를 상속
- BeanFactory에 의해 사전 등록되어 있음

### 스프링이 기본으로 제공하는 PropertyEditor 사용하기

```java
...

public class PropertyEditorBean {
	private byte[] bytes;
	private Character character;
	private Class cls;
	private Boolean trueOrFalse;
	private List<String> stringList;
	private Date date;
	private Float floatValue;
	private File file;
	private InputStream stream;
	private Locale locale;
	private Pattern pattern;
	private Properties properties;
	private String trimString;
	private URL url;

	public void setCharacter(Character character) {
		System.out.println("Character 설정: " + character);
		this.character = character;
	}

	public void setCls(Class cls) {
		System.out.println("Class 설정: " + cls.getName());
		this. cls = cls;
	}

	public void setFile(File file) {
		System.out.println("File 설정: " + file.getName());
		this.file = file;
	}

	public void setLocale(Locale locale) {
		System.out.println("Locale 설정: " + locale.getDisplayName());
		this.locale = locale;
	}

	public void setProperties(Properties properties) {
		System.out.println("읽어 들인 Properties 개수: " + properties.size() + "개");
		this.properties = properties;
	}

	public void setUrl(URL url) {
		System.out.println("URL 설정: " + url.toExternalForm());
		this.url = url;
	}

	public void setBytes(byte... bytes) {
		System.out.println("bytes 설정: " + Array.toString(bytes));
		this.bytes = bytes;
	}

	public void setTrueOrFalse(Boolean trueOrFalse) {
		System.out.println("Boolean 설정: " + trueOrFalse);
		this.trueOrFalse = trueOrFalse;
	}

	public void setStringList(List<String> stringList) {
		System.out.println("String 목록 설정. 크기: " + stringList.size());
		this.stringList = stringList;

		for (String string: stringList) {
			System.out.println("String 멤버: " + string);
		}
	}

	public void setDate(Date date) {
		System.out.println("Date 설정: " + date);
		this.date = date;
	}

	public void setDate(Date date) {
		System.out.println("Date 설정: " + date);
		this.date = date;
	}

	public void setFloatValue(Float floatValue) {
		System.out.println("Float 값 설정: " + floatValue);
		this.floatValue = floatValue;
	}

	public void setStream(InputStream stream) {
		System.out.println("Stream 설정: " + stream);
		this.stream = stream;
	}

	public void setPattern(Pattern pattern) {
		System.out.println("Pattern 설정: " + pattern);
		this.pattern = pattern;
	}

	public void setTrimString(String trimString) {
		System.out.println("문자열 trim 설정: " + trimString);
		this.trimString = trimString;
	}

	public static class CustomPropertyEditorRegistrar implements
       	PropertyEditorRegistrar {
		@Override
		public void registerCustomEditor(PropertyEditorRegistry registry) {
			SimpleDateFormat dateFormatter = new SimpleDateFormat("yyyy/MM/dd");
			registry.registerCustomEditor(Date.class,
					new CustomDateEditor(dateFormatter, true));
			registry.registerCustomEditor(String.class,
					new StringTrimmerEditor(true));
		}
	}

	public static void main(String... args) thorows Exception {
		File file = File.createTempFile("test", "txt");
		file.deleteOnExit();

		GenericXmlApplicationContext ctx = new GenericXmlApplicationContext();
		ctx.load("classpath:spring/app-context-01.xml");
		ctx.refresh();

		PropertyEditorBean bean =
					(PropertyEditorBean) ctx.getBean("builtInSample");

		ctx.close;
	}
}
```


```xml
...
<beans...>

	<bean id="customEditorConfigurer"
		  class="...CustomEditorConfigurer"
		  p:propertyEditorRegistrar-ref="propertyEditorRegistrarsList" />

	<util:list id="propertyEditorRegistrarsList">
		<bean class="...PropertyEditorBean$CustomPropertyEditorRegistrar" />
	</util:list>

	<bean id="builtInSample" class="...PropertyEditorBean"
		p:character="A"
		p:bytes="John Mayer"
		p:cls="java.lang.String"
		p:trueOrFalse="true"
		p:stringList-ref="stringList"
		p:stream="test.txt"
		p:floatValue="123.45678"
		p:date="2018/03/13"
		p:file="#{systemProperties['java.io.tmpdir']}
				#{systemProperties['file.separator']}test.txt"
		p:locale="ko_KR"
		p:pattern="a*b"
		p:properties="name=Chris age=32"
		p:trimString="String need trimming"
		p:url="https://spring.io"
	/>

	<util:list id="stringList">
		<value>String member 1</value>
		<value>String member 1</value>
	</util:list>
</beans>
```

- 위 예제를 실행하면 구성 파일에서 문자열로 전달된 속성들이 원래의 type으로 변환되어 보여진다.
