---
title: State design pattern
tags: [design-pattern]
---

### State design pattern

The **state pattern** is a behavioral software **design pattern** that allows an object to alter its behavior when its internal **state** changes.

#### Motivating example
Booking a ticket.   Booking has multiple **states**: New, Pending, Closed, Booked. The **behavior** of the booking changes depending on the **state** of the booking. How can this be accomplished?

#### Naive approach to managing state
Use flags like isBooked, isCancelled, etc...

You'll end up with a switch statement that looks something like this:

```csharp
public void ProcessingComplete(Booking booking, ProcessingResult result)
        {
            isPending = false;  

            switch (result)
            {
                case ProcessingResult.Sucess:
                    isBooked = true;
                    ShowState("Booked");
                    View.ShowStatusPage("Enjoy the Event");
                    break;
                case ProcessingResult.Fail:
                    View.ShowProcessingError();
                    Attendee = string.Empty;
                    TicketCount = 0;
                    BookingID = new Random().Next();
                    isNew = true;
                    ShowState("New");
                    View.ShowEntryPage();
                    break;
                case ProcessingResult.Cancel:
                    ShowState("Closed");
                    View.ShowStatusPage("Processing Canceled");
                    break;
            }
        }
```
For entire problematic class [see this reference](https://github.com/dev-e-loper/blog/blob/b50b18bc23f3a52ca394d4d618ded3cf7ea392e9/src/design-patterns/state/Booking.cs).

This approach doesn't work well because every time a state is added, the existing logic needs to be updated. The more states we have the more interdependent our logic becomes. This becomes difficult to extend and maintain.


### Design challenges
1. How can an object change it's behavior when it's internal state changes?
2. How can state-specific behaviors be defined so that states can be added without altering the existing states?

### Solution
State design pattern solves this problem by:
1. Encapsulating state-specific behaviors within separate state objects. 
2. Delegating the execution of its behaviors to one of the state objects at a given time instead of implementing state-specific behaviors itself. 

There are three main components to the pattern:
1. the context
The context is a class which maintains an instance of a concrete state as its current state.

2. an *abstract* state class
 The abstract state is an abstract class that defines an interface encapsulating all state-specific behaviors. 
 
3. and any number of concrete states that derrive from abstract state class
The concrete state is a subclass of the abstract state that implements behaviors specific to a particular state of the context. 

Basically you offload all the business logic to the concrete states that implement behavior. 


### Abstract state

Notice that: 
1. State is abstract which means it has to be derrive in order to be instatiated
2. The context is passed in.
```csharp
namespace State_Design_Pattern.Logic
{
    public abstract class BookingState
    {
        public abstract void EnterState(BookingContext booking);
        public abstract void Cancel(BookingContext booking);
        public abstract void DatePassed(BookingContext booking);
        public abstract void EnterDetails(BookingContext booking, string attendee, int ticketCount);
    }
}
```

### Concrete state

The following is a sample of a concreate state - New Booking. 

Note: 
1. Implements the abstract `BookingState` class
2. Implements business logic that is specific to that state
```csharp
namespace State_Design_Pattern.Logic
{
    class NewState : BookingState
    {
        public override void Cancel(BookingContext booking)
        {
            booking.TransitionToState(new ClosedState("Booking Canceled"));
        }

        public override void DatePassed(BookingContext booking)
        {
            booking.TransitionToState(new ClosedState("Booking Expired"));
        }

        public override void EnterDetails(BookingContext booking, string attendee, int ticketCount)
        {
            booking.Attendee = attendee;
            booking.TicketCount = ticketCount;
            booking.TransitionToState(new PendingState());
        }

        public override void EnterState(BookingContext booking)
        {
            booking.BookingID = new Random().Next();
            booking.ShowState("New");
            booking.View.ShowEntryPage();
        }
    }
}
```

### Context

Context class now contains very little information about the business logic for specific states.

Note: 
1. Context hold the current state as a property `currentState`
2. It delegates all the logic to the concrete state objects like so:
```csharp
public  void  Cancel()  
{ 
   currentState.Cancel(this);  
}
```
Here is the entire context class
```csharp
namespace State_Design_Pattern.Logic
{
    public class BookingContext
    {
        public MainWindow View { get; private set; }
        public string Attendee { get; set; }
        public int TicketCount { get; set; }
        public int BookingID { get; set; }

        private BookingState currentState;

        public void TransitionToState(BookingState state)
        {
            currentState = state;
            currentState.EnterState(this);
        }

        public BookingContext(MainWindow view)
        {
            View = view;
            TransitionToState(new NewState());
        }

        public void SubmitDetails(string attendee, int ticketCount)
        {
            currentState.EnterDetails(this, attendee, ticketCount);
        }

        public void Cancel()
        {
            currentState.Cancel(this);
        }

        public void DatePassed()
        {
            currentState.DatePassed(this);
        }

        public void ShowState(string stateName)
        {
            View.grdDetails.Visibility = System.Windows.Visibility.Visible;
            View.lblCurrentState.Content = stateName;
            View.lblTicketCount.Content = TicketCount;
            View.lblAttendee.Content = Attendee;
            View.lblBookingID.Content = BookingID;
        }

    }
}
```
