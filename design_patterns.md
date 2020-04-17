# Design Patterns

## Index

+ [Creational Patterns](#creational-patterns)
  + [Abstract Factory](#abstract-factory)
  + [Builder](#builder)
  + [Factory](#factory)
  + [Prototype](#prototype)
  + [Singleton](#singleton)
+ [Structural Patterns](#structural-patterns)
  + [Adapter](#adapter)
  + [Bridge](#bridge)
  + [Composite](#composite)
  + [Decorator](#decorator)
  + [Façade](#facade)
  + [Flyweight](#flyweight)
  + [Proxy](#proxy)
+ [Behavioral Patterns](#behavioral-patterns)
  + [Chain of Responsibility](#chain-of-responsibility)
  + [Command](#command)
  + [Interpreter](#interpreter)
  + [Iterator](#iterator)
  + [Mediator](#mediator)
  + [Memento](#memento)
  + [Observer](#observer)
  + [State](#state)
  + [Strategy/Policy](#strategy-or-policy)
  + [Template method](#template-method)
  + [Visitor](#visitor)
+ [Architectural Patterns](#architectural-patterns)
  + [Model-View-Controller](#model-view-controller)
+ [Anti-Patterns]()
  + [Cargo-Cult Programming](#cargo-cult-programming)
  + [Lava flow](#lava-flow)
  + [Parallel Protectionism](#parallel-protectionism)
  + [Accidental Complexity](#accidental-complexity)
  + [Object Orgy](#object-orgy)
  + [The Blob](#the-blob)
  + [Golden Hammer](#golden-hammer)
  + [Premature Optimization](#premature-optimization)
  + [Magic Values](#magic-values)
  + [Singleton](#singleton-1)


## Creational Patterns

Deal with creation of objects. Goal is to create objects in a manner suitable to the application being developed. 

### Abstract Factory
+ Create related objects without worrying about implementation details or concrete type
+ Factory creates a concrete object, but returns an abstract pointer
  + This means that we can create many object of various subclasses without needing to know what class they are
  + Example: GUI elements have similar visual properties, so we can create them using a factory

### Builder
+ Separates the creation of a complex object from the representation
  + Example: Java StringBuilder class to build strings
+ Builder can be used to create different representations of the same object type

#### Issues:
+ Builder is required for every class
+ Builder must be mutable

### Factory
+ Lets subclass decide which class to instantiate
+ Abstract create() method is declared in the Factory
  + Child classes can override create() to change the type of object being created
  + Example: ShapeFactory can create a shape subclass based on input (link)

### Prototype
+ Defines an interface with a clone() signature
+ Hides complicated implementation details, instead using an object as a "prototype" and cloning it to create new objects
  + Instantiating objects can be expensive, cost effective to create copies of an existing object
  + Uses an object as a template/preset object

#### Example
Designing tile maps for a game. Rather than loading an image every time is it needed, load it once and create copies of the first instance.

### Singleton
+ Only one instance of the class should ever be created
  + Hide constructors (& any default copying methods [e.g. C++])
  + Define getInstance() to return the single existing instance of the object
+ Singleton is globally accessible (but does not pollute global namespace)
  + Also allow lazy initialization (created when getInstance() is called, instead of on creation)
+ Sometimes considered an anti-pattern

#### Issues:
+ Dependencies of the Singleton are hidden
+ Singletons control their own creation & deletion; violates Single Responsibility Principle
+ Issues unit testing, since anything dependent on the singleton will be tightly coupled
+ Exists for the whole life of the application
+ Singletons are difficult to deal with in multi-threaded environments
+ Singletons break inheritance (Singletons cannot have child classes without significant changes)


## Structural Patterns

Provide simple ways to establish relationships between objects.

### Adapter
Allows two incompatible classes to work together by converting an incompatible interface to a compatible one.  
**Analogy:** HDMI to VGA adapter, power cord, other types of cords
 
### Bridge
Separates abstraction from implementation and allows implementation to be selected at runtime

#### Example:
Using a Logger in a class method for messages, warnings, and errors  
+ 3 implementations of the Logger (children of abstract Logger); each can be chosen (or changed) during runtime

### Composite
+ Organize data/classes into a tree structure
  + Tree structured data often requires distinction between leaf nodes and branches (containers of leaf nodes)
  + Solution: Treat leaf nodes and branches in the same way

#### Example
For some graphics drawing application, create `class Shape extends Drawable`, `class Image extends Drawable`. Where `Drawable` is a base class that has an abstract `draw()` method.  
You can add Shapes to Images, and Images to Images. Then `Image.draw()` draws all Drawables onto itself, regardless of concrete type.
+ Used extensively in Java swing GUI programming

### Decorator
Add responsibilities/features to an object at runtime, makes object declarations easily stackable.

#### Example
`Window win = new VerticalScrollDecorator(new HorizontalScrollDecorator(new PlainWindow()));`  
+ Creates window with vertical and horizontal scrolling
+ Each Decorator adds new functionality to the parent, and Decorators can be stacked
+ VerticalScrollDecorator forwards all requests to HorizontalScrollDecorator which forwards to PlainWindow

### Facade
+ Provides a simple, unified interface to a set of interfaces in a system
  + Reduce complexity as much as possible for user of interface
+ Façade because it makes things *seem* less complex

#### Example
Memory allocation in C(++), malloc()/free()/new/delete make various system calls, but provide a nice façade that memory management is simple

### Flyweight
+ Maintain large numbers of objects efficiently & avoid creating too many objects
  + Done by storing shared information internally, and passing in any external values
+ Especially useful for immutable types
  + Can store HashMap of all created objects, then get a reference to an existing object

### Proxy
+ Acts as a wrapper for some other Subject class
  + Added extra functionality
  + Often used in a Lazy loading scheme (e.g. large data sets in memory/on hard drive)

#### Example
Remote Proxy - Create a Local object that acts as a proxy for a remote object.
+ Local invokes methods of the remote object
+ [websockets](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API)

## Behavioral Patterns

Increase flexibility of communication between objects.

### Chain of Responsibility
The object-oriented version of "If.. Elseif.. Else"

+ Create a chain of Receiver objects to either 
  + Handle the request, or
  + Forward to next Receiver in chain
+ Solves problems:
  + Sender & Receiver of a request need to be decoupled
  + More than one receiver needs to be able to handle a request

#### Example
Stack trace on execution error (each step could be modelled as a Receiver of the Exception)

### Command
+ Define Command objects that encapsulate a request, Command invokes an action on a Receiver
  + Requests get delegates to Command
  + Can be paired with Chain of Responsibility Pattern
+ Avoids hard-wired request
+ Should be possible to configure an object with a request

### Interpreter
Defines patterns/syntax for a language.

#### Implementation
+ Define an Expression class with an `interpret()` method
+ Create an Abstract Syntax Tree (AST) of Expression objects
+ Call `interpret()` on the AST to interpret a sentence of Expressions

#### Example
Regular Expression matching, programming/scripting languages, etc.

### Iterator
Access and iterate over aggregate objects without exposing internal representation.  
This can be particularly useful when the internals don't lend themselves well to an iterable format (e.g. a graph or tree).

#### Example
C#, C++ iterator. Python, javascript generator.

### Mediator
Defines a Mediator object that encapsulates interaction between some set of objects. Objects delegate interaction to a Mediator instead of interacting directly. This promotes loose coupling between objects.

#### Example
Pilots communicate with flight controller, flight controller communicates with other pilots. Many pilots communicating only with each other would be complex and unmaintainable.

### Memento
Provide ability to restore state of an object by storing state externally (*Externally* here means outside of the object, not necessarily outside of the application).  
Storing state should not violate the encapsulation of the object (e.g. keep copies of the object, not its internals).

#### Example
Undo stack for hard to revert operations, like CTRL+Z  
Image editor: save image state on stack, then perform operation. To undo, just pop the last image from the stack.

### Observer
Relationship between a Subject (one) and Observers (many). The subject notifies observers whenever some state changes.  
Used to notify/update a variable number of observers

#### Example
The view in [MVC](#model-view-controller) is an observer of the Model

### State
Allow object to change behaviour when it changes state.  
Adding new states/behaviours should not affect existing states

#### Implementation:
+ Define State object for each state, with some interface for StateBehaviour
+ Main Context object delegates state-specific behaviour to its State object
#### Example
Control robot movement based on state (e.g. move forward, slow down, stop, turn, …)

### Strategy (or Policy)
Allows selection of algorithm at runtime.

#### Implementation
+ Create interface Strategy with method` algorithm()`
+ Main object has a Strategy field so algorithm to use can be chosen at runtime

#### Example
Handling payment types (credit, debit, cash, etc.), Java Comparator

### Template method
An abstract class that acts as a template to define an algorithm.  
Class Template has `algorithm()`, and some number of abstract method. Abstract methods should be called in child classes of the Template, and can be used to make changes to the algorithm

### Example
Sorting algorithm implementation, where abstract method has comparator.

### Visitor
Makes it possible to add new operation to an object without changing the object itself.

#### Implementation:
Define an object `Visitor` that references the mutable type. 

#### Uses:
+ Core class is not expected to change
+ Many new operations need to be added


## Architectural Patterns

Architectural patterns govern the architecture of an application.

### Model-View-Controller
+ **Model**: Internal structure to manage the data of the application
  + Receives input from the controller
+ **View**: Representation of data
  + Usually visual
+ **Controller**: Receives input and passes it on to the model
  + Could validate data

## Anti-Patterns

Anti-patterns are patterns that add complications to a system.  
Anti-patterns are caused by:
+ Apathy
  + Developers who don't care about problem/solution
+ Haste
  + Short deadlines, small teams for large projects, &c
+ Ignorance
+ Developer does not have the experience or skills necessary

### Cargo Cult Programming
Trying to fit new & interesting technologies into an application, even when they don't fit. this often involves a new tool or language that boasts cool & useful features, but the overall cost of using the new technology would outweigh the benefits (if any). 

#### How to Avoid:
+ Only introduce technologies that are absolutely necessary
  + Especially new, untested, rapidly changing technologies
+ Take the time to read and understand any new technology

### Lava Flow
Old code is not updated because developers are scared to touch it. This can lead to dead code, security issues, etc.

#### How to Avoid:
+ Use analysis tools to find dead code
+ Use interfaces to reorganize, isolate, and provide structure for the code

### Parallel Protectionism
Code becomes complicated and fragile, so it is copied and modified rather than extending the original code. If a bug is found in the original, now it also needs to be fixed in any new version as well.

#### How to Avoid:
+ Look for duplicate code and employ code reuse principles
+ Avoid creating complicated or tricky code
+ Focus on readability over cleverness
+ Write functions or libraries when similar code is used frequently 

### Accidental Complexity
Accidental complexity happens when a system is built without overall plan. Often related to [premature optimization](#premature-optimization).

#### How to Avoid:
+ Focus on simple designs
+ Do code reviews frequently (if working in a team/group)
+ Write readable code

### Object Orgy
Poor encapsulation can lead to private fields being modified inappropriately.

**Example:** The field `secret_key` of some class is made public to make testing easier.

#### How to Avoid:
Use proper encapsulation

### The Blob
When some piece of code or package that does too much. This happens when the developer is unsure where to put code, or too lazy to make new package. It is usually a result of not following good coding practices.

#### How to Avoid:
+ Code reviews, pair programming
+ General Rule: If you can't describe a class's purpose with one sentence, it has too much responsibility

### Golden Hammer
Using the same technology to solve any problem.

#### How to Avoid:
+ Be familiar with many languages and technologies and their pros/cons
+ Set aside personal preferences to choose best tool/technique for the problem

### Premature Optimization

["Premature optimization is the root of all evil"](https://en.wikiquote.org/wiki/Donald_Knuth#Computer_Programming_as_an_Art_(1974))

Premature optimization is when code is optimized before enough information is gathered about the problem being solved. We could optimize out some functionality that is needed later, or just waste developer hours on parts of a problem that don't need optimization at all.

#### How to Avoid:
+ Write clean, working, readable code first
+ Use tools and find the bottlenecks after code works, optimize later

### Magic Values

Magic values (most well-known as [magic numbers](https://en.wikipedia.org/wiki/Magic_number_(programming)#Unnamed_numerical_constants)) are hardcoded values with no description used throughout code. They are "magic" because these values make the program work without knowing *how* they work.  
Note that it is only magic if it is unclear what the value is meant to be doing. In this example: `def metresToKm( m ): return m * 1000` the constant 1000 does not need a variable name, since it is very clear what is being done. 

#### How to Avoid:
Magic values can easily be avoided by creating constant variables with descriptive names where applicable.

### Singleton 
See [Creation Patterns - Singleton](#singleton)
