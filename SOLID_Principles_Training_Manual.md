## **SOLID Principles Training Manual**

**Table of Contents:**

1. **Introduction to SOLID Principles**
   - 1.1 Overview of SOLID
   - 1.2 Importance of SOLID in Software Development
   - 1.3 Prerequisites

2. **Single Responsibility Principle (SRP)**
   - 2.1 Definition and Explanation
   - 2.2 Examples in C#
   - 2.3 Lab 1: Refactoring a Class with Multiple Responsibilities
   - 2.4 Common Violations and Solutions
   - 2.5 SRP Best Practices

3. **Open/Closed Principle (OCP)**
   - 3.1 Definition and Explanation
   - 3.2 Examples in C#
   - 3.3 Lab 2: Extending Functionality without Modifying Existing Code
   - 3.4 Common Violations and Solutions
   - 3.5 OCP Best Practices

4. **Liskov Substitution Principle (LSP)**
   - 4.1 Definition and Explanation
   - 4.2 Examples in C#
   - 4.3 Lab 3: Ensuring Substitutability
   - 4.4 Common Violations and Solutions
   - 4.5 LSP Best Practices

5. **Interface Segregation Principle (ISP)**
   - 5.1 Definition and Explanation
   - 5.2 Examples in C#
   - 5.3 Lab 4: Designing Interfaces for Specific Client Needs
   - 5.4 Common Violations and Solutions
   - 5.5 ISP Best Practices

6. **Dependency Inversion Principle (DIP)**
   - 6.1 Definition and Explanation
   - 6.2 Examples in C#
   - 6.3 Lab 5: Decoupling High-Level and Low-Level Modules
   - 6.4 Common Violations and Solutions
   - 6.5 DIP Best Practices

7. **Refrences**

---

## **1. Introduction to SOLID Principles**

### **1.1 Overview of SOLID**
The SOLID principles are five design principles intended to make software designs more understandable, flexible, and maintainable. The acronym SOLID stands for:

- **S**ingle Responsibility Principle (SRP)
- **O**pen/Closed Principle (OCP)
- **L**iskov Substitution Principle (LSP)
- **I**nterface Segregation Principle (ISP)
- **D**ependency Inversion Principle (DIP)

These principles were introduced by Robert C. Martin and have since become a cornerstone of object-oriented design.

### **1.2 Importance of SOLID in Software Development**
Applying SOLID principles helps developers create systems that are easier to maintain and extend over time. They promote writing code that is:

- **Scalable**: Easy to extend with new features.
- **Maintainable**: Simple to understand, debug, and refactor.
- **Flexible**: Less prone to breaking when changes are made.

### **1.3 Prerequisites**
Before diving into SOLID, readers should have a good understanding of object-oriented programming (OOP) concepts, including classes, interfaces, inheritance, and polymorphism, as well as familiarity with C# syntax and constructs.

---

## **2. Single Responsibility Principle (SRP)**

### **2.1 Definition and Explanation**
The Single Responsibility Principle states that a class should have only one reason to change, meaning it should have only one job or responsibility. This helps in reducing the complexity of code and makes it easier to understand and maintain.

### **2.2 Examples in C#**
Consider a `User` class that handles user data, authentication, and notification. This class violates SRP because it has multiple responsibilities. Refactoring it into smaller classes can help adhere to SRP.

**Example:**

```csharp
// Before SRP Violation
public class User
{
    public string Username { get; set; }
    public string Password { get; set; }

    public bool Authenticate(string username, string password)
    {
        // Authentication logic
    }

    public void SendNotification(string message)
    {
        // Notification logic
    }
}

// After SRP Adherence
public class User
{
    public string Username { get; set; }
    public string Password { get; set; }
}

public class Authenticator
{
    public bool Authenticate(string username, string password)
    {
        // Authentication logic
    }
}

public class NotificationService
{
    public void SendNotification(string message)
    {
        // Notification logic
    }
}
```

### **2.3 Lab 1: Refactoring a Class with Multiple Responsibilities**

#### **Objective:**
Refactor a given class that violates SRP into multiple classes that adhere to SRP.

#### **Task:**
You are provided with a `ProductManager` class that handles product data, inventory, and pricing. Refactor it into separate classes that each have a single responsibility.

**Initial Code:**

```csharp
public class ProductManager
{
    public void AddProduct(string name, int quantity, decimal price)
    {
        // Add product logic
    }

    public void UpdateInventory(int productId, int quantity)
    {
        // Update inventory logic
    }

    public decimal CalculateDiscount(int productId)
    {
        // Calculate discount logic
    }
}
```

#### **Solution:**

```csharp
public class Product
{
    public string Name { get; set; }
    public decimal Price { get; set; }
}

public class InventoryManager
{
    public void UpdateInventory(int productId, int quantity)
    {
        // Update inventory logic
    }
}

public class PricingManager
{
    public decimal CalculateDiscount(int productId)
    {
        // Calculate discount logic
    }
}

public class ProductManager
{
    private InventoryManager _inventoryManager = new InventoryManager();
    private PricingManager _pricingManager = new PricingManager();

    public void AddProduct(Product product, int quantity)
    {
        // Add product logic
        _inventoryManager.UpdateInventory(product.GetHashCode(), quantity);
    }
}
```

### **2.4 Common Violations and Solutions**
Common SRP violations include classes that perform multiple tasks, such as handling data, business logic, and UI interactions. The solution is to refactor these classes into smaller, more focused classes.

### **2.5 SRP Best Practices**
- Keep your classes focused on a single task.
- Regularly review your code to identify and refactor SRP violations.
- Use descriptive names for your classes to reflect their responsibilities.

---

## **3. Open/Closed Principle (OCP)**

### **3.1 Definition and Explanation**
The Open/Closed Principle states that software entities (classes, modules, functions, etc.) should be open for extension but closed for modification. This means you should be able to add new functionality without changing existing code.

### **3.2 Examples in C#**
Consider a `PaymentProcessor` class that handles payments via different methods (e.g., Credit Card, PayPal). Instead of modifying the class every time a new payment method is introduced, you can extend it by implementing a new class.

**Example:**

```csharp
// Before OCP Violation
public class PaymentProcessor
{
    public void ProcessPayment(string paymentMethod)
    {
        if (paymentMethod == "CreditCard")
        {
            // Process credit card payment
        }
        else if (paymentMethod == "PayPal")
        {
            // Process PayPal payment
        }
    }
}

// After OCP Adherence
public abstract class PaymentMethod
{
    public abstract void ProcessPayment();
}

public class CreditCardPayment : PaymentMethod
{
    public override void ProcessPayment()
    {
        // Process credit card payment
    }
}

public class PayPalPayment : PaymentMethod
{
    public override void ProcessPayment()
    {
        // Process PayPal payment
    }
}

public class PaymentProcessor
{
    public void ProcessPayment(PaymentMethod paymentMethod)
    {
        paymentMethod.ProcessPayment();
    }
}
```

### **3.3 Lab 2: Extending Functionality without Modifying Existing Code**

#### **Objective:**
Refactor a `NotificationManager` class to adhere to OCP by allowing the addition of new notification methods without modifying the existing code.

#### **Task:**
You are provided with a `NotificationManager` class that handles email and SMS notifications. Refactor it to allow adding new notification methods without modifying the existing code.

**Initial Code:**

```csharp
public class NotificationManager
{
    public void SendNotification(string message, string method)
    {
        if (method == "Email")
        {
            // Send email notification
        }
        else if (method == "SMS")
        {
            // Send SMS notification
        }
    }
}
```

#### **Solution:**

```csharp
public abstract class NotificationMethod
{
    public abstract void SendNotification(string message);
}

public class EmailNotification : NotificationMethod
{
    public override void SendNotification(string message)
    {
        // Send email notification
    }
}

public class SMSNotification : NotificationMethod
{
    public override void SendNotification(string message)
    {
        // Send SMS notification
    }
}

public class NotificationManager
{
    public void SendNotification(NotificationMethod notificationMethod, string message)
    {
        notificationMethod.SendNotification(message);
    }
}
```

### **3.4 Common Violations and Solutions**
Common OCP violations include classes that require modification every time a new feature or functionality is added. The solution is to rely on abstraction and interfaces or abstract classes, allowing you to extend functionality by adding new classes that implement these abstractions.

### **3.5 OCP Best Practices**
- Use inheritance and polymorphism to extend functionality.
- Design your classes to anticipate and accommodate future changes without requiring modifications to existing code.
- Regularly review and refactor your code to ensure it adheres to OCP.

---

## **4. Liskov Substitution Principle (LSP)**

### **4.1 Definition and Explanation**
The Liskov Substitution Principle states that objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program. In simpler terms, subclasses should be able to stand in for their parent classes without causing errors.

### **4.2 Examples in C#**
Consider a scenario where you have a `Rectangle` class and a `Square` class that inherits from `Rectangle`. A common mistake would be to override methods in the subclass in a way that violates LSP.

**Example:**

```csharp
// LSP Violation
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }

    public int Area()
    {
        return Width * Height;
    }
}

public class Square : Rectangle
{
    public override int Width
    {
        set { base.Width = base.Height = value; }
    }

    public override int Height
    {
        set { base.Width = base.Height = value; }
    }
}

// This leads to issues when using polymorphism
Rectangle rect = new Square();
rect.Width = 5;
rect.Height = 10;

// Expectation: Area should be 5 * 10 = 50
// Reality: Area is 5 * 5 = 25, violating LSP
```

**Adhering to LSP:**

```csharp
public abstract class Shape
{
    public abstract int Area();
}

public class Rectangle : Shape
{
    public int Width { get; set; }
    public int Height { get; set; }

    public override int Area()
    {
        return Width * Height;
    }
}

public class Square : Shape
{
    public int Side { get; set; }

    public override int Area()
    {
        return Side * Side;
    }
}
```

### **4.3 Lab 3: Ensuring Substitutability**

#### **Objective:**
Refactor a given codebase to ensure that subclasses can be substituted for their parent classes without altering the program's behavior.

#### **Task:**
You are provided with a `Bird` class and a `Penguin` class that inherits from it. The `Bird` class includes a `Fly` method. Refactor the code to adhere to LSP.

**Initial Code:**

```csharp
public class Bird
{
    public virtual void Fly()
    {
        // Flying logic
    }
}

public class Penguin : Bird
{
    public override void Fly()
    {
        throw new NotSupportedException("Penguins can't fly!");
    }
}
```

#### **Solution:**

```csharp
public abstract class Bird
{
    // Common bird behaviors
}

public class FlyingBird : Bird
{
    public void Fly()
    {
        // Flying logic
    }
}

public class Penguin : Bird
{
    // Penguins can't fly, so no Fly method here
}
```

### **4.4 Common Violations and Solutions**
Common LSP violations occur when a subclass overrides or extends the behavior of a superclass in a way that changes the expected outcome. The solution is to ensure that the subclass preserves the behavior expected by the superclass's contract.

### **4.5 LSP Best Practices**
- Ensure that your subclasses can be used interchangeably with their superclasses.
- Avoid overriding methods in subclasses that alter the expected behavior.
- Use abstract classes and interfaces to define clear contracts that all subclasses must adhere to.

---

## **5. Interface Segregation Principle (ISP)**

### **5.1 Definition and Explanation**
The Interface Segregation Principle states that clients should not be forced to depend on interfaces they do not use. This means that larger interfaces should be broken down into smaller, more specific ones to avoid forcing classes to implement unnecessary methods.

### **5.2 Examples in C#**
Consider a `Worker` interface that includes methods for both `Work` and `Eat`. Forcing a class to implement all methods of this interface, even if it doesn't need some of them, would violate ISP.

**Example:**

```csharp
// ISP Violation
public interface IWorker
{
    void Work();
    void Eat();
}

public class Robot : IWorker
{
    public void Work()
    {
        // Robot working logic
    }

    public void Eat()
    {
        throw new NotSupportedException("Robots don't eat!");
    }
}

// Adhering to ISP
public interface IWorkable
{
    void Work();
}

public interface IFeedable
{
    void Eat();
}

public class Human : IWorkable, IFeedable
{
    public void Work()
    {
        // Human working logic
    }

    public void Eat()
    {
        // Human eating logic
    }
}

public class Robot : IWorkable
{
    public void Work()
    {
        // Robot working logic
    }
}
```

### **5.3 Lab 4: Designing Interfaces for Specific Client Needs**

#### **Objective:**
Refactor a class that implements a large interface by breaking down the interface into smaller, more specific ones.

#### **Task:**
You are provided with a `Printer` interface that includes methods for printing, scanning, and faxing. Refactor it to adhere to ISP.

**Initial Code:**

```csharp
public interface IPrinter
{
    void Print();
    void Scan();
    void Fax();
}

public class SimplePrinter : IPrinter
{
    public void Print()
    {
        // Print logic
    }

    public void Scan()
    {
        throw new NotSupportedException("Scan not supported!");
    }

    public void Fax()
    {
        throw new NotSupportedException("Fax not supported!");
    }
}
```

#### **Solution:**

```csharp
public interface IPrinter
{
    void Print();
}

public interface IScanner
{
    void Scan();
}

public interface IFax
{
    void Fax();
}

public class SimplePrinter : IPrinter
{
    public void Print()
    {
        // Print logic
    }
}

public class MultiFunctionPrinter : IPrinter, IScanner, IFax
{
    public void Print()
    {
        // Print logic
    }

    public void Scan()
    {
        // Scan logic
    }

    public void Fax()
    {
        // Fax logic
    }
}
```

### **5.4 Common Violations and Solutions**
ISP violations typically occur when an interface forces implementing classes to include methods they don't need. The solution is to break down the interface into smaller, more focused interfaces that better match the responsibilities of implementing classes.

### **5.5 ISP Best Practices**
- Design interfaces with specific, cohesive responsibilities.
- Avoid creating "fat" interfaces with too many methods.
- Favor composition over inheritance when designing interfaces.

---

## **6. Dependency Inversion Principle (DIP)**

### **6.1 Definition and Explanation**
The Dependency Inversion Principle states that high-level modules should not depend on low-level modules. Both should depend on abstractions. Additionally, abstractions should not depend on details; details should depend on abstractions.

### **6.2 Examples in C#**
Consider a `LightBulb` class that is directly instantiated within a `Switch` class. This creates a tight coupling between the two classes. Adhering to DIP involves introducing an abstraction between the two.

**Example:**

```csharp
// DIP Violation
public class LightBulb
{
    public void TurnOn() { /*...*/ }
    public void TurnOff() { /*...*/ }
}

public class Switch
{
    private LightBulb _lightBulb = new LightBulb();

    public void Operate(bool on)
    {
        if (on)
        {
            _lightBulb.TurnOn();
        }
        else
        {
            _lightBulb.TurnOff();
        }
    }
}

// Adhering to DIP
public interface ILight
{
    void TurnOn();
    void TurnOff();
}

public class LightBulb : ILight
{
    public void TurnOn() { /*...*/ }
    public void TurnOff() { /*...*/ }
}

public class Switch
{
    private ILight _light;

    public Switch(ILight light)
    {
        _light = light;
    }

    public void Operate(bool on)
    {
        if (on)
        {
            _light.TurnOn();
        }
        else
        {
            _light.TurnOff();
        }
    }
}
```

### **6.3 Lab 5: Decoupling High-Level and Low-Level Modules**

#### **Objective:**
Refactor a class that directly depends on low-level modules by introducing abstractions.

#### **Task:**
You are provided with a `PaymentProcessor` class that directly instantiates different payment methods (e.g., `CreditCard`, `PayPal`). Refactor it to adhere to DIP.

**Initial Code:**

```csharp
public class CreditCard
{
    public void ProcessPayment() {

 /*...*/ }
}

public class PayPal
{
    public void ProcessPayment() { /*...*/ }
}

public class PaymentProcessor
{
    private CreditCard _creditCard = new CreditCard();
    private PayPal _payPal = new PayPal();

    public void Process(string method)
    {
        if (method == "CreditCard")
        {
            _creditCard.ProcessPayment();
        }
        else if (method == "PayPal")
        {
            _payPal.ProcessPayment();
        }
    }
}
```

#### **Solution:**

```csharp
public interface IPaymentMethod
{
    void ProcessPayment();
}

public class CreditCard : IPaymentMethod
{
    public void ProcessPayment() { /*...*/ }
}

public class PayPal : IPaymentMethod
{
    public void ProcessPayment() { /*...*/ }
}

public class PaymentProcessor
{
    private readonly IPaymentMethod _paymentMethod;

    public PaymentProcessor(IPaymentMethod paymentMethod)
    {
        _paymentMethod = paymentMethod;
    }

    public void Process()
    {
        _paymentMethod.ProcessPayment();
    }
}
```

### **6.4 Common Violations and Solutions**
DIP violations often occur when high-level modules are tightly coupled with low-level modules. The solution is to introduce interfaces or abstract classes that serve as intermediaries, allowing both high-level and low-level modules to depend on abstractions.

### **6.5 DIP Best Practices**
- Depend on abstractions, not concrete implementations.
- Use dependency injection to pass dependencies into classes, rather than instantiating them directly.
- Regularly refactor code to minimize coupling between modules.

## **7. Refrences**

- [SOLID Principles Stack Overflow Contributers](https://riptutorial.com/Download/solid-principles.pdf)
- [UncleBob Article](http://butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod)
- [UFMG-> Eduardo Figueiredo](https://homepages.dcc.ufmg.br/~figueiredo/disciplinas/lectures/solid_v01.pdf)

---

This training manual provides a comprehensive guide to understanding and applying the SOLID principles in C#. Each principle is explained with examples, followed by practical labs that allow you to apply what you’ve learned through hands-on refactoring exercises. By the end of this manual, you’ll have a solid foundation in SOLID principles and be equipped to write more maintainable, scalable, and flexible code.

## **Presented By:**

### **Amr_MAM**

- [https://github.com/AmrMAM](https://github.com/AmrMAM)

- [My Repositories](https://github.com/AmrMAM?tab=repositories)
