The command pattern is a behavioral design pattern in which an object is used to encapsulate all information needed to perform an action or trigger an event at a later time. 

Use case:

- Undo operation on an action performed
- Enforce an action (like logging)
- Validation for a command


Overview

- Represent an action as an object
- Decouple clients that execute the command from the details and dependencies of the command logic
- Enables delayed execution
	- can queue commands for later execution
	- if command objects are persistent, can delay across process restarts
 - Commands must be completely self-contained. The client doesn't pass any arguments.
 - Easy to add new commands. Just add a new class (open/closed principle)


Problematic solution

If you have a client that needs to execute comands

```csharp
 class Program
    {
        static void Main(string[] args)
        {
			var processor = new CommandExecutor();
			processor.ExecuteCommand(args);
```

Then you can have a class that is responsible for executing commands. 

```csharp
 public class CommandExecutor
    {
        internal void ExecuteCommand(string[] args)
        {
            switch (args[0])
            {
                case "UpdateQuantity":
                    UpdateQuantity(int.Parse(args[1]));
                    break;
                case "CreateOrder":
                    CreateOrder();
                    break;
                case "ShipOrder":
                    ShipOrder();
                    break;
                default:
                    Console.WriteLine("Unrecognized command");
                    break;
            }
        }
```

This approach violates a couple of SOLID principles:
- Single responsibility principle violation 
	- This class is doing too much...it has too much responsiblity.
- Open/closed principle violation
	- What happens when you try to add a command?  You'll need to update this `CommandExecutor` class everytime you need to add a command. 


### Command pattern solution

#### Common command interface

This interface only has one method `Execute()`. No parameters are passed in, which means that the command class is self-contained and client doesn't need to pass in any arguments to it. 

```csharp
public interface ICommand
{
    void Execute();
}
```

