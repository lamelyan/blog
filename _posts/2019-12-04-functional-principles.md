---
title: Honest functions and referental transparency
tags: [functional-principles-khorikov]
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
