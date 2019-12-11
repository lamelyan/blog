---
title: Aggregate pattern in DDD
description: 
tags: [ddd-khorikov]
---


### Aggregate 

Aggregate is a design pattern that helps us simplify the domain model by gathering multiple entities under a single abstraction.

 Aggregate is a design pattern that helps us simplify the domain model by gathering multiple entities under a single abstraction. This concept includes several implications. 

First of all, an aggregate is a conceptual whole, meaning that it represents a cohesive notion of the domain model. 

Every aggregate has a set of invariants, which it maintains during its lifetime. It means that *in any given time, an aggregate should reside in a valid state.* For example, let's say snacks have an additional attribute, weight, and we have an invariant stating that our snack machine cannot hold more than 10 pounds of snacks. This kind of validation should be performed in the aggregate so that it's not possible for the client code to add more snacks if the overall weight exceeds the limit. 

Every aggregate should have a *root*. That is, the entity which is the domain for the aggregate, so to speak. An important rule regarding this notion is that *classes outside of the aggregate can only reference the root of that aggregate*.  Restricting access to the entities that are internal to an aggregate helps protect the aggregate's invariants. Ideally there should be no way for the client code to break the aggregate's invariants and thus corrupt its internal state. 

Aggregates also act as a single operational unit for the code in your application layer. Application Services should retrieve them from the database, perform actions, and store them back as a single object. In other words, they should consider an aggregate a conceptual whole and refrain from working with separate entities in it. 


Another function aggregates hold is maintaining consistency boundaries. It means that in any given time the data in the database that belong to a single aggregate should be consistent. To achieve this, we need to persist an aggregate in a transactional manner.

The database should contain all information about the aggregate and this information must be consistent, meaning that it shouldn't break the invariants of this aggregate. At the same time, the invariants that span across several aggregates shouldn't be expected to be up-to-date all the time. They can be eventually consistent. 
