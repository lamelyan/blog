---
title: Visitor design pattern
tags: [design-pattern]
---

## Visitor pattern

Represent an operation to be performed on the elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates.

### Use visitor pattern when
- many unrelated operations on an object structure are required
- new operations need to be added frequently

### Use case

Given a person's bank accounts, real estate, and loans, figure out a person's net worth.

### Naive solution

```csharp
 static void Main(string[] args)
 {
     var person = new Person();
     person.BankAccounts.Add(new BankAccount { Amount = 1000, MonthlyInterest = 0.01 });
     person.BankAccounts.Add(new BankAccount { Amount = 2000, MonthlyInterest = 0.02 });
     person.RealEstates.Add(new RealEstate { EstimatedValue = 79000, MonthlyRent = 500 });
     person.Loans.Add(new Loan { Owed= 40000, MonthlyPayment = 40 });

     int netWorth = 0;
     foreach (var bankAccount in person.BankAccounts)
         netWorth += bankAccount.Amount;

     foreach (var realEstate in person.RealEstates)
         netWorth += realEstate.EstimatedValue;

     foreach (var loan in person.Loans)
         netWorth -= loan.Owed;

     Console.WriteLine(netWorth);
     Console.ReadLine();
 }
```

Drawbacks of this approach:
- you need to know the implementation of assets. In this example, you'll need to know that the balance of the  `BankAccount` is stored in `Value` property and for `RealEstate` it's the `EstimatedValue`.
- since the way to calculate net worth differs for each asset, the business logic cannot be written in a way to fit all entities. We could have each entity implement it's own net worth, however, with this approach you'll have to update each entity with methods that do the calculation. If you need to perform a different calculation, you'll once again need to update each entity with a method to perform that calculation.




### Solution with Visitor Pattern

#### Visitor Interface
Create a visitor interface that has a `Visit` method for each asset.

```csharp
  public interface IVisitor
    {
        void Visit(RealEstate realEstate);
        void Visit(BankAccount bankAccount);
        void Visit(Loan loan);
    }
```

#### Visitor implementation
Create the *visitor*  class that implements the `IVisitor` interface.  This *visitor* knows what to do (the business logic). This visitor *visits* different entities that allow him to visit and perform the logic on these entities. 

So `NetWorthVisitor` knows how to calculate the net worth for different entities. 

```csharp
  
    public class NetWorthVisitor : IVisitor
    {
        public int Total { get; set; }
        
        public void Visit(RealEstate realEstate)
        {
            Total += realEstate.EstimatedValue;
        }

        public void Visit(BankAccount bankAccount)
        {
            Total += bankAccount.Amount;
        }

        public void Visit(Loan loan)
        {
            Total -= loan.Owed;
        }
    }
```

#### Accept visitor

For a visitor to *visit* an entity, it needs to *accept* the visitor.  To do that, we create another interface called `IAsset` which only has one method `Accept`. 

```csharp
public interface IAsset
{
    void Accept(IVisitor visitor);
}
```

Now each entity that wants a visit, implements the `IAsset` interface. 

```csharp
 public class Loan : IAsset
    {
        public int Owed { get; set; }
        public int MonthlyPayment { get; set; }
        
        public void Accept(IVisitor visitor)
        {
            visitor.Visit(this);
        }
    }

    public class BankAccount : IAsset
    {
        public int Amount { get; set; }
        public double MonthlyInterest { get; set; }
        
        public void Accept(IVisitor visitor)
        {
            visitor.Visit(this);
        }
    }

    public class RealEstate : IAsset
    {
        public int EstimatedValue { get; set; }
        public int MonthlyRent { get; set; }
        
        public void Accept(IVisitor visitor)
        {
            visitor.Visit(this);
        }
    }
}
```

A person also *accepts a visitor*. This allows a person to loop through assets and make them *accept* a visitor.

```csharp
 public class Person : IAsset
 {
     public List<IAsset> Assets = new List<IAsset>();
     
     public void Accept(IVisitor visitor)
     {
         foreach(var asset in Assets)
             asset.Accept(visitor);
     }
 }
```

### Result
All the business logic has been extracted to a *visitor*.  If we need to perform another set of operations on these assets, we can create and *accept* a new *visitor*.

```csharp
 static void Main(string[] args)
{
    var person = new Person();
    person.Assets.Add(new BankAccount { Amount = 1000, MonthlyInterest = 0.01 });
    person.Assets.Add(new BankAccount { Amount = 2000, MonthlyInterest = 0.02 });
    person.Assets.Add(new RealEstate { EstimatedValue = 79000, MonthlyRent = 500 });
    person.Assets.Add(new Loan { Owed = 40000, MonthlyPayment = 40 });

	// visitor who knows the business logic
    var netWorthVisitor = new NetWorthVisitor(); 
    var incomeVisitor = new IncomeVisitor(); 

    // person accepts a visit from both visitors
    person.Accept(netWorthVisitor); 
    person.Accept(incomeVisitor);

    // visitors provide the results
    Console.WriteLine(netWorthVisitor.Total);
    Console.WriteLine(incomeVisitor.Amount);
}
```

### Extensibility (the power of visitor pattern)

Now if we want to perform some other computation on these assets we can implement another *visitor* that contains the new business logic. So let's say we want to find out the total income, we can implement it this way

```csharp
public class IncomeVisitor : IVisitor
{
    public double Amount;

    public void Visit(RealEstate realEstate)
    {
        Amount += realEstate.MonthlyRent;
    }

    public void Visit(BankAccount bankAccount)
    {
        Amount += bankAccount.Amount*bankAccount.MonthlyInterest;
    }

    public void Visit(Loan loan)
    {
        Amount -= loan.MonthlyPayment;
    }
}
```

and in the main program, we can *create* and *accept* a new visitor. 

```csharp
 static void Main(string[] args)
{
    ...omitting declarations for brevity...
    
	var netWorthVisitor = new NetWorthVisitor(); 
    var incomeVisitor = new IncomeVisitor();   // new visitor!

    // person accepts a visit from both visitors
    person.Accept(netWorthVisitor); 
    person.Accept(incomeVisitor); // accept it

    // visitors provide the results
    Console.WriteLine(netWorthVisitor.Total);
    Console.WriteLine(incomeVisitor.Amount);   // get the result from the new visitor!
}
```

-------------
Notes from the Pluralsight Course [C# Design Patterns: Visitor](https://app.pluralsight.com/player?course=patterns-library&author=john-sonmez&name=design-patterns-visitor&clip=0&mode=live) by John Sonmez
