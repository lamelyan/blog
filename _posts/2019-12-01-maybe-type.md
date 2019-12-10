---
layout: post
title: Avoiding Nulls with the Maybe Type
description: 
tags: functional-principles-khorikov
---


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

