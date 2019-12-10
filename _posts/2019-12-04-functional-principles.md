---
title: Functional Principles in C#
---

## Functional programming concepts

### Functions are

- *Honest* return the same result for the same input.  *Method signature honesty* guarantees that for a given number of inputs, you'll get specific possible outputs. There are no surprises, exceptions, etc.

- *Referentially transparent*  do not update the global state. 


### Advantage of functions

- *Easier to reason about.* 

   Because of two traits of *method signature honesty* and *referential transparency* it is easier to reason about the code. We don't have to look up documentation or implementation detail of a function. The signature tells us what will happen after we call a function. Unit tests become easier. No need to create mocks or test doubles. 

- *Composition.*

    When we know that a function does exactly what it suppose to without updating the state, we can treat it as a building block.  This lends the functions to composition. 


### Immutable Architecture

Volcabulary:

- *Immutability* inability to change data

- *State* data that changes over time

- *Side effect* a change that is made to some state

Stateless vs Stateful classes

Methods of a stateful class can leave a side-effect by changing the object's state.  This is a *mutable operation* and it makes your code *dishonest*. 


The stateless class is immutable. Its methods do not leave side-effects on the global state. 


### Immutability limitations

Note that immutability can have an impact on CPU and Memory usage. This is because we no longer reuse objects via properties and instead create new objects.


### How to deal with side effects

CQS - command-query separation principle

### Do not use exceptions to control the program flow
Guideline - always prefer using return values over exceptions.
Do not use exceptions for validation. 
An exception is used for stopping operation in case of a bug. 

*Fail Fast Principle* allows for:
Shortening the feedback loop.
Confidence in the working software.
Protects the persistence state (from data corruption).
