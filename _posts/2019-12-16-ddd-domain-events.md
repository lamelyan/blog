---
title: Domain events
tags: [ddd-khorikov]
---
### Domain events


Domain events facilitate communication between Bounded Contexts (BC).  We don't want to reference entities from different contexts (this will introduce coupling), instead we can use domain events. The entity from BC1 can raise an event and entity from BC2 can handle it. 

We need to make sure that events are raised when the entire transaction is complete. If events are raised by entity classes, it's hard to rollback actions if there was an error after event was raised. 

To handle this scenario, we break down the solution into two parts:
1. Creating event  - which is done by entities
2. Dispatching event - which is done by infrastructure


#### Creating an event
Instead of an entity raising an event, creates it and saves it to an internal list.  A good place for it would be in the `AggregateRoot` base class. 

```csharp 
    public abstract class AggregateRoot : Entity
    {
        private readonly List<IDomainEvent> _domainEvents = new List<IDomainEvent>();
        public virtual IReadOnlyList<IDomainEvent> DomainEvents => _domainEvents;

        // This method should only be called by derrived entities, hence it's marked as protected
        protected virtual void AddDomainEvent(IDomainEvent newEvent)
        {
            _domainEvents.Add(newEvent);
        }
        ...
    }    
```

[See this class for reference](https://github.com/vkhorikov/DddInAction/blob/85e9f26a18180d15d10a7ee63eea309ff2eea75f/DddInPractice.Logic/Common/AggregateRoot.cs#L10)

The entity then calls `AdddDomainEvent` instead of raising an event:


```csharp
    public class Atm : AggregateRoot
    {
        public virtual void TakeMoney(decimal amount)
        {
            ...perform logic ...
            
            AddDomainEvent(new BalanceChangedEvent(amountWithCommission));
        }
```

[See this for reference](https://github.com/vkhorikov/DddInAction/blob/85e9f26a18180d15d10a7ee63eea309ff2eea75f/DddInPractice.Logic/Atms/Atm.cs#L41)

#### Dispatching an event

How do we process events only when Unit Of Work is committed? 

Dispatching is done by infrastructure. If you use an ORM, you can hook into the  'post persisted events' such as  `OnPostInsert` or some other event.  

[See this EventListener class](https://github.com/vkhorikov/DddInAction/blob/master/DddInPractice.Logic/Utils/EventListener.cs)
