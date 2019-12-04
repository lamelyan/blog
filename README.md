## Functional programming concepts

### Functions are

- *Honest* return the same result for same input.  *Method signature honesty* guarantees that for a given number of inputs, you'll get specific possible outputs. There are no surprises, exceptions, etc.

- *Referentially transparent*  do not update global state. 


### Advantage of functional 

- *Easier to reason about.* 

...Because of two traits of *method signature honesty* and *referential transparency* it is eaiser to reason about the code. We don't have to look up documentation or implementation detail of a function. The signature tells us what will happen after we call a function. Unit tests become easier. No need to create mocks or test doubles. 

- *Composition.*

...When we know that a function does exactly what it suppose to without updating state, we can treat it as a building block.  This lends the functions to composition. 


### Immutable Architecture



