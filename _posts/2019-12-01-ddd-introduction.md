---
title: DDD Inroduction
description: 
tags: [ddd-khorikov]
---

### Area of Application for DDD

Every software project has a set of attributes:

- Amount of data
- Performance
- Business logic complexity
- Technical complexity

*The technics of DDD are useful if and only if the project you are working on has a lot of complex business rules.*

A typical example of such an application would be an Enterprise-level application.

### Why DDD?

Two core principles we should adhere to.  

 - YAGNI (you are not going to need it) - shortens development time
 - KISS (keep it short and simple) - keep the code maintainable

DDD practices complement these two principles. DDD breaks down a problem into consumable chunks and helps to reduce its complexity level where it's no longer hard to understand or implement. 

### Main concepts of DDD

Ubiquitous language - bridges the gap between developers and experts

Bounded context - clear boundaries between different parts of the system

Core domain - focus on the most important part of the system


### Onion Architecture

Onion architecture Is like an n-tier architecture however every layer is *isolated*.

Business logic is contained by four core elements:
- Entity
- Domain Event
- Value Object 
- Aggregate 

Entities and Value Objects

- should know the domain they represent 
- shouldn't know how they are persisted or created. That should be deferred to the repository.

### Modeling best practices

When starting on the project, developers usually start by building the database model. 
Instead, shift from building the *Database* to building *Core Domain* logic first.  
You don't need to persist or display data. Any infrastructure code is less important than domain logic. 
You can use unit tests to start building and testing the core domain. 

### Unit testing and DDD

Pragmatic approach:  Unit tests the Core Domain as much as possible. Use few integration tests for infrastructure code. 

Try to get 100% code coverage of the core domain logic (Entity, Value Objects, Aggregates, Domain Events).  The rest of the application doesn't need to have as much coverage.   Since we isolate the core domain logic from repositories or application services, we do not need to create mocks or other test-doubles to test it.  If we happen to use mocks, it's a code-smell that our domain layer is not isolated. 

Repositories, Factories, Application services, do not need to be unit-tested.  They should be integration tested instead.











