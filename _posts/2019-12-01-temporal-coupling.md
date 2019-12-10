---
title: Temporal Coupling
tags: [functional-principles-khorikov]
---


### Temporal Coupling

When the order of function calls matters it is considered a temporal coupling.  Usually, this happens because functions update the object's state via object properties. For example:

```csharp
// Example of temporal coupling.
// Changing the order of these calls will cause an error. 
CreateAddrss(address);
CreateCustomer(name);
Savecustomer();
```

To fix this, we need to specify all of the parameters in the signature and remove object properties. 

So instead of 

```csharp 
private Addres _address;
private Customer _customer;

private void CreateCustomer(string name){
   _customer = new Customer(name, _address);
}
```

do 

```csharp
// Lift all side-effects and dependencies to the signature level. 
private Customer CreateCustomer(string name, Address address){
   return new Customer(name, address);
}
```

Now, the `CreateCustomer` function cannot be called before the function that gets us the Address. 

Making method signatures honest automatically makes for an immutable class.


