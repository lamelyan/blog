---
title: Railway-oriented Programming
description: 
tags: functional-principles-khorikov
---


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

