# Strategy Pattern (Composition)

```js
🚨 Problem: Code Duplication Using Inheritance

public class Order
{
    public void ValidateOrder()
    {
        Console.WriteLine("Order validated.");
    }

    public void ProcessPayment()
    {
        Console.WriteLine("Payment processed.");
    }
}

// Standard Delivery Order
public class StandardDeliveryOrder : Order
{
    public void ProcessOrder()
    {
        ValidateOrder();
        ProcessPayment();
        Console.WriteLine("Standard delivery: Delivered in 5-7 days.");
    }
}

// Express Delivery Order
public class ExpressDeliveryOrder : Order
{
    public void ProcessOrder()
    {
        ValidateOrder();
        ProcessPayment();
        Console.WriteLine("Express delivery: Delivered in 2-3 days.");
    }
}

// Same-Day Delivery Order
public class SameDayDeliveryOrder : Order
{
    public void ProcessOrder()
    {
        ValidateOrder();
        ProcessPayment();
        Console.WriteLine("Same-day delivery: Delivered today!");
    }
}

🔴 Problems with This Approach
❌ Code Duplication: ValidateOrder() and ProcessPayment() are repeated in all subclasses.
❌ Hard to Maintain: If we need to change payment logic, we must modify every subclass.
❌ Not Flexible: If we add a new delivery type, we must copy-paste the same logic.

✅ Step 2: Using Strategy Pattern to Remove Code Duplication

public interface IDeliveryStrategy
{
    void Deliver();
}

public class StandardDelivery : IDeliveryStrategy
{
    public void Deliver()
    {
        Console.WriteLine("Standard delivery: Delivered in 5-7 days.");
    }
}

public class ExpressDelivery : IDeliveryStrategy
{
    public void Deliver()
    {
        Console.WriteLine("Express delivery: Delivered in 2-3 days.");
    }
}

public class SameDayDelivery : IDeliveryStrategy
{
    public void Deliver()
    {
        Console.WriteLine("Same-day delivery: Delivered today!");
    }
}



public class Order
{
    private readonly IDeliveryStrategy _deliveryStrategy;

    public Order(IDeliveryStrategy deliveryStrategy)
    {
        _deliveryStrategy = deliveryStrategy;
    }

    public void ProcessOrder()
    {
        ValidateOrder();
        ProcessPayment();
        _deliveryStrategy.Deliver();
    }

    private void ValidateOrder()
    {
        Console.WriteLine("Order validated.");
    }

    private void ProcessPayment()
    {
        Console.WriteLine("Payment processed.");
    }
}


public class Program
{
    public static void Main()
    {
        // Process Standard Delivery Order
        Order standardOrder = new Order(new StandardDelivery());
        standardOrder.ProcessOrder();

        Console.WriteLine();

        // Process Express Delivery Order
        Order expressOrder = new Order(new ExpressDelivery());
        expressOrder.ProcessOrder();

        Console.WriteLine();

        // Process Same-Day Delivery Order
        Order sameDayOrder = new Order(new SameDayDelivery());
        sameDayOrder.ProcessOrder();
    }
}


🚀 We removed code duplication by moving the shared logic to the Order class and used Strategy Pattern for delivery logic!
```