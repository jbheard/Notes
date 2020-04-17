# Design Patterns

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
+ Issues:
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
+ Example: Designing tile maps for game; rather than load image multiple times, create copies of the first instance

### Singleton
+ Only one instance of the class should ever be created
  + Hide constructors (& any default copying methods [e.g. C++])
  + Define getInstance() to return the single existing instance of the object
+ Singleton is globally accessible (but does not pollute global namespace)
  + Also allow lazy initialization (created when getInstance() is called, instead of on creation)
+ Sometimes considered an anti-pattern
+ Issues:
  + Dependencies of the Singleton are hidden
  + Singletons control their own creation & deletion; violates Single Responsibility Principle
  + Issues unit testing, since anything dependent on the singleton will be tightly coupled
  + Exists for the whole life of the application
  + Singletons are difficult to deal with in multi-threaded environments
  + Singletons break inheritance (Singletons cannot have child classes without significant changes)


## Structural Patterns

Provide simple ways to establish relationships between objects.

### Adapter
+ Allows two incompatible classes to work together by converting an incompatible interface to a compatible one
  + Analogy: HDMI to VGA adapter, power cord, other types of cords
    + Convert one set of signals to another so that a source & destination become compatible

### Bridge
+ Separates abstraction from implementation
  + Allows implementation to be selected at runtime
+ Example: Using a Logger in a class method for messages, warnings, and errors
  + 3 implementations of the Logger (children of abstract Logger); each can be chosen (or changed) during runtime

### Composite
+ Organize data/classes into a tree structure
  + Tree structured data often requires distinction between leaf nodes and branches (containers of leaf nodes)
  + Solution: Treat leaf nodes and branches in the same way
+ Example: drawing graphics; Shape extends Drawable, Image extends Drawable.
  + Add Shapes to Images, Images to Images. Then Image.draw() draws all Drawables onto itself, regardless of concrete type
+ Example: Java Swing GUI: all objects can be added to containers, including containers themselves, everything is created in layers

### Decorator
+ Add responsibilities/features to an object at runtime
+ Easily stack object declarations
  + Example: Window win = new VerticalScrollDecorator(new HorizontalScrollDecorator(new PlainWindow()));
    + Creates window with vertical and horizontal scrolling
    + Each Decorator adds new functionality to the parent, and Decorators can be stacked
    + VerticalScrollDecorator forwards all requests to HorizontalScrollDecorator which forwards to PlainWindow

### Façade
+ Provides a simple, unified interface to a set of interfaces in a system
  + Reduce complexity as much as possible for user of interface
+ Façade because it makes things seem less complex
+ Example: Memory allocation in C(++), malloc()/free()/new/delete make various system calls, but provide a nice façade that memory management is simple

### Flyweight
+ Maintain large numbers of objects efficiently & avoid creating too many objects
  + Done by storing shared information internally, and passing in any external values
+ Especially useful for immutable types
  + Can store HashMap of all created objects, then get a reference to an existing object

### Proxy
+ Acts as a wrapper for some other Subject class
  + Added extra functionality
  + Often used in a Lazy loading scheme (e.g. large data sets in memory/on hard drive)
+ Example: Remote Proxy - Create a Local object that acts as a proxy for a remote object
  + Local invokes methods of the remote object
  + e.g. PHP (remote) data through HTML forms (proxy)


## Behavioral Patterns

Increase flexibility of communication between objects.

### Chain of responsibility
+ Object oriented version of "If.. Elseif.. Else"
+ Create a chain of Receiver objects to either 
  + Handle the request, or
  + Forward to next Receiver in chain
+ Solves problems:
  + Sender & Receiver of a request need to be decoupled
  + More than one receiver needs to be able to handle a request
+ Example: Stack trace on execution error (each step could be modelled as a Receiver of the Exception)

### Command
+ Define Command objects that encapsulate a request, Command invokes an action on a Receiver
  + Requests get delegates to Command
  + Can be paired with Chain of Responsibility Pattern
+ Avoids hard-wired request
+ Should be possible to configure an object with a request

### Interpreter
+ Defines patterns/syntax for a language
+ Implementation:
  + Define an Expression class with an interpret() method
  + Create an Abstract Syntax Tree (AST) of Expression objects
  + Call interpret() on the AST to interpret a sentence of Expressions
+ Example: Regular Expression matching, programming/scripting languages, &c.

### Iterator
+ Access and iterate over aggregate objects without exposing internal representation
+ Example: C++ iterator

### Mediator
+ Defines a Mediator object that encapsulates interaction between some set of objects
+ Objects delegate interaction to a Mediator instead of interacting directly
+ Promotes loose coupling between colleague objects
+ Example: Pilots communicate with flight controller, flight controller communicates with other pilots
  + Many pilots communicating with each other would be complex and unmaintainable

### Memento
+ Provide ability to restore state of an object by storing state externally
  + Note: Externally means outside of the object, not necessarily outside of the application
+ Should not violate the encapsulation of the object
+ Example: Undo stack for hard to revert operations
  + e.g. Image editor: save image state on stack, then perform operation
    + To undo, just pop top of stack

### Observer
+ Relationship between a Subject (one) and Observers (many)
+ Subject notifies observers when some state changes
+ Used to notify/update a variable number of observers
  + Example: the View in MVC is an observer of the Model

### State
+ Allow object to change behaviour when it changes state
  + Adding new states/behaviours should not affect existing states
+ Implementation:
  + Define State object for each state, with some interface for StateBehaviour
  + Main Context object delegates state-specific behaviour to its State object
+ Example: Control robot movement based on state (e.g. move forward, slow down, stop, turn, …)

### Strategy (or Policy)
+ Allows selection of algorithm at runtime
+ Implementation:
  + Create interface Strategy with method algorithm()
  + Main object has a Strategy field so algorithm to use can be chosen at runtime
+ Example: Handling payment types (credit, debit, cash, etc.), Java Comparator

### Template method
+ An abstract class that acts as a template to define an algorithm
  + Class Template has algorithm(), and some number of abstract methods
  + Abstract methods should be called in child classes of the Template, and can be used to make changes to the algorithm
+ Example: Sorting algorithm implementation, where abstract method has comparator

### Visitor
+ Makes it possible to add new operation to an object without changing the object itself
+ Implementation:
  + Define an object Visitor that references mutable type 
+ Uses:
  + Core class is not expected to change
  + Many new operations need to be added


## Architectural Patterns

Architectural patterns govern the architecture of an application.

### Model-View-Controller
+ Model: Internal structure to manage the data of the application
  + Receives input from the controller
+ View: Representation of data
  + Usually visual
+ Controller: Receives input and passes it on to the model
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
+ Trying to fit new & interesting technologies into an application, even when they don't fit
+ How to Avoid:
  + Only introduce technologies that are absolutely necessary
    + Especially new, untested, rapidly changing technologies
  + Take the time to read and understand any new technology

### Lava Flow
+ Old code is not updated because developers are scared to touch it
  + Could use old technologies, old developers left company
  + Leads to dead code, security issues, &c
+ How to Avoid:
  + Use analysis tools to find dead code
  + Use interfaces to reorganize, isolate, and provide structure for the code

### Parallel Protectionism
+ Code becomes complicated and fragile, so it is copied and modified rather than extending the original code
+ If a bug is found in the original, now it also needs to be fixed in the new version
+ How to Avoid:
  + Look for duplicate code and employ code reuse principles
  + Avoid creating complicated or tricky code
    + Focus on readability over cleverness

### Accidental Complexity
+ Happens when system is built without overall plan
+ Can also happen when developers add code due to ego
  + "Look how good my code is"
+ How to Avoid:
  + Focus on simple designs
  + Do code reviews frequently
  + Write readable code

### Object Orgy
+ Poor encapsulation can lead to private fields being modified inappropriately
+ How to Avoid:
  + Use proper encapsulation

### The Blob
+ A code or package that does too much
  + Developer is unsure where to put code, or too lazy to make new package
+ Usually lack of object oriented design
+ How to Avoid:
  + Code reviews, pair programming
  + General Rule: If you can't describe a class's purpose with one sentence, it has too much responsibility

### Golden Hammer
+ Using the same technology to solve any problem
+ How to Avoid:
  + Be familiar with many languages and technologies and their pros/cons
  + Set aside personal preferences to choose best tool/technique for the problem

### Premature Optimization
+ "Premature optimization is the root of all evil" 
+ Optimizing before enough information is gathered about the problem
+ How to Avoid:
  + Write clean, working, readable code first
  + Use tools and find the bottlenecks after code works, optimize later

### Magic Values
+ Hardcoded values with no description used throughout code
+ Magic because it just seems to work without much description
+ How to Avoid:
  + Create constant variables with descriptive names

### Singleton 

See [Creation Patterns - Singleton][#singleton-1]
