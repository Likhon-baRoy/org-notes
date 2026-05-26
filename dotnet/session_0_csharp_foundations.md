[#](#) Session 0 — C# Foundations

## 0.1 C# vs .NET
- C# = programming language
- .NET = runtime/platform that runs C# applications
- ASP.NET Core = web framework for APIs/web apps

## 0.2 Variables & Data Types
Variables store data.

Examples:
```csharp
int n = 10;
string s = "hello";
decimal amount = 100.5m;
bool flag = false;
DateTime today = DateTime.Now;
```

Strongly typed:
Every variable has a type.

## 0.3 Comments
```csharp
// single line

/*
multi line
*/
```

## 0.4 Console Output
```csharp
Console.WriteLine("Hello");
```

## 0.5 Operators
Assignment:
```csharp
salary = 50000;
```

Comparison:
```csharp
salary == 50000
```

Arithmetic:
`+ - * /`

## 0.6 Methods
Reusable block of code.

Method:
```csharp
void SayHello()
{
    Console.WriteLine("Hello");
}
```

Method with parameter:
```csharp
void Greet(string name)
{
    Console.WriteLine(name);
}
```

Return value:
```csharp
int Add(int a, int b)
{
    return a + b;
}
```

## 0.7 Classes & Objects
Class = blueprint/template
Object = instance of class

```csharp
class Employee
{
    public string Name { get; set; }
    public decimal Salary { get; set; }
}

Employee employee = new Employee();
employee.Name = "Rahim";
```

Property:
- get = read
- set = assign/update

## 0.8 Constructor
Runs automatically when object is created.

```csharp
class Student
{
    public Student()
    {
        Console.WriteLine("Created");
    }
}
```

## 0.9 Encapsulation & Access Modifiers
public = accessible from anywhere
private = accessible only inside same class

```csharp
private decimal salary;
```

Encapsulation:
Hide internal data and control access.

## 0.10 If / Else
```csharp
if(age >= 18)
{
    Console.WriteLine("Adult");
}
else
{
    Console.WriteLine("Minor");
}
```

## 0.11 Loops
for:
```csharp
for(int i = 1; i <= 5; i++)
{
    Console.WriteLine(i);
}
```

foreach:
```csharp
foreach(var item in items)
{
    Console.WriteLine(item);
}
```

`i++` = increase by 1

## 0.12 List<T>
Dynamic collection of same type.

```csharp
List<string> names = new();
names.Add("Rahim");
```

Index starts at 0.

## 0.13 LINQ Basics
LINQ = query/filter data in C#

Where():
```csharp
employees.Where(x => x.Salary > 50000)
```

FirstOrDefault():
```csharp
employees.FirstOrDefault()
```

Count():
```csharp
employees.Count()
```

## 0.14 Null & Nullable Types
null = no value

Nullable:
```csharp
int? age = null;
```

Null check:
```csharp
if(name != null)
{
    Console.WriteLine(name.Length);
}
```

## 0.15 Async / Await
Used for non-blocking operations.

Best for:
- Database query
- API call
- File upload
- Email sending

Example:
```csharp
await Task.Delay(2000);
```

Mental model:
- sync = block and wait
- async = wait smartly without blocking thread
