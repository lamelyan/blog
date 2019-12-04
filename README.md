## Functional programming concepts

### Functions are

- *Honest* return the same result for same input.  *Method signature honesty* guarantees that for a given number of inputs, you'll get specific possible outputs. There are no surprises, exceptions, etc.

- *Referentially transparent*  do not update global state. 


### Advantage of functions

- *Easier to reason about.* 

   Because of two traits of *method signature honesty* and *referential transparency* it is eaiser to reason about the code. We don't have to look up documentation or implementation detail of a function. The signature tells us what will happen after we call a function. Unit tests become easier. No need to create mocks or test doubles. 

- *Composition.*

    When we know that a function does exactly what it suppose to without updating state, we can treat it as a building block.  This lends the functions to composition. 


### Immutable Architecture

Volcabulary:

- *Immutability* inability to change data

- *State* data that changes over time

- *Side effect* a change that is maade to some state

Stateless vs Statefull classes

Methods of a stateful class can leave a side-effect by changing object's state.  This is a *mutable operation* and it makes your code *dishounest*. 


Stateless class is immutable. It's methods do not leave side-effects on global state. 

### Temporal Coupling

When the order of function calls matters it is considered a temporarl coupling.  Usually, this happens because functions update object's state via object properties. For example:

```
// Examplel of temporal coupling.
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

Now, `CreateCustomer` function cannot be called before the fucntion that gets us the Address. 

Making method signatures hontest automatically makes for an immutable class.









