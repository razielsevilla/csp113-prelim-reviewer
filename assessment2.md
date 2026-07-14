Here is another complete practice assessment for you. For this version, I have applied the code structures from your module to a brand new context: an Online Food Ordering Platform 1-20. Your module's assessment task specifically mentions using the Factory Pattern for payment processing and the Observer Pattern for real-time order tracking 20, so this codebase is designed to reflect that exact scenario!

***
### The Unified Base Code: Food Ordering Platform
```java
import java.util.ArrayList;
import java.util.List;

// ==========================================
// 1. SINGLETON PATTERN
// ==========================================
class AppConfigManager {
    private AppConfigManager() {
        System.out.println("Food App Config Initialized");
    }
    private static class SingletonHelper {
        private static final AppConfigManager INSTANCE = new AppConfigManager();
    }
    public static AppConfigManager getInstance() {
        return SingletonHelper.INSTANCE;
    }
    public String getAppName() {
        return "FoodDeliveryPro";
    }
    public String getBaseDeliveryFee() {
        return "50.00";
    }
}

// ==========================================
// 2. FACTORY PATTERN
// ==========================================
interface Payment {
    void processPayment();
}
class CreditCardPayment implements Payment {
    public void processPayment() {
        System.out.println("Processing Credit Card Payment...");
    }
}
class EWalletPayment implements Payment {
    public void processPayment() {
        System.out.println("Processing E-Wallet Payment...");
    }
}
class CashOnDelivery implements Payment {
    public void processPayment() {
        System.out.println("Preparing for Cash on Delivery...");
    }
}
class PaymentFactory {
    public static Payment createPayment(String type) {
        if (type.equalsIgnoreCase("CREDIT_CARD")) return new CreditCardPayment();
        if (type.equalsIgnoreCase("EWALLET")) return new EWalletPayment();
        if (type.equalsIgnoreCase("COD")) return new CashOnDelivery();
        return null;
    }
}

// ==========================================
// 3. OBSERVER PATTERN
// ==========================================
interface OrderObserver {
    void update(String status);
}
interface OrderSubject {
    void registerObserver(OrderObserver o);
    void removeObserver(OrderObserver o);
    void notifyObservers();
}
class OrderTracker implements OrderSubject {
    private List observers = new ArrayList<>();
    private String orderStatus;
    public void registerObserver(OrderObserver o) {
        observers.add(o);
    }
    public void removeObserver(OrderObserver o) {
        observers.remove(o);
    }
    public void notifyObservers() {
        for (OrderObserver observer : observers) {
            observer.update(orderStatus);
        }
    }
    public void setStatus(String status) {
        this.orderStatus = status;
        notifyObservers();
    }
}
class CustomerApp implements OrderObserver {
    private String customerName;
    public CustomerApp(String name) {
        this.customerName = name;
    }
    public void update(String status) {
        System.out.println(customerName + "'s App: Order is " + status);
    }
}
class RestaurantDashboard implements OrderObserver {
    private String restoName;
    public RestaurantDashboard(String name) {
        this.restoName = name;
    }
    public void update(String status) {
        System.out.println(restoName + " Dashboard: Customer order is " + status);
    }
}

// ==========================================
// 4. MODEL-VIEW-CONTROLLER (MVC) PATTERN
// ==========================================
class FoodItem {
    private String name;
    private double price;
    public FoodItem(String name, double price) {
        this.name = name;
        this.price = price;
    }
    public String getName() {
        return name;
    }
    public double getPrice() {
        return price;
    }
    public void setName(String name) {
        this.name = name;
    }
    public void setPrice(double price) {
        this.price = price;
    }
}
class FoodItemView {
    public void printItem(String name, double price) {
        System.out.println("Menu Item: " + name + " | Price: P" + price);
    }
}
class FoodItemController {
    private FoodItem model;
    private FoodItemView view;
    public FoodItemController(FoodItem model, FoodItemView view) {
        this.model = model;
        this.view = view;
    }
    public void setItemName(String name) {
        model.setName(name);
    }
    public void setItemPrice(double price) {
        model.setPrice(price);
    }
    public void updateView() {
        view.printItem(model.getName(), model.getPrice());
    }
}
```

***
### Tracing Assessment: 10 Questions
Instructions: For each main method below, determine the exact output printed to the console based on the unified codebase above.

**Question 1**
```java
public static void main(String[] args) {
    AppConfigManager config = AppConfigManager.getInstance();
    System.out.println("Fee: " + config.getBaseDeliveryFee());
}
```
A) Fee: 50.00
B) Food App Config Initialized Fee: 50.00
C) Food App Config Initialized
D) Compilation Error

**Question 2**
```java
public static void main(String[] args) {
    AppConfigManager config1 = AppConfigManager.getInstance();
    AppConfigManager config2 = AppConfigManager.getInstance();
    System.out.println(config1.getAppName());
}
```
A) Food App Config Initialized FoodDeliveryPro
B) Food App Config Initialized Food App Config Initialized FoodDeliveryPro
C) FoodDeliveryPro
D) Food App Config Initialized

**Question 3**
```java
public static void main(String[] args) {
    Payment p1 = PaymentFactory.createPayment("eWallet");
    p1.processPayment();
}
```
A) Processing E-Wallet Payment...
B) NullPointerException
C) Processing Credit Card Payment...
D) Preparing for Cash on Delivery...

**Question 4**
```java
public static void main(String[] args) {
    Payment p = PaymentFactory.createPayment("DEBIT_CARD");
    if(p != null) {
        p.processPayment();
    } else {
        System.out.println("Payment Method Unavailable");
    }
}
```
A) NullPointerException
B) Processing Credit Card Payment...
C) Payment Method Unavailable
D) Compilation Error

**Question 5**
```java
public static void main(String[] args) {
    Payment p1 = PaymentFactory.createPayment("cod");
    Payment p2 = PaymentFactory.createPayment("CREDIT_CARD");
    p2.processPayment();
    p1.processPayment();
}
```
A) Preparing for Cash on Delivery... Processing Credit Card Payment...
B) Processing Credit Card Payment... Preparing for Cash on Delivery...
C) NullPointerException
D) Processing cod Payment... Processing CREDIT_CARD Payment...

**Question 6**
```java
public static void main(String[] args) {
    OrderTracker tracker = new OrderTracker();
    OrderObserver customer = new CustomerApp("Sarah");
    OrderObserver resto = new RestaurantDashboard("Burger Joint");
    tracker.registerObserver(customer);
    tracker.registerObserver(resto);
    tracker.setStatus("Preparing");
}
```
A) Sarah's App: Order is Preparing Burger Joint Dashboard: Customer order is Preparing
B) Preparing
C) Burger Joint Dashboard: Customer order is Preparing Sarah's App: Order is Preparing
D) Sarah's App: Order is Preparing

**Question 7**
```java
public static void main(String[] args) {
    OrderTracker tracker = new OrderTracker();
    OrderObserver customer = new CustomerApp("John");
    tracker.setStatus("Received");
    tracker.registerObserver(customer);
    tracker.setStatus("Out for Delivery");
}
```
A) John's App: Order is Received John's App: Order is Out for Delivery
B) John's App: Order is Out for Delivery
C) John's App: Order is Received
D) NullPointerException

**Question 8**
```java
public static void main(String[] args) {
    OrderTracker tracker = new OrderTracker();
    OrderObserver resto = new RestaurantDashboard("Pizza Hut");
    OrderObserver customer = new CustomerApp("Mike");
    tracker.registerObserver(resto);
    tracker.registerObserver(customer);
    tracker.removeObserver(resto);
    tracker.setStatus("Delivered");
}
```
A) Pizza Hut Dashboard: Customer order is Delivered Mike's App: Order is Delivered
B) Pizza Hut Dashboard: Customer order is Delivered
C) Mike's App: Order is Delivered
D) Compilation Error

**Question 9**
```java
public static void main(String[] args) {
    FoodItem item = new FoodItem("Fries", 60.00);
    FoodItemView view = new FoodItemView();
    FoodItemController ctrl = new FoodItemController(item, view);
    ctrl.updateView();
    ctrl.setItemPrice(75.00);
    ctrl.updateView();
}
```
A) Menu Item: Fries | Price: P60.0 Menu Item: Fries | Price: P75.0
B) Menu Item: Fries | Price: P75.0
C) Menu Item: Fries | Price: P60.00 Menu Item: Fries | Price: P75.00
D) Menu Item: Fries | Price: P75.0 Menu Item: Fries | Price: P75.0

**Question 10**
```java
public static void main(String[] args) {
    FoodItem item = new FoodItem("Burger", 150.0);
    FoodItemView view = new FoodItemView();
    FoodItemController ctrl = new FoodItemController(item, view);
    ctrl.setItemName("Cheeseburger");
    ctrl.setItemPrice(180.0);
}
```
A) Menu Item: Burger | Price: P150.0
B) Menu Item: Cheeseburger | Price: P180.0
C) NullPointerException
D) (Nothing is printed)

***
### Answer Key
1. B (The private constructor of a Singleton runs the very first time getInstance() is called, printing the initialization message before the fee is requested 2-4.)
2. A (Even though getInstance() is called twice, the Singleton guarantees the instance is only created once, so "Food App Config Initialized" only prints a single time 2-4.)
3. A (The Factory Method uses .equalsIgnoreCase(), so the mixed case "eWallet" successfully instantiates and returns an EWalletPayment object 8, 9.)
4. C ("DEBIT_CARD" is not a defined type in the Factory, so it returns null. The if statement catches the null and prints the fallback message 8, 9.)
5. B (Objects are created, but .processPayment() is called on p2 first, meaning the Credit Card message prints before the COD message 8, 9.)
6. A (Both observers are registered before the status is updated, so both receive the "Preparing" message in the order they were added 13-15.)
7. B ("Received" is set before the customer is registered to the Subject, so the customer only gets notified about "Out for Delivery" 13-15.)
8. C (The restaurant is registered but then immediately removed via removeObserver() before the status is updated, leaving only Mike to receive the notification 13-15.)
9. A (The Controller prints the view, updates the model's price data, and then correctly calls .updateView() a second time to show the updated data 18, 19, 21. Note: Java prints .0 for doubles unless specifically formatted otherwise).
10. D (The controller updates the underlying model data, but .updateView() is never explicitly called in the main method, so nothing is sent to the console 18, 19, 21.)
