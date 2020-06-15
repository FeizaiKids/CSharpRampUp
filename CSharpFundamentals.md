1. Define properties

```
Example 1:
        public string Name 
        {
            get
            {
                return name;
            }
            set
            {
                name = value;
            }
        }
        private string name;
```

2. property & field

only in reflection & serialization diff

```
Example 1:
        public string Name
        {
            get;
            private set;
        }
```

```
Example 2:
public class Employee
{
        public string Name { get; set;}
        public int DepartmentId { get; set;}
}
Employee e = new Employee{Name = "Scott"};
```

3. readonly string category
only can be changed in constructor

4. public const string CATEGORY 
treated as static members: classNanme.CATEGORY

5. formatting

```
Console.WriteLine($"The average grade is {result:N1}");
Console.WriteLine($"Hello, {args[0]}!");
```

6. pass by reference
```
GetBookSetName(ref  Book book, "New Name");
//out keyword need initialization
GetBookSetName(out Book book, "New Name");
```

7. delegate
describe what a method will look like

```
Example 1:
public delegate string WriteLogDelegate(string logMessage);
public void WriteLogDelegateCanPointToMethod()
{
        WriteLogDelegate log;
        log = new WriteLogDelegate(ReturnMessage);
        //log = ReturnMessage;
        //log += ReturnMessage;
        var result = log("Hello!");
}
string ReturnMessage(string message)
{
        return message;
}
```

8. event + delegate

```
#in class#
//describe the event
public delegate void GradeAddedDelegate(object sender, EventArgs args);
//define the event
public event GradeAddedDelegate GradeAdded;
//call the event
if(GradeAdded != null)
{
        GradeAdded(this, new EventArgs());
}
#in main#
GradeBook gb = new GradeBook();
gb.GradeAdded += A;
            
static void A(object sender, EventArgs e)
{
        Console.WriteLine("a grade is added.");
}
```

9. deriving from a Base Class

```
public class Book: NamedObject
{
        public Book(string name) : base(name)
        {
                //...
        }
}
```

10. abstract class

```
public abstract class BookBase
{
        public abstract void AddGrade(double grade);        
}
public override void AddGrade(double grade); //for abstract or virtual method
```

11. interface
all interface method need implementation

```
public interface IBook
{
        void AddGrade(double grade);
        Statistics GetStatistics();
        string Name {get;}
        ...
}
private static void EnterGrade(IBook book){...}

public class InMemoryBook : Book, IBook {...}
```

12. File

```
var writer = File.AppendText($"{Name}.txt");
writer.WriteLine(grade);
write.Close(); // write.Dispose();

//or
using(var writer = File.AppendText($"{Name}.txt");)
{
        writer.WriteLine(grade);
}//close
```
