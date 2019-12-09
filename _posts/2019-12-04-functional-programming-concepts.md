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


### Avoiding Nulls with the Maybe Type

Nulls introduce dishonesty to code. 

```csharp
public Employee GetEmployee(int id); // <- this may return an Employee or a null
```

If we agree to use the Maybe convention, then we can *honestly* say:

```csharp
public Organization GetById(int id); // This method guarantees we get an Organization

public Maybe<Employee> GetEmployee(int id); // This method does not guarantee we get an Employee
```

Here is an implementation of a Maybe struct

```csharp
public struct Maybe<T>
{
    private readonly T _value;
 
    public T Value
    {
        get
        {
            if(HasNoValue)
               throw new InvalidOperationException();
 
            return _value;
        }
    }
    
    public bool HasValue => _value != null; 
    public bool HasNoValue => !HasValue;   
 
    // The constructor is private. You'll never need to instantiate this value manually
    private Maybe(T value)
    {
        _value = value;
    }
 
    // Using implicit operator, you will able to do: 
    //    Maybe<string> nullableString = "abc";
    //    Maybe<string> nullableString = null; 
    public static implicit operator Maybe<T>(T value)
    {
        return new Maybe<T>(value);
    }
}
```


Consider using [https://github.com/Fody/NullGuard](https://github.com/Fody/NullGuard)   to remove all the defensive programming checks. Fody will emit the checks in the compiled code for you. The exceptions will happen close to where the object is null, making it easier to debug problems. 

Do not use this with WPF or ASP.NET applications. Use the Foddy NullGuard with Domain Logic assembly.

Make sure that Domain Model classes do not work with null references directly. They need to accept Maybe type instances.


#### Railway-oriented Programming

Handling failures and input errors in a functional way. 

Problem: Core logic is hidden by orchestration/boilerplate code (validation, error handling, logging, etc).

This results with the code that is hard to read. Example:
```csharp
public string RefillBalance(int customerId, decimal moneyAmount) {
	if (!IsMoneyAmountValid(moneyAmount)) {
		_logger.Log("Money amount is invalid");
		return "Money amount is invalid";
	}
	Customer customer = _database.GetById(customerId);
	if (customer == null) {
		_logger.Log("Customer is not found");
		return "Customer is not found";
	}
	customer.Balance += moneyAmount;
	try {
		_paymentGateway.ChargePayment(customer.BillingInfo, moneyAmount);
	}
	catch (ChargeFailedException) {
		_logger.Log("Unable to charge the credit card");
		return "Unable to charge the credit card";
	}
	try {
		_database.Save(customer);
	}
	catch (SqlException) {
		_paymentGateway.RollbackLastTransaction();
		_logger.Log("Unable to connect to the database");
		return "Unable to connect to the database";
	}
	_logger.Log("OK");
	return "OK";
}
```

Chaining method calls we could have something more succinct.

```csharp
public string RefillBalance(int customerId, decimal moneyAmount)
{
	Result<MoneyToCharge> moneyToCharge = MoneyToCharge.Create(moneyAmount);
	Result<Customer> customer = _database.GetById(customerId).ToResult("Customer is not found");
	
	return Result.Combine(moneyToCharge, customer)
		.OnSuccess(() => customer.Value.AddBalance(moneyToCharge.Value))
		.OnSuccess(() => _paymentGateway.ChargePayment(customer.Value.BillingInfo, moneyToCharge.Value))
		.OnSuccess(() => _database.Save(customer.Value)
				.OnFailure(() => _paymentGateway.RollbackLastTransaction()))
		.OnBoth(result => Log(result))
		.OnBoth(result => result.IsSuccess ? "OK" : result.Error);
}
```

Key changes to make this work:

1. Move validation checks into Value objects. So `MoneyToCharge` in this case.
2. Universalize the result type. Replace return type of `Customer` with `Result<Custeromer>` 
3. Add the `Combine` method to `Result` class
```csharp
	 public static Result Combine(params Result[] results)
     {
         foreach (Result result in results)
         {
             if (result.IsFailure)
                 return result;
         }

         return Ok();
     }
```
4. Create `ResultExtensions` class that implements OnSuccess, OnError functions. Notice that these methods accept `Result` object as first param and `Action` or `Func` delegate as second param. This allows us to execute the methods and return results.
```csharp
public static class ResultExtensions
    {
        public static Result<T> ToResult<T>(this Maybe<T> maybe, string errorMessage) where T : class
        {
            if (maybe.HasNoValue)
                return Result.Fail<T>(errorMessage);

            return Result.Ok(maybe.Value);
        }

		// Do not exectue the action if previous operation failed
        public static Result OnSuccess(this Result result, Action action)
        {
            if (result.IsFailure)
                return result;

            action();

            return Result.Ok();
        }

        public static Result OnSuccess(this Result result, Func<Result> func)
        {
            if (result.IsFailure)
                return result;
            
            return func();
        }

        public static Result OnFailure(this Result result, Action action)
        {
            if (result.IsFailure)
            {
                action();
            }

            return result;
        }

        public static Result OnBoth(this Result result, Action<Result> action)
        {
            action(result);

            return result;
        }

        public static T OnBoth<T>(this Result result, Func<Result, T> func)
        {
            return func(result);
        }
    }
```

