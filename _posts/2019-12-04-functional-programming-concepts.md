---
layout: post
title:  "Functional programming concepts"
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

### Temporal Coupling

When the order of function calls matters it is considered a temporal coupling.  Usually, this happens because functions update the object's state via object properties. For example:

```
// Example of temporal coupling.
// Changing the order of these calls will cause an error. 
CreateAddrss(address);
CreateCustomer(name);
Savecustomer();
```

To fix this, we need to specify all of the parameters in the signature and remove object properties. 

So instead of 

``` 
private Addres _address;
private Customer _customer;

private void CreateCustomer(string name){
   _customer = new Customer(name, _address);
}
```

do 

```
// Lift all side-effects and dependencies to the signature level. 
private Customer CreateCustomer(string name, Address address){
   return new Customer(name, address);
}
```

Now, the `CreateCustomer` function cannot be called before the function that gets us the Address. 

Making method signatures honest automatically makes for an immutable class.



### Immutability limitations

Note that immutability can have an impact on CPU and Memory usage. This is because we no longer reuse objects via properties and instead create new objects.


### How to deal with side effects

CQS - command-query separation principle







