## Functional programming concepts

### Function are

- *Honest* return the same result for same input.  *Method signature honesty* guarantees that for a given number of inputs, you'll get specific possible outputs. There are no surprises, exceptions, etc.

- *Referentially transparent*  do not update state. 


### Advantage of functional 

- *Code complexity.* 

Because of two traits of *method signature honesty* and *referential transparency* it is eaiser to reason about the code.

- *Composition.*

When we know that a function does exactly what it suppose to without updating state, we can treat it as a building block.  This lends the functions to composition. 
