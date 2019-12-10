---
title: Immutable Architecture
tags: [functional-principles-khorikov]
---

### Immutable Architecture

Volcabulary:

- *Immutability* inability to change data

- *State* data that changes over time

- *Side effect* a change that is made to some state

Stateless vs Stateful classes

Methods of a stateful class can leave a side-effect by changing the object's state.  
This is a *mutable operation* and it makes your code *dishonest*. 


The stateless class is immutable. Its methods do not leave side-effects on the global state. 


### Immutability limitations

Note that immutability can have an impact on CPU and Memory usage. This is because we no longer reuse objects via properties and instead create new objects.

