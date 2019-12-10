---
title: Primitive Obsession
description: 
tags: functional-principles-khorikov
---


### Primitive Obsession

Primitive obsession is an anti-pattern that stands for using primitive types for domain modeling.

Drawbacks to passing in a primitive type:

##### Dishonest functions

```csharp
function User CreateUser(string email){
   //if invalid email string, throw an exception.   
}
```
Since we throw exceptions for strings that do not meet conditions for a valid email, this makes this function dishonest. 

##### Code duplication

If we pass an email as a string somewhere else, we would have to perform the same checks which lead to code duplication. 

Also, it leads to *defensive programming* where you check the validity of a primitive type which is a code smell.  For example, checking the validity of an email inside a User class. It's not a responsibility of a User to make sure that an email is correct. 


#### Solution:
*Value objects*

To get rid of primitive obsession, introduce a separate class (a Value Object) for each concept in your codebase.

