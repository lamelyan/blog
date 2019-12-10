---
title: Entity vs Value Objects
description: 
tags: [ddd-khorikov]
---

### Entities vs Value Objects

The main difference between Entities and Value Objects is the way we identify them. 

#### Types of Equality:

- Identifier equality  - implies that a class has an id field, with two instances having same value
- Refernece equality -  two instances are equal if they reference the same address in memory
- Structural equality - all of class members match

Entities = Identifier + Reference equality. 
- Have inherent identity
- Mutable

For example: two people with the same name are still two different people.

Value Objects =  Reference + Structural equality.
- don't nhave an Id field
- can be treated interchangeably
- immutable
- cannot live on their own, they should always belong to one or several entities

For example: money. We can replace $10 with another $10 bill with no problem. We don't have an Id field and the bills can be treated interchangeably. Money is imutable, it cannot be changed. $10 is a $10 bill.  Money has to belong to an entity. It can not exist by itself. 


### As a rule, perfer value objects to entities 
- Value objects are light-weight and easier to work with
- Put most of the business logic to value objects
- Entities would act as wrappers on value objects and provide more high-level instructions

