---
title: State design pattern
tags: [design-pattern]
---

### State design pattern

The **state pattern** is a behavioral software **design pattern** that allows an object to alter its behavior when its internal **state** changes.

#### Motivating example
Work item tracking software like Jira or TFS.  A work item can have multiple **states**: Active, Resolved, Closed. The **behavior** of the work item changes depending on the **state** of the item. How can this be accomplished?

<img src="/assets/img/state-design-pattern-motivating-example.png" />

#### Naive approach to managing state
 Use flags: isBooked, isCancelled, etc...This approach doesn't work well because the flags need to be set in multiple places in the code.  
