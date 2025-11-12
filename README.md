# Java Encapsulation Lab

## Table of Contents
1. [Data Hiding](#1-data-hiding)
2. [Getters and Setters](#2-getters-and-setters)
3. [Data Validation](#3-data-validation)
4. [Constructors and Initialization](#4-constructors-and-initialization)
5. [Business Logic Methods](#5-business-logic-methods)

## Lab Setup
1. Create a package called `ie.atu.encapsulation`
2. Create a `Main` class inside this package
3. Create a `main` method inside this Main class
4. Place all the below classes from the DIY sections into this package. 

## 1. Data Hiding

### Learning Objective
Learn how to hide data using private access modifiers and understand why it's important for building robust applications.

### Explanation
Data hiding is the fundamental concept of encapsulation where we restrict direct access to certain components of an object, typically by making fields private. This protection prevents unauthorized access to internal data and helps maintain the object's state consistency. By controlling access to our object's data, we can ensure that the object's state is always valid and can't be corrupted by external code.

### Example
```java
public class Counter {
    private int count; // Private field - cannot be accessed directly from outside
    
    public void increment() {
        count++;
    }
    
    public void displayCount() {
        System.out.println("Current count: " + count);
    }
}
```

### Visual Representation
```mermaid
graph TD
    A[External Code] -->|Cannot Access| B[private count]
    A -->|Can Access| C[public methods]
    C -->|Can Access| B
```

### DIY Exercise: Secret Message
Create a `SecretMessage` class that:  
- Has a private String field to store a message. 
- Has a public method to display the message
- Create a `Secret Message` object in the `Main` class
- Try and call the message field directly using the dot operator
- Print the message to the console using the public method

## 2. Getters and Setters

### Learning Objective
Learn how to provide controlled access to private fields using getter and setter methods.

### Explanation
While private fields prevent direct access, we often need controlled ways to read and modify their values through getter and setter methods. Getter methods allow read access to private fields while maintaining encapsulation, and setter methods provide a way to modify private fields with proper validation. This approach gives us the flexibility to change how we store and validate data without affecting code that uses our class.

### Example
```java
public class Person {
    private String name;
    
    // Getter method
    public String getName() {
        return name;
    }
    
    // Setter method
    public void setName(String name) {
        this.name = name;
    }
}
```

### Visual Representation
```mermaid
sequenceDiagram
    External Code->>+Person: setName("John")
    Person->>-Person: name = "John"
    External Code->>+Person: getName()
    Person-->>-External Code: "John"
```

### DIY Exercise: Temperature Converter
Create a `Temperature` class that:
- Stores a temperature in a private double instance variable named `celsius`
- Provides a getter methods for `celsius`
- Provides a setter method that accepts celsius values

## 3. Data Validation

### Learning Objective
Understand how to validate data both in setter methods and constructors, and how to reuse validation logic effectively with helper methods.

### Explanation
Validation ensures that the data inside an object remains correct and meaningful. When encapsulating data, validation can occur in different stages of an object's lifecycle:

 - **Setter** validation ensures only valid data is stored when fields are modified after object creation.
 - **Constructor** validation ensures that objects are created in a valid state from the beginning.

### Example 1: Validation logic in a construcor and setter. Duplication of code. 
```java
public class Student {
    private int age;
    
    public Student(int age) {
        if (age < 16 || age > 100) {
            System.out.println("Invalid age: must be between 16 and 100");
            return;
        }
        this.age = age;
    }
    
    public void setAge(int age) {
        if (age < 16 || age > 100) {
            System.out.println("Invalid age: must be between 16 and 100");
            return;
        }
        this.age = age;
    }
    
    public int getAge() {
        return age;
    }
} 
```

### Example 2: Using Validation Helpers

Using helper methods avoids duplication of validation logic between constructors and setters, improving maintainability and reducing bugs.  

```java
public class Student {
    private int age;

    /* Constructor is a kind of setter too. It initially sets the value of object fields. It too
       can use the same validation as the setter method */  
    public Student(int age) {
        this.age = validateAge(age);
    }
    
    public void setAge(int age) {
        this.age = validateAge(age);
    }

    // Private internal validation method just usable in this class by the constructor and the setter  
    private int validateAge(int age) {
        if (age < 16 || age > 100) {
            System.out.println("Invalid age: must be between 16 and 100");
            return 0;
        }
        return age;
    }
    
    public int getAge() {
        return age;
    }
}
```

### Visual Representation
```mermaid
graph TD
    A[Input Data] -->|Validate| B{Valid?}
    B -->|Yes| C[Update Field]
    B -->|No| D[Throw Exception]
    
    E[Constructor/Setter] -->|Uses| F[Validation Helper]
    F -->|Valid| G[Update Field]
    F -->|Invalid| H[Throw Exception]
```

### DIY Exercise: Grade Book
Create a `Grade` class that:
1. Has these private fields:
   - studentName (String)
   - numericGrade (int)
   - courseCode (String)

2. Implements these validation helper methods:
   - validateStudentName(String name)
   - validateGrade(int grade)
   - validateCourseCode(String code)

3. Uses the helpers in both:
   - Constructor
   - Setter methods

4. Implements these features:
   - Validates that grades are within range (0-100)
   - Validates that student name is not empty
   - Validates that course code matches pattern (e.g., "CS101")
   - Provides a method to get the letter grade (A: 90-100, B: 80-89, etc.)
   - Throws appropriate exceptions with meaningful messages for invalid input

Example structure:
```java
public class Grade {
    private String studentName;
    private int numericGrade;
    private String courseCode;

    // TODO: Add constructor

    // TODO: Add getters and setters

    // TODO: Add validation helper methods

    // TODO: Add method to calculate letter grade
}
```

## 4. Constructors and Initialisation

### Learning Objective
Learn how to properly initialize encapsulated objects using constructors.

### Explanation
Constructors ensure that objects are created in a valid state by properly initialising all necessary fields. A well-designed constructor validates its parameters and establishes class invariants, preventing the creation of invalid objects. The constructor represents the first line of defense in maintaining encapsulation, as it controls how objects are created and initialized.

### Example
```java
public class Car {
    private String make;
    private String model;
    private int year;
    
    // Constructor with validation
    public Car(String make, String model, int year) {
        if (make == null || make.trim().isEmpty()) {
            throw new IllegalArgumentException("Make cannot be empty");
        }
        if (model == null || model.trim().isEmpty()) {
            throw new IllegalArgumentException("Model cannot be empty");
        }
        if (year < 1886) { // First automobile was invented in 1886
            throw new IllegalArgumentException("Invalid year");
        }
        
        this.make = make;
        this.model = model;
        this.year = year;
    }
    
    // Getters
    public String getMake() { return make; }
    public String getModel() { return model; }
    public int getYear() { return year; }
}
```

### Visual Representation
```mermaid
sequenceDiagram
    Client->>+Car: new Car("Toyota", "Camry", 2024)
    Car->>Car: Validate make
    Car->>Car: Validate model
    Car->>Car: Validate year
    Car->>Car: Initialize fields
    Car-->>-Client: New Car object
```

### DIY Exercise: Book Catalog
Create a `Book` class that:
- Has private fields for ISBN, title, and author
- Has a constructor that validates all fields
- Provides only getter methods (immutable object)
- Throws exceptions for invalid input

## 5. Business Logic Methods

### Learning Objective
Learn how to implement methods that operate on encapsulated data while maintaining object consistency.

### Explanation
Business logic methods perform operations using the object's private data while maintaining encapsulation and data integrity. These methods encapsulate not just data but also the behavior and rules that apply to that data. By keeping business logic within the class, we ensure that all operations on the data follow the same rules and maintain consistency.

### Example
```java
public class Employee {
    private String name;
    private double salary;
    private int yearsOfService;
    
    // Constructor
    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
        this.yearsOfService = 0;
    }
    
    // Business logic methods
    public void giveRaise(double percentage) {
        if (percentage <= 0) {
            throw new IllegalArgumentException("Percentage must be positive");
        }
        salary += salary * (percentage / 100);
    }
    
    public void completeServiceYear() {
        yearsOfService++;
        if (yearsOfService % 5 == 0) {
            giveRaise(5); // 5% raise every 5 years
        }
    }
    
    public double calculateBonus() {
        return salary * (yearsOfService / 10.0);
    }
    
    // Getters
    public String getName() { return name; }
    public double getSalary() { return salary; }
    public int getYearsOfService() { return yearsOfService; }
}
```

### Visual Representation
```mermaid
classDiagram
    class Employee {
        -String name
        -double salary
        -int yearsOfService
        +giveRaise(percentage)
        +completeServiceYear()
        +calculateBonus()
    }
```

### DIY Exercise: Bank Account Manager
Create an `Account` class that:
- Stores account balance and transaction history
- Implements deposit and withdrawal methods
- Calculates interest based on balance
- Provides account statement generation
- Tracks and limits daily transactions



## Summary
This lab covered the essential concepts of encapsulation in Java:
1. Basic data hiding using private fields
2. Controlled access through getters and setters
3. Data validation for maintaining integrity
4. Proper initialization with constructors
5. Business logic methods for operations

## Further Reading
- Java Documentation: [Access Control](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)
- Book: Effective Java by Joshua Bloch
- Book: Clean Code by Robert C. Martin
