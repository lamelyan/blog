---
title: Bounded Contexts
tags: [ddd-khorikov]
---

### Bounded Contexts

#### What is Bounded context
Bounded context is a central pattern in domain-driven design. It stands for separating the model and explicitly drawing the boundaries between its pieces.

####  The problem they solve
As your application grows, it becomes harder to maintain a single unified model as it becomes larger and more people get involved in the development process. Big models bring significant communication and integration overhead with them. Bounded contexts help reduce that overhead. 

Recommended talk by Eric Evans - [DDD and Microservices: At last, some boundaries]([https://vimeo.com/125769142](https://vimeo.com/125769142))

### Types of Physical Isolation of bounded context


#### Type 1: Same assembly
Keep contexts in the same assembly but different namespaces.  Each context in its folder. 
In this case, you use the same database and it is best to use a different schema for each context.

#### Type 2: Separate assemblies

Each folder it's own UI and Logic. 


#### Type 3: Separate deployment (Microservices)
Run as sepearate processes. 



As you go from Type 1 to 3, the easier it is to maintain proper isolation. At the same time, this produces bigger maintenance overhead.



