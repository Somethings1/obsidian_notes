# Code that smells

---

# Long method

---

> [!important] Any method longer than 10 lines should make you start asking question.

Mentally, it’s often harder to create a new method than adding to an existing one.

- Among all types of object-oriented code, classes with short methods live longest. The longer a method or function is, the harder it becomes to understand and maintain it.
- In addition, long methods offer the perfect hiding place for unwanted duplicate code.

## How to fix?

If you feel the need to comment on something inside a method, you should take this code and put it in another method. Even a single line of code. Give it a descriptive, so that nobody ever need to see what it does.

### Extract method

When have a code fragment that can be grouped together, then do it

```Java
void printOwing() {
  printBanner();

  // Print details.
  System.out.println("name: " + name);
  System.out.println("amount: " + getOutstanding());
}
```

⇒

```Java
void printOwing() {
  printBanner();
  printDetails(getOutstanding());
}

void printDetails(double outstanding) {
  System.out.println("name: " + name);
  System.out.println("amount: " + outstanding);
}
```

### Replace temp with query

If the code fragment use temp variable, and that variable is also used in other places, then change that temp into another method, and use it like a query.

```Java
double calculateTotal() {
  double basePrice = quantity * itemPrice;
  if (basePrice > 1000) {
    return basePrice * 0.95;
  }
  else {
    return basePrice * 0.98;
  }
}
```

⇒

```Java
double calculateTotal() {
  if (basePrice() > 1000) {
    return basePrice() * 0.95;
  }
  else {
    return basePrice() * 0.98;
  }
}
double basePrice() {
  return quantity * itemPrice;
}
```

### Introduce parameter objects

If your method contains a repeating group of parameter, replace it with a parameter object

```Java
void amountInvoicedIn (Date start, Date end);
void amountReceivedIn (Date start, Date end);
void amountOverdueIn (Date start, Date end);
```

⇒

```Java
void amountInvoicedIn (DateRange date);
void amountReceivedIn (DateRange date);
void amountOverdueIn (DateRange date);
```

### Preserve whole object

You get several values from an object, then pass them as parameters. Instead, try passing the whole object

```Java
int low = daysTempRange.getLow();
int high = daysTempRange.getHigh();
boolean withinPlan = plan.withinRange(low, high);
```

⇒

```Java
boolean withinPlan = plan.withinRange(daysTempRange);
```

### Replace method with method object

You have a long method in which the local vars are so intertwined that can’t apply **Extract method** ⇒ transform the method into a separate class so that the local variables become fields of the class. Then you can split the method.

```Java
class Order {
  // ...
  public double price() {
    double primaryBasePrice;
    double secondaryBasePrice;
    double tertiaryBasePrice;
    // Perform long computation.
  }
}
```

⇒

```Java
class Order {
  // ...
  public double price() {
    return new PriceCalculator(this).compute();
  }
}

class PriceCalculator {
  private double primaryBasePrice;
  private double secondaryBasePrice;
  private double tertiaryBasePrice;
  
  public PriceCalculator(Order order) {
    // Copy relevant information from the
    // order object.
  }
  
  public double compute() {
    // Perform long computation.
  }
}
```

### Decompose conditional

If you have a complex conditional, then decompose the complicated parts of the condition into separate methods

```Java
if (date.before(SUMMER_START) || date.after(SUMMER_END)) {
  charge = quantity * winterRate + winterServiceCharge;
}
else {
  charge = quantity * summerRate;
}
```

⇒

```Java
if (isSummer(date)) {
  charge = summerCharge(quantity);
}
else {
  charge = winterCharge(quantity);
}
```

# Long class

---

Classes usually start small. But over time, they get bloated as the program grows.

As is the case with long methods as well, programmers usually find it mentally less taxing to place a new feature in an existing class than to create a new class for the feature.

## How to fix?

### Extract class

When a class does the work of two, split it.

![[/image 23.png|image 23.png]]

![[/image 1 5.png|image 1 5.png]]

### Extract subclass

When a class has features that are used only in certain cases, make those subclass.

![[/image 2 5.png|image 2 5.png]]

  

![[/image 3 5.png|image 3 5.png]]

### Extract interface

Multiple clients are using parts of a class interface.

![[/image 4 4.png|image 4 4.png]]

![[/image 5 3.png|image 5 3.png]]

### Duplicate observed data

If a large class is responsible for GUI, try to move some of its data and behavior to a separate domain object.

![[/image 6 3.png|image 6 3.png]]

  

![[/image 7 3.png|image 7 3.png]]

# Primitive obsession

---

Like most other smells, primitive obsessions are born in moments of weakness. "Just a field for storing some data!" the programmer said. Creating a primitive field is so much easier than making a whole new class, right? And so it was done. Then another field was needed and added in the same way. Lo and behold, the class became huge and unwieldy.

Primitives are often used to "simulate" types. So instead of a separate data type, you have a set of numbers or strings that form the list of allowable values for some entity. Easy-to-understand names are then given to these specific numbers and strings via constants, which is why they're spread wide and far.

## How to fix?

### Replace data value with object

A class contains a field or a set of fields. The field has its own behaviors and associated data

![[/image 8 2.png|image 8 2.png]]

![[/image 9 2.png|image 9 2.png]]

### Replace type code with class

A class has a field that contains type code. Create a new class and use its objects instead of the type code value.

![[/image 10 2.png|image 10 2.png]]

![[/image 11 2.png|image 11 2.png]]

### Replace type code with subclasses

You have a coded type that directly affects program’s behaviors. Then create subclass for each type, extract the related behaviors to these subclasses.

![[/image 12 2.png|image 12 2.png]]

![[/image 13 2.png|image 13 2.png]]

### Replace array with object

You have an array contains various types of data. Then replace it with an object that has separate fields for it.

```Java
String[] row = new String[2];
row[0] = "Liverpool";
row[1] = "15";
```

⇒

```Java
Performance row = new Performance();
row.setName("Liverpool");
row.setWins("15");
```

# Long parameter list

---

More than 2 parameters for a method

## How to fix?

### Replace parameter with Method call

Check if a value passed to a parameter is just the result of some method call, then replace it.

```Java
int basePrice = quantity * itemPrice;
double seasonDiscount = this.getSeasonalDiscount();
double fees = this.getFees();
double finalPrice = discountedPrice(basePrice, seasonDiscount, fees);
```

⇒

```Java
int basePrice = quantity * itemPrice;
double finalPrice = discountedPrice(basePrice);
```

# Data clumps

Sometimes different parts of code contain identical group of variables. These clumps should be turned into their own class.

Often these data groups are due to poor program structure or "copypasta programming”.

If you want to make sure whether or not some data is a data clump, just delete one of the data values and see whether the other values still make sense. If this isn't the case, this is a good sign that this group of variables should be combined into an object.

## How to fix?

Move those to a new data class

# Switch statement

---

Relatively rare use of `switch` and `case` operators is one of the hallmarks of object-oriented code. Often code for a single `switch` can be scattered in different places in the program. When a new condition is added, you have to find all the `switch` code and modify it.

As a rule of thumb, when you see `switch` you should think of polymorphism.

## How to fix?

### Isolate `switch` operator

Use or

### Get rid of Type code

Use or

### Replace conditional with polymorphism

```Java
class Bird {
  // ...
  double getSpeed() {
    switch (type) {
      case EUROPEAN:
        return getBaseSpeed();
      case AFRICAN:
        return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
      case NORWEGIAN_BLUE:
        return (isNailed) ? 0 : getBaseSpeed(voltage);
    }
    throw new RuntimeException("Should be unreachable");
  }
}
```

⇒

```Java
abstract class Bird {
  // ...
  abstract double getSpeed();
}

class European extends Bird {
  double getSpeed() {
    return getBaseSpeed();
  }
}
class African extends Bird {
  double getSpeed() {
    return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
  }
}
class NorwegianBlue extends Bird {
  double getSpeed() {
    return (isNailed) ? 0 : getBaseSpeed(voltage);
  }
}

// Somewhere in client code
speed = bird.getSpeed();
```

### **Replace Parameters** `**switch**`**ing with Explicit Methods**

If there aren't too many conditions in the operator and they all call same method with different parameters, polymorphism will be superfluous. If this case, you can break that method into multiple smaller methods with **Replace Parameter with Explicit Methods** and change the `switch` accordingly.

```Java
void setValue(String name, int value) {
  if (name.equals("height")) {
    height = value;
    return;
  }
  if (name.equals("width")) {
    width = value;
    return;
  }
  Assert.shouldNeverReachHere();
}
```

⇒

```Java
void setHeight(int arg) {
  height = arg;
}
void setWidth(int arg) {
  width = arg;
}
```

### Introduce `null` object

If some of the conditional is `null`, change it

```Java
if (customer == null) {
  plan = BillingPlan.basic();
}
else {
  plan = customer.getPlan();
}
```

⇒

```Java
class NullCustomer extends Customer {
  boolean isNull() {
    return true;
  }
  Plan getPlan() {
    return new NullPlan();
  }
  // Some other NULL functionality.
}

// Replace null values with Null-object.
customer = (order.customer != null) ?
  order.customer : new NullCustomer();

// Use Null-object as if it's normal subclass.
plan = customer.getPlan();
```

# Temporary fields

---

_Temporary fields get their values (and thus are needed by objects) only under certain circumstances. Outside of these circumstances, they're empty._

Oftentimes, temporary fields are created for use in an algorithm that requires a large amount of inputs. So instead of creating a large number of parameters in the method, the programmer decides to create fields for this data in the class. These fields are used only in the algorithm and go unused the rest of the time.

This kind of code is tough to understand. You expect to see data in object fields but for some reason they're almost always empty.

## How to fix?

### Extract temporary fields and related behaviors

Use or

  

# Refused bequest

---

_If a subclass uses only some of the methods and properties inherited from its parents, the hierarchy is off-kilter. The unneeded methods may simply go unused or be redefined and give off exceptions._

Someone was motivated to create inheritance between classes only by the desire to reuse the code in a superclass. But the superclass and subclass are completely different.

```Java
class Dog {
	void move(x, y);
}

class Car extends Dog {}
// extends to use the move() method
```

## How to fix?

### Replace inheritance with delegation

When have a subclass that uses only a portion of of the method of its superclass, create a field and put a superclass in it, delegate the method to the superclass, and get rid of the inheritance

![[/image 14 2.png|image 14 2.png]]

![[/image 15 2.png|image 15 2.png]]

  

### Extract superclass

If the two classes have some shared fields and methods, but inheritance here is just alien, then create a superclass for both

![[/image 16 2.png|image 16 2.png]]

![[/image 17 2.png|image 17 2.png]]

# Alternative classes with different interfaces

---

Two classes perform identical functions with different method names.

The programmer who created one of the classes probably didn't know that a functionally equivalent class already existed.

## How to fix?

### Rename method

Rename method to make them identical in all alternative classes. Let the name describe exactly what it does.

### Move method

If a method is used more in another class than its own class, then move it.

### Add parameters

If a method lack the data to perform some action, add it.

### Parameterized method

Multiple methods perform the same actions that are different only in their internal value, number of operations, combine those

![[/image 18 2.png|image 18 2.png]]

![[/image 19 2.png|image 19 2.png]]

### Extract only identical parts

By

# Divergent change

---

_You find yourself having to change many unrelated methods when you make changes to a class. For example, when adding a new product type you have to change the methods for finding, displaying, and ordering products._

## How to fix?

### Combine classes through inheritance

By and

# Shotgun surgery

---

_Making any modifications requires that you make many small changes to many different classes._

A single responsibility has been split up among a large number of classes. This can happen after overzealous application of **Divergent Change**

## How to fix?

### Consolidate responsibility in a single class

Use or Move Field

### Inline class

If a class does almost nothing and isn’t responsible for anything, and no additional responsibilities are planned for it, then move all features from the class to another one.

![[/image 20 2.png|image 20 2.png]]

![[/image 21 2.png|image 21 2.png]]

# Parallel Inheritance Hierarchies

---

Merge class hierarchies by Move Method and Move Field

# Comments

---

If you feel that a code fragment can't be understood without comments, try to change the code structure in a way that makes comments unnecessary.

## How to fix?

### Extract variables

If an expression is hard to understand, break it into understandable segments.

```Java
void renderBanner() {
  if ((platform.toUpperCase().indexOf("MAC") > -1) &&
       (browser.toUpperCase().indexOf("IE") > -1) &&
        wasInitialized() && resize > 0 )
  {
    // do something
  }
}
```

⇒

```Java
void renderBanner() {
  final boolean isMacOs = platform.toUpperCase().indexOf("MAC") > -1;
  final boolean isIE = browser.toUpperCase().indexOf("IE") > -1;
  final boolean wasResized = resize > 0;

  if (isMacOs && isIE && wasInitialized() && wasResized) {
    // do something
  }
}
```

### Rename method

### Introduce Assertion

For a part of code to work correctly, certain conditions must be true. Introduce some assertion for that.

```Java
double getExpenseLimit() {
  // Should have either expense limit or
  // a primary project.
  return (expenseLimit != NULL_EXPENSE) ?
    expenseLimit :
    primaryProject.getMemberExpenseLimit();
}
```

⇒

```Java
double getExpenseLimit() {
  Assert.isTrue(expenseLimit != NULL_EXPENSE || primaryProject != null);

  return (expenseLimit != NULL_EXPENSE) ?
    expenseLimit:
    primaryProject.getMemberExpenseLimit();
}
```

# Duplicate code

---

Two segments of code look almost identical.

Duplication usually occurs when multiple programmers are working on different parts of the same program at the same time. Since they're working on different tasks, they may be unaware their colleague has already written similar code that could be repurposed for their own needs.

There's also more subtle duplication, when specific parts of code look different but actually perform the same job. This kind of duplication can be hard to find and fix.

## How to fix?

### Extract method

Use when the duplication is found in 2 methods of the same class

### Pull up field

![[/image 22 2.png|image 22 2.png]]

![[/image 23 2.png|image 23 2.png]]

### Pull up constructor body

Subclasses have constructors look almost identical

```Java
class Manager extends Employee {
  public Manager(String name, String id, int grade) {
    this.name = name;
    this.id = id;
    this.grade = grade;
  }
  // ...
}
```

⇒

```Java
class Manager extends Employee {
  public Manager(String name, String id, int grade) {
    super(name, id);
    this.grade = grade;
  }
  // ...
}
```

### Form template method

If the code in two subclasses look similar but not identical, use this

![[image 24.png]]

![[image 25.png]]

### Substitute algorithm

If two method in subclasses do the same with different algorithm, choose the more efficient.

### Extract superclass

If you have two unrelated classes with some common fields and methods, make them inherit another class

![[image 26.png]]

![[image 27.png]]

# Lazy class

---

If a class doesn't do enough to earn you attention, it should be deleted. Pẻhaps a class was designed to be fully functional but after some refactoring, it becomes ridiculously small.

# Data class

Class with only fields and methods to modify the fields. It cannot do anything specific with its fields.

## How to fix?

### Encapsulated field

Use getter and setter to access the field instead of leaving a public field

### Encapsulate collection

![[image 28.png]]

![[image 29.png]]

### Move client code closer to the Data

Sometimes it’s better to move the client code inside the data class.