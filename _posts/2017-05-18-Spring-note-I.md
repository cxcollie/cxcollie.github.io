---
layout: post
title: "Spring Notes I"
date: 2017-05-18
comments: true
tags: [Notes]
---

<div class="post-teaser"> Spring! Spring! Spring! </div>
<!-- more -->

<hr/>

### Inversion of Control <sup>[sec 4-6]</sup>
(spring-demo-one) The approach of outsourcing the construction and management of objects. In IoC, actual implementation can be service locator or dependency injection.

1. add lib files (spring + commons-logging-1.2.jar).
2. **applicationContext.xml** to add beans.
3. in main java, fetch context (of applicationContext.xml), then get bean (where the interface and instance class are specified).
```
Coach theCoach = context.getBean("myCoach", Coach.class);
```
4. Dependency injection: specify dependency (object) by the caller instead of the callee instance. Injection can be done on constructor (in **constructor-arg**), setter(in **property**), or literal value (as **value** instead of **ref**).
5. Bean scope:
	* singleton: default scope; single shared instance of bean
	* prototype: new bean for each container request
	* request: scoped to HTTP web request
	* session: scoped to HTTP web session
	* global-session: scoped to global HTTP web session
6. Life cycle of bean: (Spring does not call pre-destroy callback for prototype beans) 
	* loading context.xml
	* construct instances 
	* injection of dependecies 
	* call post-construct (**@PostConstruct**, or **init-method**) 
	* retrieve bean
	* call pre-destroy (**@PreDestroy** or **destroy-method**)
	* close context.

### Annotation <sup>[sec 7-9]</sup>

1. enable component scan:
```
	<context:component-scan base-package="com.@#$" />
```
2. At the class definition, add **@Component("bean_alias")** (the default name for class **AbcDef** is **abcDef**)
3. Autowired: after adding **@Autowired**, automatically fetch a bean (for constructor, method and field).
4. If several beans match for autowired, **@Qualifier("bean_alias")** can specify which bean to use (for constructor, syntax is different as:
```
	@Autowired
	public TennisCoach(@Qualifier("randomFortuneService") FortuneService theFortuneService)
```
5. To inject properties file using Java annotation, we need to:
	* load properties file in XML: **<context:property-placeholder location="classpath:sport.properties"/>**
	* write annotation as **@Value("${foo.email}")**
6. Scope: At the class definition, add **@Scope("scope_type")**

### Configuration by Java Code <sup>[sec 10]</sup>

1. Create a config class, add **@Configuration** (optional: and **@ComponentScan("com.@#$")**).
2. in main java, add 
```
	AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(config.class);
```
3. Add bean: in config class, add **@Bean beanClass()**.
4. Add properties file: in config class, add **@PropertySource("classpath:property")** (where classpath is src/).

### Spring MVC <sup>[sec 11-13]</sup>
Client -> Dispatcher Servlet (mostly developed by Spring) -> (Model) -> Controller -> (Model) -> View -> Client

1. add lib files (spring + commons-logging-1.2.jar + javax.servlet.jsp.jstl + javax.servlet.jsp.jstl.api).
2. in WEB_INF, add **web.xml** and **config.xml**
	* in **web.xml**:
	```
		<!-- Step 1: Configure Spring MVC Dispatcher Servlet -->
		<servlet>
			<servlet-name>dispatcher</servlet-name>
			<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
			<init-param>
				<param-name>contextConfigLocation</param-name>
				<param-value>/WEB-INF/spring-mvc-demo-servlet.xml</param-value>
			</init-param>
			<load-on-startup>1</load-on-startup>
		</servlet>

		<!-- Step 2: Set up URL mapping for Spring MVC Dispatcher Servlet -->
		<servlet-mapping>
			<servlet-name>dispatcher</servlet-name>
			<url-pattern>/</url-pattern>
		</servlet-mapping>
	```
	* in **config.xml**
	```
		<!-- Step 3: Add support for component scanning -->
		<context:component-scan base-package="com.luv2code.springdemo" />

		<!-- Step 4: Add support for conversion, formatting and validation support -->
		<mvc:annotation-driven/>

		<!-- Step 5: Define Spring MVC view resolver -->
		<bean
			class="org.springframework.web.servlet.view.InternalResourceViewResolver">
			<property name="prefix" value="/WEB-INF/view/" />
			<property name="suffix" value=".jsp" />
		</bean>
	```
3. to add controller: create a controller class, add **@Controller**, add method like:
```
	@RequestMapping("/")
	public String showPage() {
		return "main-menu";
	}
```
4. in jsp page, access variable (in get URL) as **${param.var}**.
5. To add model after controller:
```
	@RequestMapping("/processForm")
	public String letsShout(HttpServletRequest request, Model model) {
		String name = request.getParameter("studentName"); // from GET URL
		name = name.toUpperCase();
		model.addAttribute("message", name);
		return "helloworld";
	}
```
Then to fetch data in jsp: **${message}** (which is not shown in URL).
6. To access static resources (CSS, img):
	* in Spring config file (_config.xml_), add:
	```
		<mvc:resources mapping="/resources/**" location="/resources/"></mvc:resources>
	```
	* in view page, access as:
	```
		<img src="${pageContext.request.contextPath}/resources/images/spring-logo.png">
	```
7. To access param in request with annotation, change the method signature as:
```
	@RequestMapping("/processForm")
	public String letsShout(@RequestParam("studentName") String theName, Model model) {
```
8. Add hierarchical mapping: add **@RequestMapping("/parent-mapping")** in class name.

### Form Tag and Data Binding <sup>[sec 14]</sup>

1. in jsp, create form like:
```
	<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
```
in header, and
```
	<form:form action="processForm" **modelAttribute**="customer">
	First name: <**form:input path**="firstName" />
...
```
2. in controller: 
```
	@RequestMapping("/processForm")
	public String processForm(@ModelAttribute("customer") Customer theCustomer, Model model) {
```
3. Field is set on submit (i.e. setFirstName()). Then, model can even be passed to view page, which is accessed by **${customer.firstName}**
4. Drop-down list is 
```
	<form:select path="">
``` 
and options are 
```
	<form:option value="code" label="display" />
``` 
5. Drop-down list can be populated with LinkedHashMap (or can be any Collection? not sure) in model:
	* in jsp, 
	```
		<form:options items="${customer.countryOptions}" />
	```
	* in model class, add getCountryOptions()
6. Radio button is 
```
	<form:radiobutton path="" value="" />
```
Radio button can also be populated in similar way as drop-down list.
7. Check box is 
```
	<form:checkbox path="operatingSystems" value="" />
```
Check box value in java is array. To access check box in jsp, add
```
	<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```
in header, and
```
	<ul>
		<c: forEach var="temp" items="${customer.operatingSystems}">
			<li> ${temp} </li>
		</c:forEach>
	</ul>
```


### References
1. [Inversion of Control](https://msdn.microsoft.com/en-us/library/ff921087.aspx)
2. [How not to do dependency injection - the static or singleton container](https://www.devtrends.co.uk/blog/how-not-to-do-dependency-injection-the-static-or-singleton-container)
3. [Spring & Hibernate for Beginners](https://www.udemy.com/spring-hibernate-tutorial/learn/v4/content)