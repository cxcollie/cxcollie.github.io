---
layout: post
title: "Design Pattern Note"
date: 2017-04-30
comments: true
tags: [Notes]
---

<div class="post-teaser"> "You should design in this way, dude!" -- Gang of Four </div>
<!-- more -->

<hr/>

* [Creational Pattern](#cp)
* [Structural Pattern](#sp)
* [Behavioral Pattern](#bp)
* [J2EE Pattern](#jp)

<div id="cp">
</div>

## Creational Pattern

1. **Factory pattern**: different types of object get produced from a factory, and client asks factory to get the corresponding type of object.
2. **Abstract factory pattern**: adding one more layer on factory pattern. There is a factory of factory, and client asks for the factory first, then the object.
3. **Singleton pattern**: to ensure there is only one instance of that type, by privatizing constructor function.
4. **Builder pattern**: building a complicated object step by step with simple objects.
5. **Prototype pattern**: reuse object stored in cache. This is adopted when creating an instance is costly.

<div id="sp">
</div>

## Structural Pattern

1. **Adapter pattern**: to link the incompatible interfaces, like driver for different types.
2. **Bridege pattern**: to decouple the abstraction from the implementation, so that they can change independently.
3. **Filter pattern**: to apply filter criteria to some objects, with ease of logical operation like AND and OR.
4. **Composite pattern**: to build a tree-structure relationship among objects.
5. **Decorator pattern**: to add some new functionality to the object without modifying it.
6. **Facade pattern**: to hide some complex logic from user, so user can use them thru the object interface.
7. **Flyweight pattern**: similar with prototype, it stores previous object in cache for reuse.
8. **Proxy pattern**: a proxy object that acts in place of another object.

<div id="bp">
</div>

## Behavioral Pattern

1. **Chain of responsibility pattern**: build a chain of responsibility, so that it gets passed around.
2. **Command pattern**: a request is wrapped under a command object, and an invoker is responsible to handle the command by passing it to corresponding object that can execute the command.
3. **Interpreter pattern**: it can interpret the particular context, and is commonly used in SQL parsing and symbol processing engine.
4. **Iterator pattern**: it provides user the access, which is often an iterator, to a collection without knowing its underlying logic.
5. **Mediator pattern**: it collaborates different objects and acts as a mediator among them.
6. **Memento pattern**: there are three actor classes here: memento, originator and caretaker. Memento is storing the state, originator produce new memento, and caretaker stores the previous mementos.
7. **Observer pattern**: it is used when there is a one-to-many relationship. The dependent objects change automatically when the observed object changes.
8. **State pattern**: it is used when an object's behavior changes based on its state. The state object stores the state info, and the context object updates its state variable.
9. **NULL object pattern**: it replaces the behavior of null object, and acts as a default behavior when data is not available.
10. **Strategy pattern**: an object's functionality can change based on its strategy variable, which is similar with state pattern.
11. **Template pattern**: the interface class defines the overall structure or procedure, then its concrete class fill them up in detail.
12. **Visitor pattern**: an object class's logic changes as and when the visitor changes.

<div id="jp">
</div>

## J2EE Pattern

1. **MVC pattern**: it stands for Model-View-Controller. Model stores the data representation; view is responsible for visualization of model data; and controller manages both model and view.
2. **Business delegate pattern**: it decouples presentation tier and business tier. Client acts as presentation interface; business delegate is the single entry point for client to access business logic; business service acts as actual business implementation logic; and lookup service fetch corresponding business service based on the request.
3. **Composite entity pattern**: it is an EJB entity bean which represents the interal dependency of objects. When a composite entity object is updated, its dependent objects can also get updated. Composite entity is the primary entity bean, which stores coarse-grained objects; coarse-grained object contains dependent objects; dependent object depends on the coarse-grained object in its lifecycle.
4. **Data access object pattern**: usually known as DAO, it provides an abstraction on data manipulation from actual data storage logic. DAO interface enables standard operation on model; DAO concrete class implements actual logic for storage mechanism, like access SQL; model/value can be simple POJO object.
5. **Front controller pattern**: to enable single handler to dispatch request to different handler based on the request nature. Front controller is a single handler for all request, and contains the dispatcher object; dispatcher can dispatch specific request to corresponding handler; view is the object to represent the request.
6. **Intercepting filter pattern**: to do some pre-processing/post-processing of the request/response of the application. Filter implements the actual filter logic; filter chain defines chain/order of filters; target is the request handler; filter manager manages filter chain and filter.
7. **Service locator pattern**: it caches service for reuse. Service is the actual service logic; context/initial context creates new service as a service factory; cache stores the previous created service; service locator lookup service in cache or creates a new one.
8. **Transfer object pattern**: to pass data with different attributes in one shot between client and server. Business object stores multiple transfer object; transfer object is a POJO object that stores data object.

### References
1. [Design Pattern Tutorial](https://www.tutorialspoint.com/design_pattern/index.htm)