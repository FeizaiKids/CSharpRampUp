1. intro

```
public class CircularBuffer<T>
{
  private T[] _buffer;
  //...
}
var buffer = new CircularBuffer<double>();
```

2. List

3. Queue
First In First Out data structure

```
Example:
Queue<Employee> line = new Queue<Employee>();
line.Enqueue(new Employee {Name = "Alex"});
line.Enqueue(new Employee {Name = "Dani"});
line.Enqueue(new Employee {Name = "Chris"});

while(line.Count > 0)
{
  var employee = line.Dequeue();
  Console.WriteLine(employee.Name);//Alex Dani Chris
}

line.Peek();
line.Contains();
line.ToArray();
```

4. Stack
Last In First Out data structure

```
Example:
Stack<Employee> line = new Stack<Employee>();
line.Push(new Employee {Name = "Alex"});
line.Push(new Employee {Name = "Dani"});
line.Push(new Employee {Name = "Chris"});

while(line.Count > 0)
{
  var employee = line.Pop();
  Console.WriteLine(employee.Name);//Alex Dani Chris
}

line.Peek();
line.Contains();
line.ToArray();
```

5. HashSet
no duplicate item

```
Example:
HashSet<int> set = new HashSet<int>();
set.Add(1);
set.Add(2);
set.Add(2);

foreach(var item in set)
{
  Console.WriteLine(item);
}

var set1 = new HashSet<int>() {1,2,3};
var set2 = new HashSet<int>() {2,3,4};
set1.IntersectWith(set2);//set1 : new[] {2,3}
set1.UnionWith(set2);//set1: new[] {1,2,3,4}
set1.SymmetricExceptWith(set2);//set1: new[] {1,4} 
---------------for object, reference types is different---------
```

6. LinkedList

```
LinkList<int> list = new LinkedList<int>();
list.AddFirst(2);
list.AddFirst(3);

var first = list.First;//not int type
list.AddAfter(first, 5);
list.AddBefore(first, 10);

foreach(var item in list)
{
  Console.WriteLine(item);
}
```

7. Dictionary

```
Example:
var employeesByName = new Dictionary<string, Employee>();
employeesByName.Add("Scott", new Employee {Name = "Scott"});

var scott = employeesByName["Scott"];

foreach (var item in employeesByName)
{
  Console.WriteLine("{0}:{1}", item.Key, item.Value.Name);
}
```

8. SortedDictionary/ SortedList/ SortedSet

```
SortedDictionary is similar to SortedList
diff memory usage
```

9. construct a generic data structure

```
Example:
    public interface IBuffer<T>
    {
        bool IsEmpty { get; }
        void Write(T value);
        T read();
    }

    public class Buffer<T> : IBuffer<T>
    {
        protected Queue<T> _queue = new Queue<T>();

        public virtual bool IsEmpty
        {
            get { return _queue.Count == 0; }
        }

        public virtual void Write(T value)
        {
            _queue.Enqueue(value);
        }

        public virtual T read()
        {
            return _queue.Dequeue();
        }
    }

    public class CircularBuffer<T> : Buffer<T>
    {
        int _capacity;
        public CircularBuffer(int capacity = 10)
        {
            _capacity = capacity;
        }

        public override void Write(T value)
        {
            base.Write(value);
            if (_queue.Count > _capacity)
            {
                _queue.Dequeue();
            }
        }

        public bool IsFull { get { return _queue.Count == _capacity; } }
    }
```

10. IEnumerable<T>

```
IEnumerable<T> : IEnumerable
{
    IEnumerator<T> GetEnumerator();
}
//IEnumerator GetEnumerator();

Example:
        public IEnumerator<T> GetEnumerator()
        {
            //return _queue.GetEnumerator();
            foreach (var item in _queue)
            {
                //...
                yield return item; //automatically build the state machine to implement IEnumerator<T>, it allows these items to be handed back one at a time in a lazy manner
            }
        }

        IEnumerator IEnumerable.GetEnumerator() //use explicit interface implementation
        {
            return GetEnumerator();
            
        }
```   
   
11 Other interfaces
IList<T>                            access by index
ICollection<T>                      add,remove,search
IDictionary<K,V>                    access by key
IReadOnlyCollection<T>              countable collection
ISet<T>                             set based operation
IComparer<T>, IEqualityComparer<T>  compare objects

12. compare
IEqualityComparer<T> 

```
Example 1:
        public class EmployeeComparer : IEqualityComparer<Employee>
        {
            public bool Equals(Employee x, Employee y)
            {
                return String.Equals(x.Name, y.Name);
            }

            public int GetHashCode(Employee obj)
            {
                return obj.Name.GetHashCode();
            }
        }

        static void Main(string[] args)
        {
            var departments = new SortedDictionary<string, HashSet<employee>>();

            departments.Add("Sales", new HashSet<Employee>(new EmployeeComparer()));
            departments["Sales"].Add(new Employee { Name = "Joy" });
            departments["Sales"].Add(new Employee { Name = "Dani" });
            departments["Sales"].Add(new Employee { Name = "Dani" });

        }
        
        Example 2: (restructure)
        public class DepartmentCollection : SortedDictionary<string, SortedSet<Employee>>
        {
          public DepartmentCollection Add(string departmentName, Employee employee)
          {
            if(!ContainsKey(departmentName))
            {
              Add(departmentName, new SortedSet<Employee>(new EmployeeCompare()));
            }
            this[departmentName].Add(employee);
            return this;
          }
        }
        
        var departments = new DepartmentCollection();
        departments.Add("Sales", new Employee {Name = "Joy"})
                    .Add("Sales", new Employee {Name = "Joy"})
                    .Add("Sales", new Employee {Name = "Joy"});
```



