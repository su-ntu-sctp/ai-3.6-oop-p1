# Lesson: Introduction to Object-Oriented Programming (OOP) in Java

# Lesson Overview  

This lesson introduces the core principles of Object-Oriented Programming (OOP) in Java. Learners will move from basic variables and methods to designing reusable classes with attributes, constructors, and encapsulation. The lesson also covers `static` members, overriding `toString()`, and data modeling using POJOs and records through hands-on examples like `Car` and `VendingMachine`.

# Lesson Objectives  

- Understand the purpose of classes and objects in organizing code.  
- Create and use constructors, including overloaded and copy types.  
- Apply encapsulation with private fields, getters, and setters.  
- Differentiate instance and static members and their usage.  
- Override `toString()` for better object representation.  
- Model data using POJOs and records in practical exercises.


## Part 1: Class Attributes and Methods

Create an `App.java` and code along.

We had been defining variables and methods previously in our code.

Let's define some variables to represent a person and `static` methods to greet and eat.

```java
public class App {
  public static void main(String[] args) {

    String name1 = "Tony Stark";
    int age1 = 35;
    String gender1 = "male";
    boolean isMarried1 = true;

    String name2 = "Bruce Banner";
    int age2 = 36;
    String gender2 = "male";
    boolean isMarried2 = false;

    greet(name1, age1, gender1, isMarried1);
    eat(name1, "burger");

    greet(name2, age2, gender2, isMarried2);
    eat(name2, "pizza");
  }

  public static void greet(String name, int age, String gender, boolean isMarried) {
    System.out.println("Hello world, I'm " + name + " and I'm a " + age + " year old " + gender + ".");
  }

  public static void eat(String name, String meal) {
    System.out.println(name + " is currently eating " + meal);
  }
}

```

If you observe the above code, these variables are related to each other. They are all attributes of a person.

We can group them together in a class so that we can create multiple objects of the same type. This is easier than having to define variables separately for each person. More importantly, this will make our code more organized and easier to maintain.

### Class

A class is a blueprint for creating objects. It defines the attributes and methods that an object will have.

There can only be one public class in a file. The name of the file should match the name of the public class.

Create a `Person.java` file and define a `Person` class.

```java
public class Person {

}
```

#### Access Modifiers for Classes

Classes also have access modifiers. The access modifier controls the visibility of the class.

| Access Keyword | Description                        |
| -------------- | ---------------------------------- |
| `public`       | accessible from anywhere           |
| blank          | accessible from within the package |

### Methods and Attributes

When we talk about objects in the real world e.g. a car, a dog, a house, etc. we can describe it in terms of its **state** and its **behaviour**.

For example, a dog has state like its color, weight, breed, etc. It also has behaviours like bark, eat, run and play.

In Java, the state of an object is represented by its **attributes** and its behaviour is represented by its **methods**.

Let's add some attributes and methods to our Person class.

```java
public class Person {

  String name;
  int age;
  String gender;
  boolean isMarried;

  public void greet() {
    System.out.println("Hello world, I'm " + name + " and I'm a " + age + " year old " + gender + ".");
  }

  public void eat(String meal) {
    System.out.println(name + " is currently eating " + meal);
  }
}
```

We can instantiate the `Person` class in our `main` method and invoke the instance methods.

```java
public class App {
  public static void main(String[] args) {
    Person personA = new Person();

    personA.greet();
    personA.eat("burger");

  }
}
```

Observe the output. Is the result expected?

---

## Part 2: Constructors

A **constructor** is a special method that is called when an object is instantiated i.e. when the `new` keyword is used. The purpose of a constructor is to initialize values for the newly created object.

A constructor:

- is defined using the same syntax as a method
- does not have a return type.
- name must be the same as the class name.

Currently, we do not have a constructor defined in our `Person` class. But Java provides a default constructor that does not take in any parameters - this is called the **no-argument constructor**. Since there are no arguments, this constructor will initialize the attributes to their default values.

This is why we could instantiate the `Person` object without any errors. However, the attributes are not initialized to the values that we want.

We could also explicitly declare the no-argument constructor in our class.

```java
public class Person {

  // Explicitly declaring the no-argument constructor
  public Person() {

  }

}
```

Let's add a **parameterized constructor** to our `Person` class.

This can be done by right-clicking in VSCode, "Source Action", "Generate Constructors".

```java
public Person(String name, int age, String gender, boolean isMarried) {
  this.name = name;
  this.age = age;
  this.gender = gender;
  this.isMarried = isMarried;
}
```

With the constructor defined, we can now instantiate the `Person` object with the values that we want.

```java
public class App {
  public static void main(String[] args) {
    Person tony = new Person("Tony Stark", 35, "male", false);

    tony.greet();
    tony.eat("burger");

  }
}
```

Note that if an explicit constructor is defined, there will no longer be a default constructor implicitly defined.

Remove the no-argument constructor and try to instantiate the `Person` object again without any arguments. What happens?

### What is `this`?

The `this` keyword is used to refer to the current instance in a method or constructor.

In the `Person` class, we now have a constructor that takes in 4 parameters.

```java
public Person(String name, int age, String gender, boolean isMarried)
```

However, the parameters have the same name as the attributes. How does Java know e.g., whether the `name` variable refers to the argument or the attribute?

This is where the `this` keyword comes in. The `this` keyword refers to the current instance of the object. It can be used to refer to the attributes and methods of the object.

Using a constructor with arguments, we can initialize the attributes of the object with the values passed in.

### Constructor Overloading

We can define multiple constructors for a class. This is called **constructor overloading**.

In our previous lesson, we learned method overloading. The constructor is also a method, so we can apply the same concept here.

For example, we can define a constructor to take in two parameters and set default values for the other attributes.

```java
public Person(String name, int age) {
  this.name = name;
  this.age = age;
  this.gender = "Unknown";
  this.isMarried = false;
}
```

We could improve this further by calling the all-argument constructor from the two-argument constructor. Remember that our constructor is a method, so we can call it from another method.

```java
public Person(String name, int age) {
  this(name, age, "Unknown", false);
}
```

Test it out by instantiating the `Person` object with two arguments.

```java
Person bruce = new Person("Bruce Banner", 40);

bruce.greet();
bruce.eat("popcorn");
```

### Copy Constructor

A **copy constructor** is a constructor that takes in an object of the same class and copies its values to the new object. This is actually just another overloaded constructor.

```java
public Person(Person person) {
  this.name = person.name;
  this.age = person.age;
  this.gender = person.gender;
  this.isMarried = person.isMarried;
}
```

Or we can call the all-argument constructor.

```java
public Person(Person person) {
  this(person.name, person.age, person.gender, person.isMarried);
}
```

Then to invoke the copy constructor, we pass in an object of the same class.

```java
Person bruceClone = new Person(bruce);
```

---

## 👨‍💻 Activity

Create a `Car` class with the following attributes:

- `String make`
- `String model`
- `int year`
- `double price`

Create the following constructors:

- no-argument constructor
- constructor that takes in `make`, `model`, `year` and `price`
- constructor that takes in `make`, `model` and `year` and sets the price to 0
- copy constructor

---

## Part 3: Encapsulation, Accessor and Mutator Methods

In the previous lesson, we learned how to use access modifiers to control access to methods. We can also use access modifiers to control access to attributes. We refer to class attributes and methods as **class members**.

| Access Keyword | Description                                     |
| -------------- | ----------------------------------------------- |
| `public`       | accessible from anywhere                        |
| `private`      | accessible only from within the class           |
| `protected`    | accessible from within the class and subclasses |
| blank          | accessible from within the class and package    |

Currently, we can access attributes directly from the `App` class.

```java
System.out.println(tony.name);
System.out.println(tony.age);
System.out.println(tony.gender);
System.out.println(tony.isMarried);
```

Generally, we should keep all fields private because we do not want other classes to be able to access and modify the values directly.

```java
private String name;
private int age;
private String gender;
private boolean isMarried;
```

The moment we do this though, we will get an error in the `App` class because we can no longer access the attributes directly. This is because the attributes are now set to `private`.

How do we access them then? This is where **accessor** and **mutator** methods come in. These are more commonly known as **getters** and **setters** respectively. These methods are used to access and modify the values of the attributes.

A getter method is used to retrieve the value of an attribute. It is a public method that returns the value of the attribute.

```java
public String getName() {
  return name;
}
```

A setter method is used to change the value of an attribute. It is a public method that takes in a parameter and sets the value of the attribute.

```java
public void setName(String name) {
  this.name = name;
}
```

The naming convention for getters and setters is to prefix the attribute name with `get` and `set` respectively, except for boolean attributes. For boolean attributes, the prefix should be `is` instead of `get` e.g. `isMarried`.

In VSCode, we do not have to type these out but can instead generate them by right-clicking, "Source Action", "Generate Getters and Setters".

```java
public String getName() {
  return name;
}

public void setName(String name) {
  this.name = name;
}

public int getAge() {
  return age;
}

public void setAge(int age) {
  this.age = age;
}

public String getGender() {
  return gender;
}

public void setGender(String gender) {
  this.gender = gender;
}

public boolean isMarried() {
  return isMarried;
}

public void setMarried(boolean isMarried) {
  this.isMarried = isMarried;
}
```

Now, we can access the attributes using these methods.

```java
System.out.println(tony.getName());
System.out.println(tony.getAge());
System.out.println(tony.getGender());
System.out.println(tony.isMarried());
```

We could also set the values using the setter methods.

```java
tony.setName("Tony Stuck");
tony.setAge(25);
```

It is good practice to keep fields private and provide getters and setters to access and modify the values because it allows us to control how the values are accessed and modified.

This is one of the principles of Object Oriented Programming (OOP) - **Encapsulation**.

<img src="https://files.prepinsta.com/2023/05/Encapsulation.webp" width=500>

Source: https://prepinsta.com/java/encapsulation/

In OOP, encapsulation means two things:

1. The bundling of methods and attributes on a single object
2. The hiding of the internal representation, or state, of an object from the outside

The methods are usually public because we want to give users a way to interact with the object. The attributes are usually private because we do not want users to be able to access and modify the values directly.

This encapsulates our class which allows us to provide a public interface that never changes while the implementation details can change at any time without affecting the users.

For example, we might change the implementation of the `greet` method include the marital status. Or we might change the implementation of the `isMarried` attribute to return a string instead of a boolean.

```java
public void greet() {
  System.out.println("Hello world, I'm " + getName() + " and I'm a " + getAge() + " year old " + getGender() + ".");
  System.out.println(isMarried() ? "I'm married." : "I'm not married.");
}
```

To the users, the interface remains the same regardless of the changes. They can still call the `greet` method and get the same result.

---

## Part 4: `static` Keyword

In the previous lesson we used the `static` keyword on methods. We can also use it on attributes.

Just like methods, `static` attributes are accessed using the class name while instance attributes are accessed using the object name.

Let's define a `static` attribute.

```java
public static final String scientificName = "Homo Sapien";
```

The `final` keyword is used to restrict modification of the variable.

And access them in our `main` method.

```java
// Printing out a static variable
System.out.println(Person.scientificName);
```

Let's add another static variable to keep track of the number of Person objects instantiated.

```java
private static int personCount = 0;
```

And a corresponding static method to get the value.

```java
public static int getPersonCount() {
  return personCount;
}
```

Now, we will increment it every time a new Person is instantiated.

```java
public Person(String name, int age, String gender, boolean isMarried) {
  this.name = name;
  this.age = age;
  this.gender = gender;
  this.isMarried = isMarried;
  personCount++;
}
```

We can now instantiate a few more persons and use the static method to display the count.

```java
Person peter = new Person("Peter Parker", 20, "male", false);
Person wanda = new Person("Wanda Maximoff", 25, "female", true);

System.out.println(Person.getPersonCount());

// Should not use instance to call a static method
System.out.println(peter.getPersonCount());
```

It is usually not recommended to call a static member with an instance variable instead of the class name. This is because it can be confusing and make the code less readable.

Static members are associated with the class, not with any particular instance of the class. Therefore, it is more consistent to access them using the class name. This also makes it clear that the member is static, which can be important for understanding the code.

---

## Part 5: `toString` and `@Override`

By default, if we try to print out an object, we will get the class name and the hashcode.

```java
System.out.println(tony);
```

We can override the `toString` method to return a string representation of the object. With VSCode, we can generate the `toString` method by right-clicking, "Source Action", "Generate toString()".

```java
@Override
public String toString() {
  return "Person [name=" + name + ", age=" + age + ", gender=" + gender + ", isMarried=" + isMarried + "]";
}
```

The `@Override` annotation is used to indicate that the method is overriding a method from the superclass. This is optional but it is good practice to include it. We will learn more about annotations in later lessons.

---

## 👨‍💻 Activity

Continue with the `Car` class from the previous activity.

1. Encapsulate the attributes and provide accessor and mutator methods.
1. Add a `static` attribute to keep track of the number of cars instantiated and a corresponding static method to get the value.
1. Override the `toString` method to return a string representation of the object.

---

## Part 6: Intro to the POJO

A Plain Old Java Object (POJO) is a simple object used to represent data in a Java program.

We define it using the `class` keyword with a set of attributes and corresponding getters and setters.

Take for example, an expense object.

```java
public class Expense {
  private String name;
  private double amount;

  public Expense() {

  }

  public Expense(String name, double amount) {
    this.name = name;
    this.amount = amount;
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public double getAmount() {
    return amount;
  }

  public void setAmount(double amount) {
    this.amount = amount;
  }
}
```

We could then create an `ArrayList` of expenses.

```java
ArrayList<Expense> expenses = new ArrayList<>();

expenses.add(new Expense("Food", 100));
expenses.add(new Expense("Transport", 500));
System.out.println(expenses);
```

But we will not be able to see the data. This is because the `toString` method is not overridden in the `Expense` class. So we can either override the `toString` method or loop through the `ArrayList` and print out the values.

By using a POJO, we can easily create objects to represent our data instead of using arrays or ArrayLists of strings or numbers. We could also use it to pass data between components or store data in a database.

---

## Part 7: Intro to the Record

The Record is a new feature introduced in Java 14. It is a new type of class that is designed to be a simple and concise way to create classes whose main purpose is to hold data.

Creating a record is similar to creating a POJO. We use the `record` keyword instead of the `class` keyword.

```java
public record Expense(String name, double amount) {}
```

This `Record` has two attributes - `name` and `amount`. These attributes are `private` and `final` by default. This means that they cannot be modified once the object is created.

It also has a constructor that takes in two parameters and sets the values of the attributes.

To access the fields, we can use the getter methods, which are automatically generated.

```java
Expense expense = new Expense("Food", 10.5);

System.out.println(expense.name());
System.out.println(expense.amount());
```

The `toString` method is also automatically generated. We can print out the previous `ArrayList` of expenses and see the data.

---

## 👨‍💻 Activity

With our new knowledge of classes, let's work on the VendingMachine project again.

This time, we will create a `VendingMachine` class to represent the vending machine.

It should have the following attributes:

- `String location` - location of vending machine
- `double earnings` - total earnings of machine
- `double balance` - how much money user has inserted

It should have a constructor that takes the location as a parameter and initializes the earnings and balance to 0.

Methods:

- `insertCoin` - for receiving payment
- `selectDrink` - for selecting a drink
- `getPrice` - for getting the price of a drink
- `dispenseDrink` - for dispensing a drink
- `printEarnings` - prints out the total earnings

Test code:

```java
VendingMachine ntuVendingMachine = new VendingMachine("NTU");

ntuVendingMachine.insertCoin(1);
ntuVendingMachine.selectDrink(Drink.COKE); // should not dispense, insufficient payment
ntuVendingMachine.insertCoin(0.5);
ntuVendingMachine.selectDrink(Drink.COKE);

ntuVendingMachine.insertCoin(0.5); // should not dispense, insufficient payment
ntuVendingMachine.selectDrink(Drink.WATER);
ntuVendingMachine.insertCoin(1);
ntuVendingMachine.selectDrink(Drink.WATER);

ntuVendingMachine.printEarnings();
```

Optional Challenge: Store the transactions history.

---

END