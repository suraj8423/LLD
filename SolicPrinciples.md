# Solid Principles

- S : Single Responsibility Principle
- O : Open/Closed Principle
- L : Liskov Substitution Principle
- I : Interface Segmented Principle
- D : Dependency Inversion Principle


* Let's discuss about the S of the SOLID principle

```js
 A class should have only 1 reason to change

 class Marker {
    String name;
    String color;
    int year;
    int price;

    public Marker(String name, String color,int year,int price) {
      this.name = name;
      this.color = color;
      this.year = year;
      this.price = price;
    }
 }

 class Invoice {
    private Marker marker;
    private int quantity;

    public Invoice(Marker marker, int quantity) {
        this.marker = marker;
        this.quantity = quantity;
    }

    public int calculationTotal() {
        int price = ((marker.price)*this.quantity);
        return price;
    }

    public void printInvoice() {
        // print the Invoice
    }

    public void saveToDb() {
        // sava data into DB
    }
 }

 If you will see this class this is not following single responsbility principle.
 So what 'S' says is that a class should have only one reason to be changed.
 Now if someone will come tomorrow and says I need to make some change in printInvoice method or calculationTotal or saveToDb method, then we need to make change in 3 different function that should not happen.
 So what we will do we will create 3 different classes that will handle there respective works independently.


class Invoice {
    private Marker marker;
    private int quantity;

    public Invoice(Marker marker, int quantity) {
        this.marker = marker;
        this.quantity = quantity;
    }

    public int calculationTotal() {
        int price = ((marker.price)*this.quantity);
        return price;
    }
 }
 
 class InvoiceDao {
    Invoice invoice;

    public InvoiceDao(Invoice invoice) {
        this.invoice = invoice;
    }

    public void saveToDb() {
        // sava data into DB
    }
 }

 and similarily we will create a different class to print also and now it is following the single responsbility principle.


```

* Let's discuss O of SOLID Principle

```js
So mantra is : Open for Extension but Closed for Modification

Now let's say we have the same class InvoiceDao and we want add one more way i.e. save the data to file also

class InvoiceDao {
    Invoice invoice;

    public InvoiceDao(Invoice invoice) {
        this.invoice = invoice;
    }

    public void saveToDb() {
        // sava data into DB
    }

    public void saveToFile(String filename) {
        // Save Invoice in the file with the given name
    }
 }

 now what is the problem if we are adding this method in the existing code, this is prone to bugs because this can break the existing runnig code.

 So the solution is to create a Interface and extend the classes

 interface InvoiceDao {
    public void save(Invoice invoice);
 }

 class DatabaseInvoiceDao implements InvoiceDao {
    pubic void save(Invoice invoice) {
        // save in DB
    }
 }

class FileInvoiceDao implements InvoiceDao {
    pubic void save(Invoice invoice) {
        // save in files
    }
 }

```

* L stands for Liskov Substitution Principle(LSP)

```js
This principle is all about making sure that subclasses (child classes) can replace their parent without breaking the program.

public class Bird {
    public virtual void Fly() {
        // normal fly mechanism
    }
}

public class Sparrow : Bird {
    // no issues here sparrow can fly
}

public class Penguin : Bird {
    public override void Fly() {
        throw new Exception("Penguin can't fly");
    }
}

so here this is a problem this code is voilating the LSP, because you are narrowing the parent code scope, you are saying Penguin is a bird but it cannot fly.


public class Bird
{
    public void Eat()
    {
        Console.WriteLine("I am eating!");
    }
}

// Interface for birds that can fly
public interface IFlyable
{
    void Fly();
}

// Sparrow implements IFlyable because it can fly
public class Sparrow : Bird, IFlyable
{
    public void Fly()
    {
        Console.WriteLine("I am flying like a Sparrow!");
    }
}

// Eagle implements IFlyable and defines its own Fly behavior
public class Eagle : Bird, IFlyable
{
    public void Fly()
    {
        Console.WriteLine("I am flying high like an Eagle!");
    }
}

// Penguin does not implement IFlyable because it cannot fly
public class Penguin : Bird
{
    // No Fly() method here, because Penguins can't fly
}

```

* I - Interface Segregation Principle (ISP)

```js
The Interface Segregation Principle (ISP) says:

"A class should not be forced to implement methods it does not use."

👉 This means that instead of having one big interface with too many methods, we should create smaller, more specific interfaces that only include the methods a class actually needs.

=====> Bad code example

public interface IWorker {
    void Work();
    void Eat();
    void Sleep();
}

public class HumanWorker : IWorker {
    // because this is human class they will do all the things
}

public class RobotWorker : IWorker
{
    public void Work()
    {
        Console.WriteLine("I am working like a robot!");
    }

    public void Eat()
    {
        throw new NotImplementedException("I don't eat!");
    }

    public void Sleep()
    {
        throw new NotImplementedException("I don't sleep!");
    }
}

❌ What’s Wrong?
RobotWorker doesn’t need Eat() and Sleep(), but it is forced to implement them.
This violates ISP because an interface should contain only the methods relevant to a class.


Thank you! I'm glad you liked the explanation. Now, let’s dive into the I in SOLID – Interface Segregation Principle (ISP) – in the simplest way possible. 🚀

I - Interface Segregation Principle (ISP)
The Interface Segregation Principle (ISP) says:

"A class should not be forced to implement methods it does not use."

👉 This means that instead of having one big interface with too many methods, we should create smaller, more specific interfaces that only include the methods a class actually needs.

📌 Simple Example (Without Code)
Imagine you go to a restaurant.

There’s a Vegetarian customer who only wants Veg dishes.
There’s a Non-Vegetarian customer who wants both Veg & Non-Veg dishes.
Now, suppose the restaurant has one big menu with both Veg & Non-Veg dishes.

The Vegetarian customer is forced to look at Non-Veg dishes (which they don’t need).
The Non-Vegetarian customer is fine with everything.
🚨 Problem: Not all customers need everything on the menu!

✅ Solution: Separate the menu into:

Veg Menu for vegetarians 🍎
Non-Veg Menu for non-vegetarians 🍗
Now, everyone gets what they need without being forced to look at extra things. This is Interface Segregation!

💻 Bad Code Example (Violating ISP)
Here’s a wrong approach where one big interface forces classes to implement methods they don’t need:

csharp
Copy
Edit
// A big interface with too many methods
public interface IWorker
{
    void Work();
    void Eat();
    void Sleep();
}

// A human worker can do all these things
public class HumanWorker : IWorker
{
    public void Work()
    {
        Console.WriteLine("I am working!");
    }

    public void Eat()
    {
        Console.WriteLine("I am eating!");
    }

    public void Sleep()
    {
        Console.WriteLine("I am sleeping!");
    }
}

// A Robot Worker doesn't eat or sleep, but is forced to implement those methods
public class RobotWorker : IWorker
{
    public void Work()
    {
        Console.WriteLine("I am working like a robot!");
    }

    public void Eat()
    {
        throw new NotImplementedException("I don't eat!");
    }

    public void Sleep()
    {
        throw new NotImplementedException("I don't sleep!");
    }
}
❌ What’s Wrong?
RobotWorker doesn’t need Eat() and Sleep(), but it is forced to implement them.
This violates ISP because an interface should contain only the methods relevant to a class.

✅ Good Code Example (Following ISP)

// Separate interfaces for different responsibilities
public interface IWork
{
    void Work();
}

public interface IEat
{
    void Eat();
}

public interface ISleep
{
    void Sleep();
}

// Human Worker implements everything
public class HumanWorker : IWork, IEat, ISleep
{
    public void Work()
    {
        Console.WriteLine("I am working!");
    }

    public void Eat()
    {
        Console.WriteLine("I am eating!");
    }

    public void Sleep()
    {
        Console.WriteLine("I am sleeping!");
    }
}

// Robot Worker only implements what it needs
public class RobotWorker : IWork
{
    public void Work()
    {
        Console.WriteLine("I am working like a robot!");
    }
}

💡 Don’t make one big interface. Instead, make small, meaningful interfaces!
💡 A class should only implement the methods that are relevant to it.
```

* D - Dependency Inversion Principle (DIP)

```js

High-level modules should not depend on low-level modules. Both should depend on abstractions (interfaces or abstract classes).
💻 Bad Code Example (Violating DIP)

public class WiredMouse {
    public void Connect() {
        Console.WriteLine("Wired Mouse Connected");
    }
}

public class Computer {
    private WiredMouse _mouse;

    public Computer() {
        _mouse = new WiredMouse(); // ❌ Direct dependency on a specific type of mouse
    }
    public void Start() {
        _mouse.Connect();
        Console.WriteLine("Computer Started!")
    }
}

❌ What’s Wrong?
: The Computer class depends directly on WiredMouse.
: If we want to use a WirelessMouse, we have to modify the Computer class, which is bad.
: This violates DIP because the high-level module (Computer) should not directly depend on a low-level module (WiredMouse).

// 1️⃣ Create an abstraction (interface)
public interface IMouse
{
    void Connect();
}

// 2️⃣ Implement different types of mice
public class WiredMouse : IMouse
{
    public void Connect()
    {
        Console.WriteLine("Wired Mouse Connected!");
    }
}

public class WirelessMouse : IMouse
{
    public void Connect()
    {
        Console.WriteLine("Wireless Mouse Connected!");
    }
}

// 3️⃣ Computer class now depends on IMouse (abstraction)
public class Computer
{
    private IMouse _mouse;

    // Inject the dependency via constructor
    public Computer(IMouse mouse)
    {
        _mouse = mouse;
    }

    public void Start()
    {
        _mouse.Connect();
        Console.WriteLine("Computer Started!");
    }
}


public class Program
{
    public static void Main()
    {
        IMouse wiredMouse = new WiredMouse();
        IMouse wirelessMouse = new WirelessMouse();

        Computer computer1 = new Computer(wiredMouse);
        computer1.Start();
        // Output:
        // Wired Mouse Connected!
        // Computer Started!

        Computer computer2 = new Computer(wirelessMouse);
        computer2.Start();
        // Output:
        // Wireless Mouse Connected!
        // Computer Started!
    }
}

✅ Why is This Better?
✅ Loosely Coupled: The Computer class doesn’t care whether it gets a WiredMouse or WirelessMouse.
✅ Easily Extendable: If we add a new BluetoothMouse, we don’t have to modify the Computer class!
✅ Follows Dependency Injection: We pass the dependency (IMouse) via the constructor instead of hardcoding it.
```
