---
title: Dependency Injection Principles, Practices, and Patterns 
description: 
tags: [dd-ppp]
---

### Overview

>DI is nothing more than a collection of design principles and patterns. It’s more about a way of thinking and designing code than it is about tools and techniques.

>Classes shouldn’t ask a third party for their Dependencies. This is an anti-pattern called Service Locator. Instead, classes should specify their required Dependencies statically using constructor parameters, a practice called Constructor Injection.

>One of the most important software design principles that enables DI is the Liskov Substitution Principle. It allows replacing one implementation of an interface with another without breaking either the client or the implementation.
[reference](https://livebook.manning.com/book/dependency-injection-principles-practices-patterns/chapter-1/point-7644-298-300-0)

### Configuration 
>Although the ability to configure a compiled application is important, only the finished application should rely on configuration files. *It’s more flexible for reusable libraries to be imperatively configurable by their callers, instead of reading configuration files themselves.* In the end, the ultimate caller is the application’s entry point. At that point, all relevant configuration data can be read from a configuration file directly at startup and fed to the underlying libraries as needed. [reference](https://livebook.manning.com/book/dependency-injection-principles-practices-patterns/chapter-2/point-7645-147-147-0)




### Composition Root
>The first time we heard about Constructor Injection, we had a hard time understanding the real benefit. Doesn’t it push the burden of controlling the Dependency onto some other class? Yes, it does — and that’s the whole point. In an _n_-layer application, you can *push that burden all the way to the top of the application into a Composition Root.*

>The Composition Root is located as close as possible to the application’s entry point. In most .NET Core application types, the entry point is the `Main` method. Inside the Composition Root, you can decide to compose your application manually — that’s using Pure DI — or to delegate it to a DI Container. [reference](https://livebook.manning.com/book/dependency-injection-principles-practices-patterns/chapter-3/point-7646-58-58-0)

### Independent domain model

>The domain model is a plain, vanilla C# library that we add to the solution. This library will contain POCOs and interfaces. The POCOs will model the domain while the interfaces provide Abstractions that will serve as our main external entry points into the domain model. _They’ll provide the contract through which the domain model interacts with the data access layer._ [reference](https://livebook.manning.com/book/dependency-injection-principles-practices-patterns/chapter-3/point-7643-64-65-0)


### Contextual information should be hidden 

Consider the `userContext` parameter in the following example.

```csharp
public class ProductService : IProductService
{
    private readonly IProductRepository repository;
    private readonly IUserContext userContext;

    public ProductService(
        IProductRepository repository,    
        IUserContext userContext)    
    {
        if (repository == null)
            throw new ArgumentNullException("repository");
        if (userContext == null)
            throw new ArgumentNullException("userContext");

        this.repository = repository;
        this.userContext = userContext;
    }

    public IEnumerable<DiscountedProduct> GetFeaturedProducts()
    {
        return
            from product in this.repository    
                .GetFeaturedProducts()    
            select product    
                .ApplyDiscountFor(this.userContext); // passing in a dependency into a method 
									                 // is called method injection   
    }
}

```
>The `IUserContext` interface allows `ProductService` to retrieve information about the current user without `HomeController` needing to provide this. `HomeController` doesn’t need to know which role(s) are authorized for a discount price, nor is it easy for `HomeController` to inadvertently enable the discount by passing, for example, `true` instead of `false`. This reduces the overall complexity of the UI layer.

>To reduce the overall complexity of a system, runtime data that describes contextual information is best hidden behind an Abstraction and injected into a consumer that requires it to function. _Contextual information_ is metadata about the current request. This is typically information that the user shouldn’t be allowed to influence directly. Examples are the user’s identity (which was established on login) and the system’s current time.


### Dependency Inversion Principle

Much of what we’re trying to accomplish with DI is related to the  Dependency Inversion Principle. This principle states that higher-level modules in our applications shouldn’t depend on lower-level modules; instead, modules of both levels should depend on  Abstractions.
![](https://dpzbhybb2pdcj.cloudfront.net/seemann2/HighResolutionFigures/figure_3-10.png)

> The `ProductService` component is part of the higher-level domain layer module, whereas the `IProductRepository` implementation — let’s call it `SqlProductRepository` — is part of the lower-level data access module. Instead of letting our `ProductService` depend on `SqlProductRepository`, we let both `ProductService` and `SqlProductRepository` depend on the `IProductRepository` Abstraction. `SqlProductRepository` implements the Abstraction, while `ProductService` uses it.


####  Inversion vs.  Injection

> The relationship between the Dependency Inversion Principle and DI is that the Dependency Inversion Principle prescribes _what_ we would like to accomplish, and DI states _how_ we would like to accomplish it. The principle doesn’t describe how a consumer should get ahold of its Dependencies. 
