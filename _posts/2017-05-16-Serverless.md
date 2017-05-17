---
layout: post
title: "Serverless and Lambda Expression"
date: 2017-05-16
comments: true
tags: [Notes]
---

<div class="post-teaser"> Welcome to the βΔλ fraternity! </div>
<!-- more -->

<hr/>

### Serverless

1. <a href="https://martinfowler.com/articles/serverless.html">Serverless architecture</a> refers to utilizing the third-party services (like AWS Lambda) or ephermal containers<sup>[1]</sup>. It is usually linked with PaaS, API Gateway, and Lambda Expression. It doesn't mean 'no server in backend', but freedom from worrying about operating, monitoring and scaling servers. An example of web application using serverless architecture is shown as below.

<div style="text-align: center">
<img style="width: 75%" src ="{{site.url}}/images/2017-05/Lambda_WebApplications.png" />
<p class='imageNotation'>Serverless web application using AWS.<sup>[2]</sup></p>
</div>

2. #NoOps: Serverless does not mean freedom from worrying about operation work. <a href="">Outsourcing servers does not mean oursourcing responsibility</a href="https://www.infoq.com/presentations/bustle-serverless"><sup>[2]</sup>. We still need to think about benchmark, load testing, resilency and administration. Sometimes more works are needed as the tools of serverless are new. So it's better to say: #LessOps.
3. The major advantages are usage of cloud. We can rely on the experts to scale the servers for us automatically. The operation cost is less (pay much as we use, like number of Lambda called, instead of running a EC2 all day).
4. The disadvantages include:
	* Lock-in effect: we may rely more on single cloud vendor.
	* Harder to test: so we need to figure out what's going on in the black box if we want good performance.

### Lambda Expressions

1. Lambda expressions are commonly used in serverless architecture. It enables developers to write anonymous class in a much more readable way. In Java 8, a typical lambda expression looks like this (<a href="https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html">detailed implementation</a><sup>[4]</sup>):
```
Collections.sort(personList, **(p1, p2) -> p1.firstName.compareTo(p2.firstName)**);
```
As can be seen, the anonymous class is more compact and near to the expression.
2. If the return type is same as that of expression, we can refactor code as follows:
	* Using anonymous class, it is written as:

	```
	public static void printPersons(
	    List<Person> roster, CheckPerson tester) { // CheckPerson is the anonymous class
	    for (Person p : roster) {
	        if (tester.test(p)) {
	            p.printPerson();
	        }
	    }
	}
	```
	* Without lambda expression, we need to have an anonymous 'stranger':

	```
	printPersons(
	    roster, new CheckPersonEligibleForSelectiveService());

	```
	where CheckPersonEligibleForSelectiveService() is an instance of CheckPerson interface. It is very hard to understand the code as we need to find the actual implementation of CheckPersonEligibleForSelectiveService(), and often time it is only used once and thrown away. The CheckPerson interface only has one method: test(). 
	* Then we came up with the idea of using anonymous class:

	```
	printPersons(
	    roster,
	    new CheckPerson() {
	        public boolean test(Person p) {
	            return p.getGender() == Person.Sex.MALE
	                && p.getAge() >= 18
	                && p.getAge() <= 25;
	        }
	    }
	);
	```
	* Easier to understand, but still not compact. Then we use lambda expression to refactor it as:

	```
	printPersons(
	    roster,
	    (Person p) -> p.getGender() == Person.Sex.MALE
	        && p.getAge() >= 18
	        && p.getAge() <= 25
	);
	``` 
	Perfect!
3. Lambda chaining: when there is a serial relationship between several (lambda) expressions:
```
public static <X, Y> void processElements(
    Iterable<X> source, // source
    Predicate<X> tester, // filter
    Function <X, Y> mapper, // mapper
    Consumer<Y> block) { // action
    for (X p : source) {
        if (tester.test(p)) {
            Y data = mapper.apply(p);
            block.accept(data);
        }
    }
}
```
we can use 'chaining' of lambda expressions as:
```
processElements(
    roster, // source
    p -> p.getGender() == Person.Sex.MALE
        && p.getAge() >= 18
        && p.getAge() <= 25, // filter
    p -> p.getEmailAddress(), // mapper
    email -> System.out.println(email) // action
);
```
4. An example of aggregate operation is:
```
roster
	.stream()
	.filter(
	    p -> p.getGender() == Person.Sex.MALE
	        && p.getAge() >= 18
	        && p.getAge() <= 25)
	.map(p -> p.getEmailAddress())
	.forEach(email -> System.out.println(email));
```


### References
1. [Serverless Architectures by Mike Roberts](https://martinfowler.com/articles/serverless.html)
2. [The Hitchhiker's Guide to Serverless JavaScript by Steve Faulkner](https://www.infoq.com/presentations/bustle-serverless)
3. [OPERATIONAL BEST PRACTICES #SERVERLESS by Charity.WTF](https://charity.wtf/2016/05/31/operational-best-practices-serverless/)
4. [Lambda Expressions](https://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html)