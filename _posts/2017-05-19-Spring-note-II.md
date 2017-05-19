---
layout: post
title: "Spring Notes II"
date: 2017-05-19
comments: true
tags: [Notes]
---

<div class="post-teaser"> Spring! Spring! Spring! </div>
<!-- more -->

<hr/>

### Spring MVC Form Validation <sup>[sec 15-16]</sup>

1. Add JARs in Hibernate Validator (into MVC projects)
2. In Customer class, add annotation to field
```
	@NotNull(message="is required")
	@Size(min=1, message="is required")
	private String lastName;
```
3. In JSP, add tag to field
```
	Last name: <form:input path="lastName" />
	<form:errors path="lastName" cssClass="error_mssg" />
```
4. In controller, modify method signature
```
	@RequestMapping("/processForm")
	public String processForm(@Valid @ModelAttribute("customer") Customer theCustomer, 
		BindingResult theBindingResult) {

		if (theBindingResult.hasErrors()) {
			return "customer-form";
		} else {
			return "customer-confirmation";
		}
```
5. To pre-process model (i.e. to trim white spaces), can use initBinder method in controller:
```
	@InitBinder
	public void initBinder(WebDataBinder dataBinder) {
		StringTrimmerEditor stringTrimmerEditor = new StringTrimmerEditor(true);
		dataBinder.registerCustomEditor(String.class, stringTrimmerEditor);
	}
```
6. To check number in range, add
```
	@Min(value=0, message="must be > 0")
	@Max(value=10, message="must be < 10")
	private int freePasses;
```
To make it required, change it as (int needs to be Integer)
```
	@NotNull(message="is required")
	@Min(value=0, message="must be > 0")
	@Max(value=10, message="must be < 10")
	private Integer freePasses;
```
7. To check regular expression, add
```
	@Pattern(regexp="^[a-zA-Z0-9]{5}", message="only 5 chars")
	private String postalCode;
```
8. To check String in int input field:
	* Create custom error message **src/resources/messages.properties**, add
	```
		typeMismatch.customer.freePasses=Invalid number
	```
	which is in format _Error-type.Model-attribute-name.Field-name=Custom-message_
	* Load custom message resource in Spring config file **WebContent/WEB-INF/spring-mvc-demo-servlet.xml**, add
	```
		<bean id="messageSource"
			class="org.springframework.context.support.ResourceBundleMessageSource">
			<property name="basenames" value="resources/messages" />
		</bean>
	```

### Custom Java Annotation <sup>[sec 17]</sup>

1. (optional) Create new package
2. Create annotation class
```
	@Constraint(validatedBy = CourseCodeConstraintValidator.class)
	@Target({ElementType.METHOD, ElementType.FIELD})
	@Retention(RetentionPolicy.RUNTIME)
	public @interface CourseCode {

		// define default course code
		public String value() default "LUV";
		
		// define default error message
		public String message() default "must start with LUV";
		
		// define default groups
		public Class<?>[] groups() default {};
		
		// define default payload
		public Class<? extends Payload>[] payload() default {};
	}
```
where Payload(_javax.validation_) means custom details about validation failure (severity, error code).
3. Create constraint validator class
```
	public class CourseCodeConstraintValidator implements ConstraintValidator<CourseCode, String> {

		private String coursePrefix;
		
		@Override
		public void initialize(CourseCode theCourseCode) {
			coursePrefix = theCourseCode.value();
		}

		@Override
		public boolean isValid(String theCode, 
							ConstraintValidatorContext theConstraintValidatorContext) {
			
			boolean result = true;
			if (theCode != null) result = theCode.startsWith(coursePrefix);
			return result;
		}

	}
```
4. In Model class, use annotation as
```
	@CourseCode(value="TOPS", message="must start with TOPS")
	private String courseCode;
```

### Hibernate <sup>[sec 18-21]</sup>

1. Add Hibernate ORM required folder JARs to lib
2. Add MySQL connector JAR to lib
3. (optional) To test JDBC connection, create main Java code
```
	public static void main(String[] args) {
		String jdbcUrl = "jdbc:mysql://localhost:3306/table_name?useSSL=false";
		String user = "user";
		String pwd = "pwd";
		
		try {
			Connection myConn =
					DriverManager.getConnection(jdbcUrl, user, pwd);
			System.out.println("Successfully connect!!");
		} catch (Exception exc) {
			exc.printStackTrace();
		}

	}
```
4. Add config file _src/hibernate.cfg.xml_
5. Create entity class Student
```
	@Entity
	@Table(name="student")
	public class Student {

		@Id
		@GeneratedValue(strategy=GenerationType.IDENTITY)
		@Column(name="id")
		private int id;
		
		@Column(name="first_name")
		private String firstName;
	...
```
in which we:
	* Map class to database table
	* Map field to database column
	* Add no-arg constructor, toString(), getter/setter methods
6. SessionFactory: 
	* Reads the hibernate config file 
	* Creates Session objects 
	* Heavy-weight object
	* Only create once in your app
7. Session:
	* Wraps a JDBC connection
	* Main object used to save/retrieve objects  
	* Short-lived object
	* Retrieved from SessionFactory
8. A typical Hibernate code (Create)
```
	public static void main(String[] args) {
		SessionFactory factory = new Configuration()
									.configure("hibernate.cfg.xml")
									.addAnnotatedClass(Student.class)
									.buildSessionFactory();
		
		Session session = factory.getCurrentSession();
		
		try {
			Student tmpStudent = new Student("Paul", "Wall", "abc@com");
			session.beginTransaction();
			session.save(tmpStudent);
			session.getTransaction().commit();
			
			System.out.println("Done");
		} finally {
			factory.close();
		}

	}
```
9. Generation types:
	* AUTO: Pick an appropriate strategy for the particular database
	* IDENTITY: Assign primary keys using database identity column (commonly used for auto-increment)
	* SEQUENCE: Assign primary keys using a database sequence
 	* TABLE: Assign primary keys using an underlying database table to ensure uniqueness
10. We can also define custom generation strategy by:
	* Create subclass of **org.hibernate.id.SequenceGenerator**
	* Override the method: public Serializable **generate()**
11. Read (and update)
```
	Student myStudent = session.get(Student.class, tmpStudentID);
	myStudent.setFirstName("Scooby");

```
12. Query:
```
	List<Student> theStudents = session.createQuery("from Student s where s.lastName='Doe'").getResultList();
			
	for (Student tmpStudent : theStudents) {
		System.out.println(tmpStudent);
	}
```
13. Update all:
```
	session.createQuery("update Student set email='foo@gmail.com'").executeUpdate();
```
14. Delete:
```
	session.delete(myStudent);
```
or delete with query
```
	session.createQuery("delete from Student where id=2").executeUpdate();
```

### Spring MVC with Hibernate <sup>[sec 22-27]</sup>

1. Connect to db.
2. Add config file (_spring-mvc-crud-demo-servlet.xml_ and _web.xml_) in WEB_INF.
3. Add JARs in optional/c3p0 in Hibernate to lib (for db connection pooling).
4. Create Entity class Customer.
5. Create DAO class DAOImpl (add **@Repository** annotation before class, and **@Transactional** before CRUD methods):
	* @Repository: mark as DAO implementation in component-scan (compared with @Controller for Spring MVC)
	* @Transactional: automatically begin and end a transaction (but still need to get session)
6. Add static CSS in WebContent/resources/CSS. then in jsp header, add it as
```
	<link type="text/css" rel="stylesheet" 
		href="${pageContext.request.contextPath}/resources/css/style.css" />
```
7. For GET mapping, either:
	* **@RequestMapping(path="/processForm", method=RequestMethod.GET)**, or
	* **@GetMapping("/processForm")** (after Spring 4.3)
8. Service layer: normally between Controller and DAO, annotated as **@Service**.
9. Button in jsp to add customer:
```
	<input type="button" value="Add Customer"
	   onclick="window.location.href='showFormForAdd'; return false";
	   class="add-button"
	/>
```
10. To redirect to controller (i.e. to /list, instead of only view):
```
	return "redirect:/customer/list";
```
11. For update link, add in jsp:
```
	<c:url var="updateLink" value="/customer/showFormForUpdate">
	<c:param name="customerId" value="${tempCustomer.id}" />
```
12. If combine save/insert and update, in DAOImpl we should use **session.saveOrUpdate()**.
13. To add params in query:
```
	Query<Customer> theQuery = 
			currentSession.createQuery("delete from Customer where id=:customerId");
	theQuery.setParameter("customerId", theId);
```

### Aspect Oriented Programming <sup>[sec 22-27]</sup>

1. It can solve problems of 
	* code tangling (tangle different irrevelant logic together) and 
	* code scattering (scatter similar code in many classes)
2. Aspect encapsulates cross-cutting logic/concern. AOP can be used for logging, security, transaction, API management and exception handling.
3. Spring AOP vs. AspectJ: both support AOP; former is lighter, but only method level; latter's performance is better, and can be applied on constructor, method and field, but can be complex.
4. AOP advice type:
	* Before Advice
	* After finally Advice
	* After returning Advice
	* After throwing Advice
	* Around Advice
5. AOP weaving type:
	* compile-time
	* load-time
	* run-time
6. To add a before service:
	* add aspectjweaver JAR to lib (besides Spring. Although using Spring AOP, it relies some parts of AspectJ)
	* Create business logic/method (i.e. AccountDAO)
	* Create config class
	```
		@Configuration
		@EnableAspectJAutoProxy
		@ComponentScan("com.cxcollie.aopdemo")
		public class DemoConfig {...}
	```
	* Create aspect class (assign Pointcut event)
	```
		@Aspect
		@Component
		public class MyDemoLoggingAspect {

			@Before("execution(public void addAccount())")
			public void beforeAccountAdvice() {
				System.out.println("Doing cool stuff before some methods");
			}
		}
	```
	* In main Java code, get bean and excute bean
7. Pointcut execution format/language:
```
	execution(modifiers-pattern? return-type-pattern declaring-type-pattern? 
		method-name-pattern(param-pattern) throw-pattern?)
```
	* modifiers-pattern: Spring AOP only supports public or *
	* return-type-pattern: return type of method
	* declaring-type-pattern: the class name of given method
	* pattern is optional if it has '?'
	* pattern can use wildcard (for param pattern, () means no param, (*) means one arg with any type, (..) means 0 or more arg with any type)
	* in param pattern, must use full qualified class name

Alright! Next step is to refactor UIDrive with Spring!

### References

1. [Spring & Hibernate for Beginners](https://www.udemy.com/spring-hibernate-tutorial/learn/v4/content)