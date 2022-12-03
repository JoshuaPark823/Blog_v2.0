---
title: "The Foundations of Object-Oriented Design (Encapsulation, Abstraction, Inheritance, and Polymorphism)"
date: 2022-12-03
draft: false
tags: [
    'Software Design',
    'Object-Oriented Programming (OOP)'
]
---

# Introduction

We all want to write better code. Code that's easily readable, highly scalable, and performant. Code that can stand the test of time and continually provide value for end-users long after its author has left. But such code isn't developed with short term motives that account only for the near future. 
In order to deliver software systems as robust as we've described, we need to look further ahead and anticipate what challenges and setbacks other developers may face when inevitably working with our code. Gaining this ability takes time, practice, and the prerequisite knowledge of how such systems are designed. 

Object-Oriented design has become a somewhat "de-facto" standard of enterprise software systems over the decades. And although it isn't the be-all-end-all of software design, it's a great place to begin learning some of the tried and true components that lead to well-designed software.

In this post, we introduce some of the foundations of OOP assuming some familiarity with basic programming concepts.

# Encapsulation

The idea behind Encapsulation is to enclose data, logic, and computation, within a "capsule". An example would be if you had a nut enclosed within a shell. The purpose of the shell (and capsule) is to protect its inner contents from being accessed by potentially unwanted external factors. In software design, the shell is generally represented through an **interface** and the concept of Encapsulation is tied strongly to **information hiding**. Creating encapsulated structures that expose only the information required to use them while hiding the rest results in helpful abstractions that reduce complexity for client code.

* Suggested Further Reading: Reference Sharing, Escaping References, Immutable Wrappers, Reference Copying, Copy Constructors

# Abstraction

Abstraction is a concept that follows very naturally from Encapsulation. Mentioned above, it involves making encapsulated structures expose only the **minimum amount of information** necessary while hiding the rest of the logic. Well encapsulated abstractions have several benefits: code is more easily understandable in isolation, isolated components are less error-prone (and more easily testable), and it becomes easier to change certain parts of the system without impacting others.

#### Example 1
It's a Monday morning and you're headed to the office. You get in your car knowing how to fasten the seat belt, turn on the vehicle, and move in the direction you wish to travel (minimum required information). In order to use the car, you don't need to understand the inner workings of the engine or the mechanics under the hood which represents the code hidden from the client (caller).

#### Example 2
Suppose you're working with an ADT such as the `Stack`. Taking advantage of the data structure requires only working knowledge of the `push()` and `pop()` methods, among others, without knowing how the `Stack` is allocating memory within its own class.

* Suggested Further Reading: Loose Coupling, Separation of Concerns, Interface Segregation

# Inheritance

Inheritance is a mechanism of object-oriented languages that is incredibly useful for supporting code reuse. Although we won't cover any of the literature on the downfalls of duplicate code, the bottom line is that it's good to avoid it. Inheritance allows us to define classes with respect to other classes. It allows us to define **sub classes** that adds to / extends an existing **base class** (known as the superclass). Declarations from the super class are shared with sub classes which prevents repeated declarations of similar logic.

* Suggested Further Reading: Subtyping, Downcasting, Method Overriding & Overloading, Liskov Substitution Principle

# Polymorphism

Polymorphism is the ability to have different "shapes" or define different behaviours for concrete classes in subtype relations. This can either be the relation between a concrete class implementing an interface or a concrete class inheriting shared behaviour from a super class.

#### Example 1
Suppose you have an interface `IAnimal` that declares a `talk()` method. Multiple concrete classes implementing the interface such as the classes `Dog` and `Cat` can define their own `talk()` behaviours that either bark, meow, or make some other noise.

#### Example 2
Suppose you have a base class `Cat` that defines the methods `meow()` and `eat()` and `run()`. You're building a zoo and would like to implement a subclass `Tiger` that extends `Cat`, sharing the common behaviour. However, the `Tiger` class adds new functionality `track()` and `hunt()` giving the sub class extended functionality while maintaining the inherited declarations from `Cat`.

* Suggested Further Reading: Prototypes & Cloning, Aggregation, Delegation

# References

* Robillard, M. P. (2019). Introduction to software design with Java. Springer International Publishing. 
* Martin, R. C., Feathers, M. C., Ottinger, T. R., &amp; Langr, J. J. (2016). Clean code A handbook of agile software craftsmanship. Pearson Education, Inc. 
