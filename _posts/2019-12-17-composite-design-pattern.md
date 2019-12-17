---
title: Composite design pattern
tags: [design-pattern]
---
### Composite Pattern

The composite pattern provides a way to work with **tree structures** in a **uniform fashion** regardless if you are working with a parent or a child item in that tree. 

### Where is it useful?
Think **tree structures**.
**Compose** objects into **tree** structures. 
**Uniform** interaction  allows us to *act* on tree structures uniformly

Example: File system. 
Files are leaf notes and directories are the **composites** that contain leaf notes or other directories or composites.

### Composite Pattern Characteristics
Composite patterns consist of the following:
- Component - a common interface
- Composite - component with children
- Leaf - component without children
- Client - manipulates objects in the composition



**Main benefit** - *Client can manipulate a component at any level of the tree hierarchy.*

### Example
Problem: Show the size of a file or directory component. 

```csharp
public abstract class FileSystemItem
{
    public FileSystemItem(string name)
    {
        this.Name = name;
    }

    public string Name { get; }

    public abstract decimal GetSizeInKB();  // method to be executed by leaf or composite nodes
    
}
```

#### Leaf node that implements abstract class `FileSystemItem`

```csharp
public class FileItem : FileSystemItem
{
    public FileItem(string name, long fileBytes) : base(name)
    {
        this.FileBytes = fileBytes;
    }

    public long FileBytes { get; }

    public override decimal GetSizeInKB()
    {
        return decimal.Divide(this.FileBytes, 1000);
    }
}
```

####  Composite node that implements the abstract class `FileSystemItem`

```csharp
 public class DirectoryItem : FileSystemItem
 {
     public DirectoryItem(string name) : base(name)
     {
     }

     public List<FileSystemItem> Items { get; } = new List<FileSystemItem>();

     public void Add(FileSystemItem component)
     {
         this.Items.Add(component);
     }

     public void Remove(FileSystemItem component)
     {
         this.Items.Remove(component);
     }

     public override decimal GetSizeInKB()
     {
         return this.Items.Sum(x => x.GetSizeInKB());
     }
 }
}
```

####  Use a Builder Pattern to build the directory structure.  Here is an implementation of the builder itself. 

```csharp
 public class FileSystemBuilder
{
    private DirectoryItem currentDirectory;

    public FileSystemBuilder(string rootDirectory)
    {
        this.Root = new DirectoryItem(rootDirectory);
        this.currentDirectory = this.Root;
    }

    public DirectoryItem Root { get; }

    public DirectoryItem AddDirectory(string name)
    {
        var dir = new DirectoryItem(name);
        this.currentDirectory.Add(dir);
        this.currentDirectory = dir;
        return dir;
    }

    public FileItem AddFile(string name, long fileByes)
    {
        var file = new FileItem(name, fileByes);
        this.currentDirectory.Add(new FileItem(name, fileByes));
        return file;
    }

    // Note the iterative stack based solution.
    public DirectoryItem SetCurrentDirectory(string directoryName)
    {
        var dirStack = new Stack<DirectoryItem>();
        dirStack.Push(this.Root);
        while (dirStack.Any())
        {
            var current = dirStack.Pop();
            if (current.Name == directoryName)
            {
                this.currentDirectory = current;
                return current;
            }
            foreach (var item in current.Items.OfType<DirectoryItem>())
            {
                dirStack.Push(item);
            }
        }
        throw new InvalidOperationException($"Directory name '{directoryName}' not found!");
    }

}
```
Note the iterative stack-based solution for `SetCurrentDirectory` method. We could use recursion but that might lead to stack overflow exception and hard to follow. 

#### Then build a directory structure using this builder

```csharp
  private static void BuilderExample()
  {
      var builder = new FileSystemBuilder("development");
      builder.AddDirectory("project1");
      builder.AddFile("p1f1.txt", 2100);
      builder.AddFile("p1f2.txt", 3100);
      builder.AddDirectory("sub-dir");
      builder.AddFile("p1f3.txt", 4100);
      builder.AddFile("p1f4.txt", 5100);
      builder.SetCurrentDirectory("development");
      builder.AddDirectory("project2");
      builder.AddFile("p2f1.txt", 6100);
      builder.AddFile("p2f2.txt", 7100);

      Console.WriteLine($"Total size (root) : {builder.Root.GetSizeInKB()}");
      Console.WriteLine(JsonConvert.SerializeObject(builder.Root, Formatting.Indented));
  }
```


### Examples of Composites Pattern in .NET

JSON, XML documents are examples of composites.

--------------


Notes from Pluralsight cource: [C# Design Patterns: Composite](https://app.pluralsight.com/library/courses/c-sharp-design-patterns-composite/) by [Steve Michelotti](https://app.pluralsight.com/profile/author/steve-michelotti)
