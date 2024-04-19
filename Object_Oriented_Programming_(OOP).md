# Object-Oriented Programming (OOP)

### Table of Contents

1. **Introduction to Object-Oriented Programming (OOP) in C#**
   - Key Principles of OOP
     - Encapsulation
     - Inheritance
     - Polymorphism
     - Abstraction

2. **The Training Labs**
   - Lab 1: Introduction to Classes and Objects
     - Create a `Book` class
     - Instantiate the `Book` class
     - Write methods to display book details
   - Lab 2: Implementing Encapsulation
     - Create a `BankAccount` class
     - Test the `BankAccount` class
   - Lab 3: Understanding Inheritance
     - Create a base class `Vehicle`
     - Create derived classes `Car` and `Bicycle`
     - Test the derived classes
   - Lab 4: Practicing Polymorphism
     - Implement a `Calculator` class with overloaded methods
     - Create a `Shape` base class with derived classes
     - Test runtime polymorphism
   - Lab 5: Applying Abstraction
     - Create an abstract class `Employee`
     - Implement the abstract method in derived classes
     - Create an interface `IPrintable`
     - Test the implementation
   - Lab 6: Building a Real-World Application
     - Design a `Library` system
     - Implement borrowing and returning books
     - Extend the system

# 

**Object-Oriented Programming (OOP)** is a programming paradigm that uses "objects" to represent data and methods. These objects are instances of classes, which define the structure and behavior of the objects. OOP in C# revolves around four key principles: **Encapsulation**, **Inheritance**, **Polymorphism**, and **Abstraction**.

#### Key Principles of OOP

1. **Encapsulation**:
   - **Definition**: Encapsulation is the concept of bundling the data (variables) and methods that operate on the data into a single unit, known as a class. It restricts direct access to some of an object's components, which is a means of preventing accidental interference and misuse of the data.
   - **Example**:

   ```csharp
   public class BankAccount
   {
       private string accountNumber;
       private double balance;

       public BankAccount(string accNumber, double initialBalance)
       {
           accountNumber = accNumber;
           balance = initialBalance;
       }

       public void Deposit(double amount)
       {
           if(amount > 0)
           {
               balance += amount;
               Console.WriteLine($"Deposited: ${amount}");
           }
       }

       public void Withdraw(double amount)
       {
           if(amount > 0 && amount <= balance)
           {
               balance -= amount;
               Console.WriteLine($"Withdrawn: ${amount}");
           }
           else
           {
               Console.WriteLine("Invalid amount or insufficient balance.");
           }
       }

       public double GetBalance()
       {
           return balance;
       }
   }
   ```

   - **Practice**: Create a class `Car` with private fields `make`, `model`, and `year`, and public methods to set and get these values.

2. **Inheritance**:
   - **Definition**: Inheritance is a mechanism in OOP that allows a new class to inherit properties and methods from an existing class. The class that is inherited from is called the "base class" or "parent class", and the class that inherits is called the "derived class" or "child class".
   - **Example**:
     ```csharp
     public class Animal
     {
         public void Eat()
         {
             Console.WriteLine("Eating...");
         }
     }

     public class Dog : Animal
     {
         public void Bark()
         {
             Console.WriteLine("Barking...");
         }
     }

     class Program
     {
         static void Main()
         {
             Dog dog = new Dog();
             dog.Eat();  // Inherited method
             dog.Bark(); // Child class method
         }
     }
     ```
   - **Practice**: Create a base class `Employee` with properties like `Name` and `Position`, and a method `Work()`. Then create a derived class `Manager` that adds a method `ManageTeam()`.

3. **Polymorphism**:
   - **Definition**: Polymorphism allows objects of different classes to be treated as objects of a common super class. It can be achieved in C# through method overriding (runtime polymorphism) and method overloading (compile-time polymorphism).
   - **Example**:
     ```csharp
     public class Shape
     {
         public virtual void Draw()
         {
             Console.WriteLine("Drawing a shape");
         }
     }

     public class Circle : Shape
     {
         public override void Draw()
         {
             Console.WriteLine("Drawing a circle");
         }
     }

     public class Square : Shape
     {
         public override void Draw()
         {
             Console.WriteLine("Drawing a square");
         }
     }

     class Program
     {
         static void Main()
         {
             Shape shape1 = new Circle();
             Shape shape2 = new Square();

             shape1.Draw(); // Outputs: Drawing a circle
             shape2.Draw(); // Outputs: Drawing a square
         }
     }
     ```
   - **Practice**: Create a `Vehicle` class with a method `Start()`, and derive `Car` and `Motorcycle` classes that override the `Start()` method.

4. **Abstraction**:
   - **Definition**: Abstraction is the concept of hiding the complex implementation details and showing only the essential features of an object. In C#, abstraction is achieved using abstract classes and interfaces.
   - **Example**:
     ```csharp
     public abstract class Animal
     {
         public abstract void MakeSound();

         public void Sleep()
         {
             Console.WriteLine("Sleeping...");
         }
     }

     public class Dog : Animal
     {
         public override void MakeSound()
         {
             Console.WriteLine("Bark");
         }
     }

     class Program
     {
         static void Main()
         {
             Animal myDog = new Dog();
             myDog.MakeSound(); // Outputs: Bark
             myDog.Sleep();     // Outputs: Sleeping...
         }
     }
     ```
   - **Practice**: Create an abstract class `Appliance` with an abstract method `TurnOn()`, and derive classes `WashingMachine` and `Refrigerator` that implement the `TurnOn()` method.

### The Training Labs

---

#### **Lab 1: Introduction to Classes and Objects**

**Objective**: Understand the basics of classes and objects in C#.

**Concepts Covered**:
- Defining a class.
- Creating properties and methods.
- Instantiating objects and interacting with them.

**Activities**:

1. **Create a `Book` class**:
   - The `Book` class will have three properties: `Title`, `Author`, and `Price`.

   ```csharp
   public class Book
   {
       public string Title { get; set; }
       public string Author { get; set; }
       public double Price { get; set; }
   }
   ```

2. **Instantiate the `Book` class**:
   - Create an instance of the `Book` class and assign values to its properties.

   ```csharp
   class Program
   {
       static void Main(string[] args)
       {
           Book myBook = new Book();
           myBook.Title = "Flutter BLoc Training Manual";
           myBook.Author = "Amr Mostafa";
           myBook.Price = 29.99;

           Console.WriteLine("Book Details:");
           Console.WriteLine($"Title: {myBook.Title}");
           Console.WriteLine($"Author: {myBook.Author}");
           Console.WriteLine($"Price: ${myBook.Price}");
       }
   }
   ```

3. **Write methods to display book details**:
   - Add a method inside the `Book` class to display the book's information.

   ```csharp
   public class Book
   {
       public string Title { get; set; }
       public string Author { get; set; }
       public double Price { get; set; }

       public void DisplayDetails()
       {
           Console.WriteLine($"Title: {Title}");
           Console.WriteLine($"Author: {Author}");
           Console.WriteLine($"Price: ${Price}");
       }
   }

   class Program
   {
       static void Main(string[] args)
       {
           Book myBook = new Book();
           myBook.Title = "Flutter BLoc Training Manual";
           myBook.Author = "Amr Mostafa";
           myBook.Price = 29.99;

           myBook.DisplayDetails();
       }
   }
   ```

---

#### **Lab 2: Implementing Encapsulation**

**Objective**: Learn how to implement encapsulation in C# by controlling access to the class's data.

**Concepts Covered**:
- Creating private fields.
- Implementing public methods to access and modify these fields.
- Encapsulating data to protect it from unwanted access.

**Activities**:

1. **Create a `BankAccount` class**:
   - The `BankAccount` class will have private fields for `accountNumber` and `balance`.

   ```csharp
   public class BankAccount
   {
       private string accountNumber;
       private double balance;

       public BankAccount(string accNumber, double initialBalance)
       {
           accountNumber = accNumber;
           balance = initialBalance;
       }

       public void Deposit(double amount)
       {
           if(amount > 0)
           {
               balance += amount;
               Console.WriteLine($"Deposited: ${amount}");
           }
       }

       public void Withdraw(double amount)
       {
           if(amount > 0 && amount <= balance)
           {
               balance -= amount;
               Console.WriteLine($"Withdrawn: ${amount}");
           }
           else
           {
               Console.WriteLine("Invalid amount or insufficient balance.");
           }
       }

       public double GetBalance()
       {
           return balance;
       }
   }
   ```

2. **Test the `BankAccount` class**:
   - Write a program to test the deposit and withdrawal methods and display the current balance.

   ```csharp
   class Program
   {
       static void Main(string[] args)
       {
           BankAccount myAccount = new BankAccount("123456", 1000);

           myAccount.Deposit(500);
           myAccount.Withdraw(200);

           Console.WriteLine($"Current Balance: ${myAccount.GetBalance()}");
       }
   }
   ```

---

#### **Lab 3: Understanding Inheritance**

**Objective**: Practice implementing inheritance in C# to reuse and extend the functionality of classes.

**Concepts Covered**:
- Creating a base class.
- Deriving classes from a base class.
- Overriding methods in derived classes.

**Activities**:

1. **Create a base class `Vehicle`**:
   - The `Vehicle` class will have properties like `Brand` and `Speed`.

   ```csharp
   public class Vehicle
   {
       public string Brand { get; set; }
       public int Speed { get; set; }

       public void Start()
       {
           Console.WriteLine($"{Brand} vehicle starting...");
       }
   }
   ```

2. **Create derived classes `Car` and `Bicycle`**:
   - The `Car` and `Bicycle` classes will inherit from `Vehicle` and add their own methods.

   ```csharp
   public class Car : Vehicle
   {
       public void Honk()
       {
           Console.WriteLine("Car honking...");
       }
   }

   public class Bicycle : Vehicle
   {
       public void RingBell()
       {
           Console.WriteLine("Bicycle ringing bell...");
       }
   }
   ```

3. **Test the derived classes**:
   - Instantiate the `Car` and `Bicycle` classes, and call their methods to demonstrate inheritance.

   ```csharp
   class Program
   {
       static void Main(string[] args)
       {
           Car myCar = new Car();
           myCar.Brand = "Toyota";
           myCar.Speed = 120;
           myCar.Start();
           myCar.Honk();

           Bicycle myBicycle = new Bicycle();
           myBicycle.Brand = "Giant";
           myBicycle.Speed = 20;
           myBicycle.Start();
           myBicycle.RingBell();
       }
   }
   ```

---

#### **Lab 4: Practicing Polymorphism**

**Objective**: Explore how polymorphism allows methods to behave differently based on the object calling them.

**Concepts Covered**:
- Method overloading (compile-time polymorphism).
- Method overriding (runtime polymorphism).

**Activities**:

1. **Implement a `Calculator` class with overloaded methods**:
   - The `Calculator` class will have multiple `Add` methods with different parameters.

   ```csharp
   public class Calculator
   {
       public int Add(int a, int b)
       {
           return a + b;
       }

       public double Add(double a, double b)
       {
           return a + b;
       }

       public string Add(string a, string b)
       {
           return a + b;
       }
   }

   class Program
   {
       static void Main(string[] args)
       {
           Calculator calc = new Calculator();

           Console.WriteLine(calc.Add(5, 10));       // Output: 15
           Console.WriteLine(calc.Add(5.5, 10.5));  // Output: 16.0
           Console.WriteLine(calc.Add("Hello, ", "World!")); // Output: Hello, World!
       }
   }
   ```

2. **Create a `Shape` base class with derived classes**:
   - The `Shape` class will have a `Draw()` method that will be overridden in derived classes `Triangle` and `Rectangle`.

   ```csharp
   public class Shape
   {
       public virtual void Draw()
       {
           Console.WriteLine("Drawing a shape.");
       }
   }

   public class Triangle : Shape
   {
       public override void Draw()
       {
           Console.WriteLine("Drawing a triangle.");
       }
   }

   public class Rectangle : Shape
   {
       public override void Draw()
       {
           Console.WriteLine("Drawing a rectangle.");
       }
   }
   ```

3. **Test runtime polymorphism**:
   - Demonstrate polymorphism by using the base class reference to call overridden methods.

   ```csharp
   class Program
   {
       static void Main(string[] args)
       {
           Shape myShape = new Triangle();
           myShape.Draw(); // Output: Drawing a triangle.

           myShape = new Rectangle();
           myShape.Draw(); // Output: Drawing a rectangle.
       }
   }
   ```

---

#### **Lab 5: Applying Abstraction**

**Objective**: Understand how to use abstraction to define common behavior for a group of related objects.

**Concepts Covered**:
- Creating abstract classes and methods.
- Implementing abstract methods in derived classes.
- Using interfaces to define contracts.

**Activities**:

1. **Create an abstract class `Employee`**:
   - The `Employee` class will have an abstract method `CalculateSalary()`.

   ```csharp
   public abstract class Employee
   {
       public string Name { get; set; }
       public abstract double CalculateSalary();
   }
   ```

2. **Implement the abstract method in derived classes**:
   - Create `FullTimeEmployee` and `PartTimeEmployee` classes that inherit from `Employee` and implement `CalculateSalary()`.

   ```csharp
   public class FullTimeEmployee : Employee
   {
       public double MonthlySalary { get; set; }

       public override double CalculateSalary()
       {
           return MonthlySalary;
       }
   }

   public class PartTimeEmployee : Employee
   {
       public double HourlyRate { get; set; }
       public int HoursWorked { get; set; }

       public override double CalculateSalary()
       {
           return HourlyRate * HoursWorked;
       }
   }
   ```

3. **Create an interface `IPrintable`**:
   - The `IPrintable` interface will define a `PrintDetails()` method.

   ```csharp
   public interface IPrintable
   {
       void PrintDetails();
   }

   public class FullTimeEmployee : Employee, IPrintable
   {
       public double MonthlySalary { get; set; }

       public override double CalculateSalary()
       {
           return MonthlySalary;
       }

       public void PrintDetails()
       {
           Console.WriteLine($"Full-time Employee: {Name}, Salary: {CalculateSalary()}");
       }
   }

   public class PartTimeEmployee : Employee, IPrintable
   {
       public double HourlyRate { get; set; }
       public int HoursWorked { get; set; }

       public override double CalculateSalary()
       {
           return HourlyRate * HoursWorked;
       }

       public void PrintDetails()
       {
           Console.WriteLine($"Part-time Employee: {Name}, Salary: {CalculateSalary()}");
       }
   }
   ```

4. **Test the implementation**:
   - Instantiate objects of `FullTimeEmployee` and `PartTimeEmployee`, and call `PrintDetails()`.

   ```csharp
   class Program
   {
       static void Main(string[] args)
       {
           FullTimeEmployee ftEmployee = new FullTimeEmployee
           {
               Name = "John Doe",
               MonthlySalary = 3000
           };

           PartTimeEmployee ptEmployee = new PartTimeEmployee
           {
               Name = "Jane Doe",
               HourlyRate = 20,
               HoursWorked = 80
           };

           ftEmployee.PrintDetails();  // Output: Full-time Employee: John Doe, Salary: 3000
           ptEmployee.PrintDetails();  // Output: Part-time Employee: Jane Doe, Salary: 1600
       }
   }
   ```

---

#### **Lab 6: Building a Real-World Application**

**Objective**: Apply the OOP principles learned in the previous labs to build a small real-world application.

**Concepts Covered**:
- Integrating encapsulation, inheritance, polymorphism, and abstraction into a cohesive project.
- Designing classes that interact to form a functional system.

**Activities**:

1. **Design a `Library` system**:
   - Create classes like `Book`, `Member`, and `Loan` to manage the library operations.

   ```csharp
   public class Book
   {
       public string Title { get; set; }
       public string Author { get; set; }
       public bool IsAvailable { get; set; } = true;
   }

   public class Member
   {
       public string Name { get; set; }
       public List<Loan> Loans { get; } = new List<Loan>();

       public void BorrowBook(Book book)
       {
           if(book.IsAvailable)
           {
               Loan loan = new Loan
               {
                   Book = book,
                   LoanDate = DateTime.Now
               };
               Loans.Add(loan);
               book.IsAvailable = false;
               Console.WriteLine($"{Name} borrowed {book.Title}");
           }
           else
           {
               Console.WriteLine($"{book.Title} is not available.");
           }
       }

       public void ReturnBook(Book book)
       {
           Loan loan = Loans.FirstOrDefault(l => l.Book == book);
           if(loan != null)
           {
               Loans.Remove(loan);
               book.IsAvailable = true;
               Console.WriteLine($"{Name} returned {book.Title}");
           }
       }
   }

   public class Loan
   {
       public Book Book { get; set; }
       public DateTime LoanDate { get; set; }
   }
   ```

2. **Implement borrowing and returning books**:
   - Allow members to borrow and return books, and track the availability of each book.

   ```csharp
   class Program
   {
       static void Main(string[] args)
       {
           Book book1 = new Book { Title = "1984", Author = "George Orwell" };
           Book book2 = new Book { Title = "To Kill a Mockingbird", Author = "Harper Lee" };

           Member member1 = new Member { Name = "Alice" };
           Member member2 = new Member { Name = "Bob" };

           member1.BorrowBook(book1); // Output: Alice borrowed 1984
           member2.BorrowBook(book1); // Output: 1984 is not available.

           member1.ReturnBook(book1); // Output: Alice returned 1984
           member2.BorrowBook(book1); // Output: Bob borrowed 1984
       }
   }
   ```

3. **Extend the system**:
   - Add more features, such as overdue book tracking, member registration, and fine calculations to make the system more robust.

---
https://github.com/AmrMAM

Amr_MAM
