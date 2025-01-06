import java.util.ArrayList;
import java.util.List;

// Abstract Class: User
abstract class User {
    protected int userId;
    protected String name;
    protected String email;

    public User(int userId, String name, String email) {
        this.userId = userId;
        this.name = name;
        this.email = email;
    }

    public abstract void displayDetails();

    public String getEmail() {
        return email;
    }
}

// Customer Class
class Customer extends User {
    private List<Order> orders = new ArrayList<>();
    private String shippingAddress;

    public Customer(int userId, String name, String email, String shippingAddress) {
        super(userId, name, email);
        this.shippingAddress = shippingAddress;
    }

    @Override
    public void displayDetails() {
        System.out.println("Customer ID: " + userId);
        System.out.println("Name: " + name);
        System.out.println("Email: " + email);
        System.out.println("Shipping Address: " + shippingAddress);
        System.out.println("Orders Count: " + orders.size());
    }

    public void placeOrder(Product product, int quantity, String paymentMethod) {
        if (product.getStock() >= quantity) {
            double totalPrice = product.getPrice() * quantity;
            Payment payment;
            if (paymentMethod.equalsIgnoreCase("CreditCard")) {
                payment = new CreditCardPayment("1234-5678-9012-3456", "John Doe");
            } else {
                payment = new PayPalPayment(email);
            }
            payment.processPayment(totalPrice);

            product.reduceStock(quantity);
            Order order = new Order("ORD" + (orders.size() + 1), this, product, quantity, totalPrice, "Placed");
            orders.add(order);
            System.out.println("Order placed successfully!");
        } else {
            System.out.println("Insufficient stock!");
        }
    }

    public void cancelOrder(Order order) {
        if (orders.contains(order)) {
            order.cancelOrder();
            orders.remove(order);
            System.out.println("Order cancelled successfully!");
        } else {
            System.out.println("Order not found!");
        }
    }
}

// Admin Class
class Admin extends User {
    private List<Product> products = new ArrayList<>();

    public Admin(int userId, String name, String email) {
        super(userId, name, email);
    }

    @Override
    public void displayDetails() {
        System.out.println("Admin ID: " + userId);
        System.out.println("Name: " + name);
        System.out.println("Email: " + email);
        System.out.println("Products Managed: " + products.size());
    }

    public void addProduct(Product product) {
        products.add(product);
        System.out.println("Product added successfully!");
    }

    public void removeProduct(String productId) {
        products.removeIf(product -> product.getProductId().equals(productId));
        System.out.println("Product removed successfully!");
    }

    public void updateStock(String productId, int newStock) {
        for (Product product : products) {
            if (product.getProductId().equals(productId)) {
                product.setStock(newStock);
                System.out.println("Stock updated successfully!");
                return;
            }
        }
        System.out.println("Product not found!");
    }
}

// Product Class
class Product {
    private String productId;
    private String name;
    private double price;
    private int stock;

    public Product(String productId, String name, double price, int stock) {
        this.productId = productId;
        this.name = name;
        this.price = price;
        this.stock = stock;
    }

    public String getProductId() {
        return productId;
    }

    public double getPrice() {
        return price;
    }

    public int getStock() {
        return stock;
    }

    public void setStock(int stock) {
        this.stock = stock;
    }

    public void reduceStock(int quantity) {
        stock -= quantity;
    }

    public void increaseStock(int quantity) {
        stock += quantity;
    }

    public void displayDetails() {
        System.out.println("Product ID: " + productId);
        System.out.println("Name: " + name);
        System.out.println("Price: $" + price);
        System.out.println("Stock: " + stock);
    }
}

// Order Class
class Order {
    private String orderId;
    private Customer customer;
    private Product product;
    private int quantity;
    private double totalPrice;
    private String status;

    public Order(String orderId, Customer customer, Product product, int quantity, double totalPrice, String status) {
        this.orderId = orderId;
        this.customer = customer;
        this.product = product;
        this.quantity = quantity;
        this.totalPrice = totalPrice;
        this.status = status;
    }

    public void cancelOrder() {
        status = "Cancelled";
        product.increaseStock(quantity);
    }

    public void displayOrderDetails() {
        System.out.println("Order ID: " + orderId);
        System.out.println("Customer: " + customer.name);
        System.out.println("Product: " + product.getProductId());
        System.out.println("Quantity: " + quantity);
        System.out.println("Total Price: $" + totalPrice);
        System.out.println("Status: " + status);
    }
}

// Payment Interface
interface Payment {
    void processPayment(double amount);
}

// CreditCardPayment Class
class CreditCardPayment implements Payment {
    private String cardNumber;
    private String cardHolderName;

    public CreditCardPayment(String cardNumber, String cardHolderName) {
        this.cardNumber = cardNumber;
        this.cardHolderName = cardHolderName;
    }

    @Override
    public void processPayment(double amount) {
        System.out.println("Processing credit card payment of $" + amount);
    }
}

// PayPalPayment Class
class PayPalPayment implements Payment {
    private String email;

    public PayPalPayment(String email) {
        this.email = email;
    }

    @Override
    public void processPayment(double amount) {
        System.out.println("Processing PayPal payment of $" + amount);
    }
}

// Main Class: ShoppingSystem
public class ShoppingSystem {
    public static void main(String[] args) {
        // Setup Admins
        Admin admin1 = new Admin(1, "Alice", "alice@shop.com");
        Admin admin2 = new Admin(2, "Bob", "bob@shop.com");

        // Setup Products
        Product product1 = new Product("P001", "Laptop", 999.99, 10);
        Product product2 = new Product("P002", "Phone", 499.99, 20);
        Product product3 = new Product("P003", "Tablet", 299.99, 15);

        admin1.addProduct(product1);
        admin1.addProduct(product2);
        admin1.addProduct(product3);

        // Setup Customers
        Customer customer1 = new Customer(1, "Charlie", "charlie@gmail.com", "123 Street, NY");
        Customer customer2 = new Customer(2, "Diana", "diana@gmail.com", "456 Avenue, LA");

        // Simulate Orders
        customer1.placeOrder(product1, 2, "CreditCard");
        customer2.placeOrder(product2, 5, "PayPal");

        // Admin actions
        admin1.updateStock("P001", 8);
        admin2.removeProduct("P003");

        // Display Details
        admin1.displayDetails();
        product1.displayDetails();
        customer1.displayDetails();
    }
}
