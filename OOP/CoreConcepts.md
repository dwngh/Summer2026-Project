# Core Concepts of OOP

## Definition
* **Object Oriented Programming** is the programming paradigm that organizes code into objects which are instances of classes. The object contain attributes and methods to provide logic or support code for the given logic unit.

* A **class** is a blueprint or a template that defines the structure and behavior of the object. It serves as a user defined data type and encapsulates data (attributes) and methods (functions) that operate on that data.

* An **object** is an instance of a class. It represents real world entity and can hold its own state (attributes) and behavior (methods) defined by the class

## Principles

There are four main principle of OOP:
* **Inheritance**: Is a mechanism where one class (sublass or derived class) accquires properties and methods from another class (Parent class or base class). It promotes code usability and establish an "is-a" relationship between classes. *Type of Inheritance: Single, Multiple, Multilevel, Hierarchical*.

* **Encapsulation**: Is the bundling of data and method that operate on the data within a single unit. It restrict access to the data by providing public interface to interact with the object state. Encapsulation help hiding data and protecting the internal state of an object.

* **Polymorphism**: Allows objects of different classes to be treated as objects of a common superclass. It enables a single interface to represent multiple types of objects. Polymorphism is achieved through method overloading (compile-time polymorphism) and method overriding (runtime polymorphism).

* **Abstraction**: Is the process of hiding implementation details of an object and exposing only relevant behaviours or features. It allow developers to work with high-level concepts and ignore low-level complexity.

Recently, there is a princple called **"Composition over inheritance"**, which suggests building complex objects by combining multiple simple ones instead of relying on deep inheritance hierarchies. Different from Inheritance, which is "is-a" relationship, Composition is a "has-a".

Beside of four main principles, there are also five design principles in OOP, which known as SOLID:
* **Single Responbility Principle**: A class should have one and only one reason to change, meaning it should have one job or responbility.
* **Open/Close Principle**: Software Entities should open for extension but closed for modification, allow behaviour to change without altering the source code. 
* **Liskov Substitution Principle**: Objects of a superclass should be replaceable with objects of its subclasses without breaking the application. 
* **Interface Segregation Principle**: Clients should not be forced to depend on the interface they do not use; many specific interfaces are better than one-general purpose interface.
* **Dependency Inversion Principle**: High-level modules should not depend on low-level modules, both of them should depend on abstraction, and abstraction should not depend on detail.
## Programming Concepts



