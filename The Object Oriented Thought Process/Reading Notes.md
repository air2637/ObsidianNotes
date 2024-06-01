
- When comes to OO solution:
	- Don't get hung up in trying to do a perfect design at the first time, but instead **let your thought process go in different directions**
	- Don't try to conform to any standards/conventions, because **the whole idea is to be creative**
	- In fact, at the start, **don't event consider a specific programming, as the first order of business is to identify and solve the business problems** - work on conceptual analysis and design first!
	- *The author says it is important to remember while I still can't fully comprehend*:
		- knowing the difference between interface and implementation
			- decide what user needs to know (user here means the program designer, developer, not the end user)
			- more importantly to decide what user needs **not** to know, e.g. a change in implementation should not need a change in interface
		- think more abstractly - taking taxi to airport, rather than telling the taxi driver on each of the turns, directions to airport
		- give the user minimal interface possible, leverage on UML use cases diagram

- interface vs abstract
	- interface is for behaviour inheritance
	- abstract class is for implementation inheritance, as it may provide both interfaces and implementations 


##### Object lifecycle

- talk about when an object is born, is alive and is died
- Default constructor
	- the implicit constructor given, if you don't write one explicitly
	- it call the constructor of its parent class (*so it you write a constructor on you own, the `super()` is not called? -> **False***):
		- the constructor of the class's superclass is called
		- all instance variables are initialised
		- the rest code in constructor is executed

```java
public Cabbie() {
	super();
}
```

- it is always a good practice to **write your own constructor in which set the initiate the attribute value of the class** - safe state
- Some frameworks DI pattern (Inversion of Control) may less favour for classes with multiple constructors - say https://blogs.cuttingedge.it/steven/posts/2013/di-anti-pattern-multiple-constructors/
- About destructor - .NET, java reclaim memory automatically via a garbage collection mechanism


##### Error handling

- Ignore the problem - not a good idea!
	- the application will end up either being crashed or running in a unstable state
- check for the problem and abort the application
	- application can be gracefully terminated
- check for the problem and catch the mistake and attempt to fix this mistake
- throw an exception

##### The importance of Scope

if scope of attribute / methods is properly declared, they may be shared by all objects instantiated from the same class - thus sharing the memory allocated for the attributes/ methods

On the other hand, not giving other classes the excessive access to attributes of your class, as to avoid the case they modify the value in an unintended manner - this leads you to search & analyse which class is the one modified your attribute during debug and it should be avoided!

- local attribute
- instance attribute
- class attribute - be mindful about the synchronisation issue e.g. say one instance using the class attribute to count number of sheep, and another instance use the same attribute to count number of dogs

#### OO Solid Design Process


##### Guidelines / Principles of OO design

1. know the difference between interface and implementation
2. think more abstractly
3. give minimal interface

So that a change in implementation should not require a change to the user's code.





##### Gather the requirement of work

Statement of Work - A detailed documentation of what your system is trying to achieve, the feel and look of the system

##### Facilitate the understanding with a Prototype

The prototype could be a simple UI, or even just whiteboard drawing, and this process should help the next stage - identifying and classes

##### Identify the Classes

Be mindful on the Nouns identified in the Prototyping, and they are likely to be the Classes in the system. 

Also keep in mind this is an iterative processes, so work something out first and check if it fulfills the requirements.

The class should be stated with:
- scope, responsibilities
- interactions with other classes

##### Model the system with UML

UML is just one tool for modelling. Modelling helps clearly specified the classes defined and their interactions.

##### Prototyping UI in Code

Should be done by a tool for rapid prototyping

At this step, it is just **facades** with no business logic involved.
####  Inheritance and Composition

- Is-a relationship means Inheritance, but you need to consider of the level of **generalization** vs **specification** of each class along the inheritance tree

- has-a relationship fits for Composition 


#### Design Patterns

3 categories - 
- Creational Patterns that creates object for you, rather than having you instantiate the instances directly.
- Structure Pattern that composes group of objects into a larger structure
- Behaviour Patterns that defines the communications between objects


