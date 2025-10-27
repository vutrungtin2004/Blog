---
title: "Lập trình Hướng Đối Tượng trong Java - OOP Fundamentals"
date: 2024-01-20T14:00:00+07:00
draft: false
description: "Tìm hiểu sâu về 4 tính chất cơ bản của OOP: Encapsulation, Inheritance, Polymorphism, Abstraction trong Java"
tags: ["Java", "OOP", "Object-Oriented", "Lập trình"]
categories: ["Java"]
author: "Trung Tín"
showAuthor: true
showDate: true
showDateUpdated: true
showReadingTime: true
showTableOfContents: true
showPagination: true
showTaxonomies: true
showWordCount: true
---

{{< lead >}}
🎯 Master **4 Pillars của OOP** - Encapsulation, Inheritance, Polymorphism, Abstraction - nền tảng để trở thành Java Developer chuyên nghiệp!
{{< /lead >}}

## Giới thiệu về OOP

{{< alert "star" >}}
**⭐ Foundation of Java:** OOP là **TRÍ TUỆ CỐT LÕI** của Java - mọi framework và library đều build trên 4 nguyên lý này!
{{< /alert >}}

Lập trình Hướng Đối Tượng (Object-Oriented Programming - OOP) là một paradigm lập trình dựa trên khái niệm "objects" - các đối tượng chứa dữ liệu (attributes) và hành vi (methods).

OOP giúp:
- Tổ chức code tốt hơn
- Tái sử dụng code
- Dễ bảo trì và mở rộng
- Mô hình hóa vấn đề thực tế

## 4 Tính chất cơ bản của OOP

{{< mermaid >}}
graph TD
    A[OOP Principles] --> B[Encapsulation]
    A --> C[Inheritance]
    A --> D[Polymorphism]
    A --> E[Abstraction]
    
    B --> F[Data Hiding]
    C --> G[Code Reuse]
    D --> H[Flexibility]
    E --> I[Simplification]
    
    style A fill:#ff6b6b
    style B fill:#51cf66
    style C fill:#339af0
    style D fill:#ff8787
    style E fill:#ffd43b
{{< /mermaid >}}

### 1. Encapsulation (Đóng gói)

{{< badge >}}
🔒 Data Protection
{{< /badge >}}

**Encapsulation** là việc gói gọn dữ liệu (attributes) và các phương thức (methods) thao tác trên dữ liệu đó vào trong một đơn vị duy nhất (class), đồng thời che giấu chi tiết bên trong.

#### Lợi ích:
- Bảo vệ dữ liệu
- Dễ bảo trì
- Tăng tính linh hoạt

#### Ví dụ:

```java
public class BankAccount {
    // Private attributes - ẩn dữ liệu
    private String accountNumber;
    private double balance;
    private String ownerName;
    
    // Constructor
    public BankAccount(String accountNumber, String ownerName) {
        this.accountNumber = accountNumber;
        this.ownerName = ownerName;
        this.balance = 0.0;
    }
    
    // Public methods - interface để tương tác
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Nạp tiền thành công: " + amount + " VND");
        } else {
            System.out.println("Số tiền không hợp lệ!");
        }
    }
    
    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("Rút tiền thành công: " + amount + " VND");
            return true;
        } else {
            System.out.println("Số dư không đủ hoặc số tiền không hợp lệ!");
            return false;
        }
    }
    
    // Getter methods
    public double getBalance() {
        return balance;
    }
    
    public String getAccountNumber() {
        return accountNumber;
    }
    
    public String getOwnerName() {
        return ownerName;
    }
    
    // Display info
    public void displayInfo() {
        System.out.println("=== Thông tin tài khoản ===");
        System.out.println("Số tài khoản: " + accountNumber);
        System.out.println("Chủ tài khoản: " + ownerName);
        System.out.println("Số dư: " + balance + " VND");
    }
}

// Sử dụng
public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount("123456789", "Trung Tín");
        
        account.deposit(1000000);
        account.withdraw(500000);
        account.displayInfo();
        
        // Không thể truy cập trực tiếp: account.balance = 999999; // Error!
        // Phải dùng methods: account.deposit(999999);
    }
}
```

### 2. Inheritance (Kế thừa)

{{< badge >}}
🧬 Code Reuse
{{< /badge >}}

**Inheritance** cho phép một class (subclass/child class) kế thừa attributes và methods từ class khác (superclass/parent class).

{{< alert "lightbulb" >}}
**💡 IS-A Relationship:** Dog IS-A Animal, Car IS-A Vehicle. Inheritance mô hình hóa mối quan hệ "là một" trong thế giới thực!
{{< /alert >}}

#### Lợi ích:
- Tái sử dụng code
- Tạo mối quan hệ "IS-A"
- Dễ mở rộng

#### Ví dụ:

```java
// Parent class (Superclass)
public class Animal {
    protected String name;
    protected int age;
    
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void eat() {
        System.out.println(name + " đang ăn...");
    }
    
    public void sleep() {
        System.out.println(name + " đang ngủ...");
    }
    
    public void makeSound() {
        System.out.println(name + " đang kêu...");
    }
}

// Child class - Dog IS-A Animal
public class Dog extends Animal {
    private String breed;
    
    public Dog(String name, int age, String breed) {
        super(name, age); // Gọi constructor của parent class
        this.breed = breed;
    }
    
    // Override method từ parent class
    @Override
    public void makeSound() {
        System.out.println(name + " kêu: Gâu gâu!");
    }
    
    // Method riêng của Dog
    public void fetch() {
        System.out.println(name + " đang bắt bóng!");
    }
}

// Child class - Cat IS-A Animal
public class Cat extends Animal {
    private boolean isIndoor;
    
    public Cat(String name, int age, boolean isIndoor) {
        super(name, age);
        this.isIndoor = isIndoor;
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " kêu: Meo meo!");
    }
    
    public void scratch() {
        System.out.println(name + " đang cào!");
    }
}

// Sử dụng
public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog("Buddy", 3, "Golden Retriever");
        Cat cat = new Cat("Kitty", 2, true);
        
        dog.eat();          // Kế thừa từ Animal
        dog.makeSound();    // Override method
        dog.fetch();        // Method riêng
        
        cat.eat();          // Kế thừa từ Animal
        cat.makeSound();    // Override method
        cat.scratch();      // Method riêng
    }
}
```

### 3. Polymorphism (Đa hình)

{{< badge >}}
🎭 Many Forms
{{< /badge >}}

**Polymorphism** cho phép một hành động có thể được thực hiện theo nhiều cách khác nhau.

{{< alert "star" >}}
**⭐ Power of Polymorphism:** "One interface, many implementations" - viết code linh hoạt, dễ mở rộng!
{{< /alert >}}

**2 loại Polymorphism:**
- {{< badge >}}Compile-time{{< /badge >}} Method Overloading
- {{< badge >}}Runtime{{< /badge >}} Method Overriding

#### Method Overloading (Nạp chồng phương thức):

```java
public class Calculator {
    // Cùng tên method, khác parameters
    
    public int add(int a, int b) {
        return a + b;
    }
    
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    public double add(double a, double b) {
        return a + b;
    }
    
    public String add(String a, String b) {
        return a + b;
    }
}

// Sử dụng
public class Main {
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        
        System.out.println(calc.add(5, 10));           // 15
        System.out.println(calc.add(5, 10, 15));       // 30
        System.out.println(calc.add(5.5, 10.5));       // 16.0
        System.out.println(calc.add("Hello ", "Java")); // Hello Java
    }
}
```

#### Method Overriding & Runtime Polymorphism:

```java
public class Shape {
    public void draw() {
        System.out.println("Vẽ hình...");
    }
    
    public double calculateArea() {
        return 0;
    }
}

public class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public void draw() {
        System.out.println("Vẽ hình tròn");
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

public class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    @Override
    public void draw() {
        System.out.println("Vẽ hình chữ nhật");
    }
    
    @Override
    public double calculateArea() {
        return width * height;
    }
}

// Sử dụng - Runtime Polymorphism
public class Main {
    public static void main(String[] args) {
        // Polymorphism: Parent reference, Child object
        Shape shape1 = new Circle(5);
        Shape shape2 = new Rectangle(4, 6);
        
        // Method được gọi dựa trên object thực tế (runtime)
        shape1.draw();  // Vẽ hình tròn
        System.out.println("Diện tích: " + shape1.calculateArea());
        
        shape2.draw();  // Vẽ hình chữ nhật
        System.out.println("Diện tích: " + shape2.calculateArea());
        
        // Array of shapes
        Shape[] shapes = {
            new Circle(3),
            new Rectangle(5, 4),
            new Circle(7)
        };
        
        for (Shape shape : shapes) {
            shape.draw();
            System.out.println("Diện tích: " + shape.calculateArea());
        }
    }
}
```

### 4. Abstraction (Trừu tượng)

{{< badge >}}
🎨 Simplification
{{< /badge >}}

**Abstraction** là việc ẩn đi chi tiết implementation và chỉ hiển thị functionality cho người dùng.

{{< alert "lightbulb" >}}
**💡 Real World Analogy:** Bạn lái xe không cần biết engine hoạt động thế nào - chỉ cần biết gas, brake, steering. Đó là abstraction!
{{< /alert >}}

**2 cách để đạt được abstraction:**
- {{< badge >}}Abstract Class{{< /badge >}} 0-100% abstraction
- {{< badge >}}Interface{{< /badge >}} 100% abstraction

#### Abstract Class:

```java
// Abstract class
public abstract class Vehicle {
    protected String brand;
    protected String model;
    
    public Vehicle(String brand, String model) {
        this.brand = brand;
        this.model = model;
    }
    
    // Abstract method - phải override
    public abstract void start();
    public abstract void stop();
    
    // Concrete method - có implementation
    public void displayInfo() {
        System.out.println("Brand: " + brand);
        System.out.println("Model: " + model);
    }
}

public class Car extends Vehicle {
    private int numberOfDoors;
    
    public Car(String brand, String model, int numberOfDoors) {
        super(brand, model);
        this.numberOfDoors = numberOfDoors;
    }
    
    @Override
    public void start() {
        System.out.println("Xe ô tô " + brand + " đang khởi động bằng chìa khóa...");
    }
    
    @Override
    public void stop() {
        System.out.println("Xe ô tô " + brand + " đang dừng lại...");
    }
}

public class Motorcycle extends Vehicle {
    private boolean hasHelmet;
    
    public Motorcycle(String brand, String model, boolean hasHelmet) {
        super(brand, model);
        this.hasHelmet = hasHelmet;
    }
    
    @Override
    public void start() {
        System.out.println("Xe máy " + brand + " đang khởi động bằng điện...");
    }
    
    @Override
    public void stop() {
        System.out.println("Xe máy " + brand + " đang dừng lại...");
    }
}
```

#### Interface:

```java
// Interface
public interface Flyable {
    // Abstract methods (public abstract by default)
    void takeOff();
    void fly();
    void land();
    
    // Default method (Java 8+)
    default void checkWeather() {
        System.out.println("Kiểm tra thời tiết trước khi bay...");
    }
    
    // Static method (Java 8+)
    static void displayInfo() {
        System.out.println("Interface Flyable cho các đối tượng có thể bay");
    }
}

public class Airplane implements Flyable {
    private String model;
    
    public Airplane(String model) {
        this.model = model;
    }
    
    @Override
    public void takeOff() {
        System.out.println("Máy bay " + model + " đang cất cánh...");
    }
    
    @Override
    public void fly() {
        System.out.println("Máy bay " + model + " đang bay ở độ cao 10,000m");
    }
    
    @Override
    public void land() {
        System.out.println("Máy bay " + model + " đang hạ cánh...");
    }
}

public class Bird implements Flyable {
    private String species;
    
    public Bird(String species) {
        this.species = species;
    }
    
    @Override
    public void takeOff() {
        System.out.println("Chim " + species + " đang vỗ cánh cất cánh...");
    }
    
    @Override
    public void fly() {
        System.out.println("Chim " + species + " đang bay tự do");
    }
    
    @Override
    public void land() {
        System.out.println("Chim " + species + " đang đáp xuống cành cây...");
    }
}

// Sử dụng
public class Main {
    public static void main(String[] args) {
        Flyable airplane = new Airplane("Boeing 747");
        Flyable bird = new Bird("Đại bàng");
        
        airplane.checkWeather();
        airplane.takeOff();
        airplane.fly();
        airplane.land();
        
        System.out.println();
        
        bird.checkWeather();
        bird.takeOff();
        bird.fly();
        bird.land();
        
        Flyable.displayInfo(); // Static method
    }
}
```

## So sánh Abstract Class vs Interface

| Abstract Class | Interface |
|----------------|-----------|
| Có thể có concrete methods | Chỉ có abstract methods (trước Java 8) |
| Có thể có attributes | Chỉ có constants (public static final) |
| Single inheritance | Multiple inheritance |
| Có constructor | Không có constructor |
| extends keyword | implements keyword |
| Dùng khi có mối quan hệ "IS-A" | Dùng khi có mối quan hệ "CAN-DO" |

## Kết luận

{{< timeline >}}

{{< timelineItem icon="lock" header="Encapsulation" badge="Protection" >}}
{{< badge >}}private{{< /badge >}} {{< badge >}}getter/setter{{< /badge >}} - Bảo vệ data, control access, easy maintenance!
{{< /timelineItem >}}

{{< timelineItem icon="code-branch" header="Inheritance" badge="Reuse" >}}
{{< badge >}}extends{{< /badge >}} {{< badge >}}super{{< /badge >}} - Tái sử dụng code, xây dựng hierarchy, IS-A relationship!
{{< /timelineItem >}}

{{< timelineItem icon="masks-theater" header="Polymorphism" badge="Flexibility" >}}
{{< badge >}}@Override{{< /badge >}} {{< badge >}}Overloading{{< /badge >}} - One interface, many forms, ultimate flexibility!
{{< /timelineItem >}}

{{< timelineItem icon="wand-magic-sparkles" header="Abstraction" badge="Simplicity" >}}
{{< badge >}}abstract{{< /badge >}} {{< badge >}}interface{{< /badge >}} - Hide complexity, show only what's needed!
{{< /timelineItem >}}

{{< /timeline >}}

{{< alert "check" >}}
**✅ OOP Mastery Checklist:**
- ✅ **Encapsulate** everything - always use private + getter/setter
- ✅ **Inherit** wisely - only when there's true IS-A relationship
- ✅ **Polymorphism** for flexibility - code to interfaces, not implementations
- ✅ **Abstract** when needed - hide complexity, expose simplicity
{{< /alert >}}

## Tài liệu tham khảo

{{< button href="https://docs.oracle.com/javase/tutorial/java/concepts/" target="_blank" >}}
📚 Oracle OOP Concepts
{{< /button >}}

{{< button href="https://www.oreilly.com/library/view/head-first-java/0596009208/" target="_blank" >}}
📖 Head First Java
{{< /button >}}

{{< button href="https://www.oreilly.com/library/view/effective-java/9780134686097/" target="_blank" >}}
📕 Effective Java
{{< /button >}}

