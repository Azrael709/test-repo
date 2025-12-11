那我直接把这个 doc 里的 **Module 4 / 5 / 6 全部核心知识点**给你整理成一份复习提纲式笔记，方便你考前刷一遍 👇 

---

## 目录（你可以当 checklist 用）

1. 数组 Arrays
2. 类与对象进阶（字段、构造方法、this、方法）
3. 继承 Inheritance（extends、super、多态、重写 vs 重载）
4. 抽象类 & 接口（abstract class, interface）
5. Object 类、`toString`、`equals`、`instanceof` 和类型转换
6. UML：类图（关系、multiplicity、aggregation / composition）
7. UML：序列图 Sequence Diagram

---

## 1. 数组 Arrays

### 1.1 概念与声明初始化

* **数组 = 相同类型变量的有序集合，是一个对象**
* 每个元素有一个 **索引 index，从 0 开始**。
* 声明语法：

  ```java
  data_type[] variableName;
  String[] names;
  int[] numbers;
  ```
* 初始化语法（指定长度，用 `new`）：

  ```java
  names = new String[10];   // 10 个元素，索引 0~9
  int[] numbers = new int[12];
  ```
* 声明 + 初始化 + 赋初值（array initialiser）：

  ```java
  String[] words = new String[] {"Hello", "World", "Hi"};
  int[] arrayA = {1, 2, 3, 4, 5, 6}; // 省略 new 的简写
  ```

### 1.2 默认值（primitive vs object）

* **基本类型数组**元素默认值：

  * `int` → `0`
  * `double` → `0.0`
  * `boolean` → `false`
  * `char` → `'\u0000'`
* **对象类型数组**元素默认值：

  * 所有元素一开始都是 `null`

  ```java
  String[] names = new String[12]; // 全部是 null
  names[0] = "Alice";
  names[7] = "Bob";
  ```

### 1.3 length 属性 & 遍历数组

* 每个数组对象都有 **`length` 字段**：元素个数

  ```java
  int[] nums = new int[100];
  int len = nums.length; // 100
  ```
* 访问最后一个元素：`array[array.length - 1]`
* 用普通 `for` 遍历并修改元素：

  ```java
  for (int i = 0; i < nums.length; i++) {
      nums[i]++; // 给每个元素 +1
  }
  ```
* 用增强 for-each 遍历（读取更方便，不能直接改原数组下标）：

  ```java
  for (int num : nums) {
      System.out.println(num);
  }
  ```

### 1.4 数组是对象：引用 vs 复制

* 数组变量**存的是引用**，不是完整数组内容。
* **引用赋值**（两个变量指向同一个数组）：

  ```java
  int[] arrayA = {1, 2, 3, 4, 5, 6};
  int[] arrayB;
  arrayB = arrayA;  // 指向同一个数组
  // 修改 arrayB[0] 会影响 arrayA[0]
  ```
* **真正复制数组内容**（元素一一拷贝）：

  ```java
  int[] arrayA = {1, 2, 3, 4, 5, 6};
  int[] arrayB = new int[arrayA.length];
  for (int i = 0; i < arrayB.length; i++) {
      arrayB[i] = arrayA[i];
  }
  ```
* 记住：**数组名是“地址”，赋值只是两个人共用同一块内存**。

---

## 2. 类与对象进阶（Chocolate 为例）

### 2.1 类的一般结构

```java
public class ClassName {
    // 字段 fields（状态 state）
    // 构造方法 constructors
    // 方法 methods（行为 behaviour）
}
```

* **状态 / 字段 fields**：保存对象信息
* **构造方法 constructor**：创建对象时初始化状态
* **方法 methods**：定义对象能做什么

### 2.2 字段：实例变量 vs 静态变量 vs 常量

以 `Chocolate` 为例：

```java
public class Chocolate {
    private int code;              // 实例变量
    private String description;    // 实例变量
    private double price;          // 实例变量

    public static int quantityInStock;        // 静态变量
    public static final String CHOCOLATE_TYPE = "White"; // 常量
}
```

* **实例变量 instance variable**：

  * 每个对象一份，描述“这个对象”的状态。
* **静态变量 static variable（类变量）**：

  * 所有对象共享一份，用 `ClassName.variable` 访问。
* **常量 static final**：

  * 值不能修改，通常全大写命名。

### 2.3 默认值 & null

* 对象创建时，所有实例变量自动获得默认值（同上）。
* 对于对象类型，如果不指向任何对象 → `null`：

  ```java
  Chocolate bar = null;
  bar = new Chocolate(1, "Snickers", 1.80);
  ```
* ⚠ 禁止对 `null` 调用方法，否则 `NullPointerException`。

### 2.4 构造方法 Constructor

作用：**初始化实例变量**。

```java
public class Chocolate {
    private int code;
    private String description;
    private double price;

    // 带参构造
    public Chocolate(int c, String desc, double p) {
        code = c;
        description = desc;
        price = p;
    }
}
```

要点：

* 构造方法名 = 类名，没有返回类型。
* 可以**重载**多个构造，只要参数列表不同。
* 如果你 **没写任何构造**，Java 会自动给一个 **无参默认构造**。
* 一旦你自己写了构造，默认无参构造就**消失**，要自己再写一个。

### 2.5 `this` 关键字

* `this` 代表“**当前这个对象**”。
* 常用场景：构造方法参数名与字段名相同时区分：

```java
public Chocolate(int code, String description, double price) {
    this.code = code;
    this.description = description;
    this.price = price;
}
```

### 2.6 实例方法：getter / setter / toString / equals

以完整 `Chocolate` 为例（简化）：

```java
public class Chocolate {
    private int code;
    private String description;
    private double price;

    public Chocolate(int code, String description, double price) {
        this.code = code;
        this.description = description;
        this.price = price;
    }

    // getter
    public double getPrice() {
        return price;
    }

    // setter
    public void setDescription(String desc) {
        description = desc;
    }

    // 一般方法
    public void assignRandomCode() {
        code = (int) (Math.random() * 1000);
    }

    public void print() {
        System.out.println("Description: " + description + ", Price: $" + price);
    }

    // toString：返回“对象的字符串表示”
    public String toString() {
        return "This Chocolate " + description + " is delicious.";
    }

    // equals：判断两个对象是否“相等”
    public boolean equals(Object other) {
        if (other instanceof Chocolate) {
            Chocolate otherC = (Chocolate) other;
            return this.price == otherC.price
                && this.description.equals(otherC.description)
                && this.code == otherC.code;
        }
        return false;
    }
}
```

* **getter / accessor**：读取字段（一般叫 `getXxx`）。
* **setter / mutator**：修改字段（一般叫 `setXxx`）。
* `toString()`：调 `System.out.println(obj)` 时会自动调用。
* `equals(Object o)`：定制“相等”的含义，配合 `instanceof` 和强制类型转换。

### 2.7 创建对象 & 调用方法

```java
public class ChocolateFactory {
    public void start() {
        Chocolate moroBar = new Chocolate(1, "Moro bar", 1.50);
        Chocolate snickersBar = new Chocolate(2, "Snickers", 1.80);

        snickersBar.print();

        moroBar.print();
        moroBar.setDescription("Moro Bar Delight");
        moroBar.print();

        if (!moroBar.equals(snickersBar)) {
            System.out.println(moroBar); // 自动调用 toString()
        }
    }
}
```

---

## 3. 继承 Inheritance（Module 5）

### 3.1 概念

* **继承 inheritance**：让一个类复用另一个类的字段和方法，形成 **“is-a” 关系**：

  * `Dog is an Animal`
  * `Rectangle is a Shape`
* 术语：

  * **父类 / 超类 superclass**：被继承的类（如 `Animal`）
  * **子类 / 子类型 subclass**：继承的类（如 `Dog extends Animal`）

### 3.2 基本语法：`extends`

```java
public class Animal { ... }

public class Dog extends Animal { ... }

public class Cat extends Animal { ... }
```

特性：

* 子类**继承父类所有 `public` 和 `protected` 字段与方法**。
* 构造方法 **不会继承**。
* Java **只支持单继承**：一个类只能 `extends` 一个父类，但可以有很多子类。
* 可以多层继承：`Cat extends Feline extends Animal`。

### 3.3 构造方法与 `super`

* 创建子类对象时：**先执行父类构造，再执行子类构造**。
* 如果父类有无参构造，Java 会默认在子类构造开头加 `super();`。
* 如果父类**只有有参构造**，子类构造的第一行必须**手动调用**其中一个：

```java
public class Animal {
    private int numLegs;
    private boolean canFly;

    public Animal(int numLegs, boolean canFly) {
        this.numLegs = numLegs;
        this.canFly = canFly;
    }
}

public class Cat extends Animal {
    public Cat() {
        super(4, false); // 调父类构造
    }
}

public class Bird extends Animal {
    public Bird(boolean canFly) {
        super(2, canFly);
    }
}
```

### 3.4 方法重写 overriding vs 重载 overloading

**重写 overriding**（子类改写父类实现）：

* 条件：

  * 方法名相同
  * 参数列表相同
  * 返回类型兼容
* 用 `@Override` 标明（推荐）：

```java
public class Animal {
    public void sayHello() {
        System.out.println("...");
    }
}

public class Dog extends Animal {
    @Override
    public void sayHello() {
        System.out.println("Woof");
    }
}

public class Cat extends Animal {
    @Override
    public void sayHello() {
        System.out.println("Meow");
    }
}
```

**重载 overloading**（同一个类中，方法名相同但参数列表不同）：

* 与继承无关。
* 例如多个构造、多个 `print(...)`。

> 考点：**Overriding = 子类改父类；Overloading = 同名不同参。**

### 3.5 多态 Polymorphism & 动态绑定

**多态 = “同一个父类类型，运行时表现不同的子类行为”**。

```java
Animal a = new Animal();
Animal cat = new Cat();
Animal dog = new Dog();

a.sayHello();    // Animal 的 ...
cat.sayHello();  // 调用 Cat 的 sayHello
dog.sayHello();  // 调用 Dog 的 sayHello
```

要点：

* **变量的编译时类型 = Animal**，但**运行时对象类型可能是 Dog / Cat**。
* **真正被调用的方法是根据“对象的实际类型”来决定的**（dynamic dispatch / 动态分派）。
* 方法查找过程（method lookup）：

  1. 先从对象实际所属的类开始找该方法；
  2. 没找到就往父类链上找。

多态的例子（参数是父类类型）：

```java
public class AnimalDaycare {
    public void deposit(Animal animal) {
        System.out.print("It said: ");
        animal.sayHello(); // 具体调用哪个 sayHello 取决于传入对象类型
    }
}
```

---

## 4. 抽象类 & 接口（Abstract class & Interface）

### 4.1 抽象类 abstract class

用途：**不想在父类里给出具体实现，但强制子类必须实现某些方法**。

```java
public abstract class Animal {
    public abstract void sayHello(); // 抽象方法，没有方法体
}
```

* 抽象方法：关键字 `abstract`，**只有方法签名，没有实现**，后面直接分号。
* 包含抽象方法的类必须声明为 `abstract`。
* 子类如果是 **具体类（非 abstract）**，必须实现父类的所有抽象方法。

例子：

```java
public class Tyrannosaurus extends Animal {
    @Override
    public void sayHello() {
        System.out.println("RAWR");
    }
}
```

* 抽象类**不能被实例化**：不能 `new Animal()`；
* 但可以作为**引用类型**使用：

```java
Animal tRex = new Tyrannosaurus(); // OK
```

### 4.2 `super` 关键字的两种用法

1. **调用父类方法**：

   ```java
   public class Animal {
       public void eat(Food food) {
           System.out.println(getName() + " ate the " + food.getName());
       }
   }

   public class Tyrannosaurus extends Animal {
       @Override
       public void eat(Food food) {
           if (food.isMeat()) {
               super.eat(food); // 调用父类版本
           } else {
               System.out.println(getName() + " spat out the " + food.getName());
           }
       }
   }
   ```

2. **调用父类构造方法**（前面已讲）：`super(参数...)` 必须是构造函数第一行。

### 4.3 接口 interface

**接口 = 只写“方法应该有什么”，不写“方法怎么做”的蓝图**。

```java
public interface Shape {
    public double getArea();
}
```

* 接口中：

  * 方法**默认都是 `public abstract`**（即使不写）。
  * 字段只能是 **`public static final` 常量**。
* 类实现接口：`implements`

  * 类必须实现接口中所有方法。

例子：

```java
public class Rectangle implements Shape {
    private double width, height;

    @Override
    public double getArea() {
        return width * height;
    }
}

public class Circle implements Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double getArea() {
        return Math.PI * radius * radius;
    }
}
```

* 接口类型变量可以指向实现类对象：

```java
Shape c1 = new Circle(2.0);
Shape c2 = new Circle(3.0);

printArea(c1);
printArea(c2);

private void printArea(Shape shape) {
    System.out.println("Area is " + shape.getArea());
}
```

### 4.4 多接口与接口继承

* 一个类只能 `extends` 一个父类，但可以 `implements` 多个接口：

```java
public interface Shape {
    double getArea();
}

public interface Polygon {
    int getNumSides();
}

public class Rectangle implements Shape, Polygon {
    private double width, height;

    @Override
    public double getArea() {
        return width * height;
    }

    @Override
    public int getNumSides() {
        return 4;
    }
}
```

* 接口也可以 `extends` 其他接口：

```java
public interface Polygon extends Shape {
    int getNumSides();
}

public class Rectangle implements Polygon {
    ...
}
```

### 4.5 抽象类 vs 接口（考试超高频对比）

**抽象类（abstract class）：**

* 可以有：

  * 非 `static`、非 `final` 的字段（真正的“对象状态”）
  * `public / protected / private` 的具体方法
  * 构造方法
* 适用场景：

  * 一组**紧密相关**的类，有很多**共有代码**可以放在父类里复用；
  * 需要有非 public 方法或需要保留状态。

**接口（interface）：**

* 所有字段都 **自动 `public static final`**；
* 所有方法都 **自动 `public abstract`**；
* 没有构造方法，不能保存实例状态（只能依靠实现类的字段）。
* 适用场景：

  * 可能由**完全不相关的类**来实现，但它们都需要遵守某个“行为规范”；
  * 希望利用**多重继承“类型”的能力**（一个类实现多个接口）。

---

## 5. Object 类、`toString`、`equals`、`instanceof` & Casting

### 5.1 Object 类

* 如果一个类没有写 `extends`，默认就 `extends java.lang.Object`。
* 所有类都继承 Object 的方法，如：

  * `toString()`
  * `equals(Object obj)`

**`toString()` 默认行为：**返回“类名@内存地址”。
一般会**重写**：

```java
@Override
public String toString() {
    return "This animal is a " + this.getName();
}
```

**`equals(Object obj)` 默认行为：**像 `==` 一样比较“是不是同一个对象地址”。
通常我们希望比较“内容”，所以要重写：

```java
@Override
public boolean equals(Object obj) {
    return this.getName().equals(((Animal)obj).getName());
}
```

### 5.2 `instanceof` & 类型转换 casting

**`instanceof`**：判断对象是否是某个类型（或其子类型）。

```java
@Override
public boolean equals(Object obj) {
    if (obj instanceof Animal) {
        Animal other = (Animal) obj;
        return (other.numLegs == this.numLegs
            && other.canFly == this.canFly
            && other.getName().equals(this.getName()));
    } else {
        return false;
    }
}
```

流程模式（非常典型的写法）：

1. 用 `instanceof` 看 `obj` 是不是某种类型；
2. 如果是，进行 **向下转型 cast**：`(Animal) obj`；
3. 再访问该类型的字段/方法。

⚠ 如果把不兼容的类型强转，会抛 `ClassCastException`。所以要先用 `instanceof` 检查。

---

## 6. UML 基本概念 & 类图 Class Diagram

### 6.1 UML 是什么

* UML = **Unified Modelling Language**，软件建模的标准符号。
* 常见图：

  * Use case diagram（用例图，用户 & 功能）
  * Class diagram（类图，类 & 接口 & 关系）
  * Sequence diagram（序列图，对象交互顺序）
  * State diagram（状态图，一个对象状态变化）
  * Deployment diagram（部署图，软件部署在硬件上）

### 6.2 类图中的类和接口表示

* 类框分三层：

  1. 类名
  2. 字段（变量）
  3. 方法（只写签名即可）

* 可见性符号：

  * `+` public
  * `-` private
  * `#` protected

* 变量写法：`name : Type`

* 方法写法：`methodName(paramType) : ReturnType`

* 接口：在类名上方写 `<<interface>>`，或将其标注为接口。

### 6.3 关系类型（重点）

#### 1）继承 / 泛化（Generalisation）

* 用 **实线 + 空心三角箭头** 指向父类。
* 例：`Rectangle` 继承 `Shape`：

```java
abstract class Shape { … }
class RectangleShape extends Shape { … }
```

#### 2）实现 / Realisation

* 类实现接口：**虚线 + 空心三角箭头** 指向接口。
* `GraphicsPainter` 实现 `Painter` 接口。

#### 3）依赖 / Dependency

* 一个类把另一个类作为 **参数 / 返回值 / 局部变量** 使用。
* 图中用 **虚线箭头** 从“使用者”指向“被使用的类”。

```java
public class Y {
    public void m1(X param) { ... }  // 参数中使用 X
    public X m2() { ... }            // 返回值使用 X
    public void m3() { 
        X localVar;                  // 局部变量使用 X
    }
}
```

#### 4）关联 / Association

* 类与类之间“知道对方”的关系 → **通常由实例变量实现**。
* 画法：两类之间的**实线**。

例：Employer – Employee：

```java
public class Employer {
    private Employee[] myEmployees;
}

public class Employee {
    private Employer myEmployer;
}
```

* 双向关联：两个类互相持有对方引用。

* 单向关联：只有一方持有另一方引用（减少耦合）。

* 可以给关联**命名**（动词），如 `owns`、`registers`。

* 也可以给关联两端**角色名 role**（名词），如人是车的 `registeredKeeper`。

#### 5）多重度 Multiplicity（1, 0..*, 1..5 等）

表示“一端对象能和另一端多少个对象关联”：

* `1` : 恰好 1 个
* `0..1`：0 或 1 个
* `*` 或 `0..*`：任意多个
* `1..5`：1 到 5 个

例：Borrower – Book：

* 一个 `Book` **最多关联一个** Borrower → multiplicity `0..1`
* 一个 `Borrower` **最多借 5 本书** → multiplicity `0..5`（或 `*` + 代码限制）

代码中体现：

```java
public class Borrower {
    private static final int BORROWING_CAPACITY = 5;
    private List<Book> booksOnLoan;

    public boolean canBorrow() {
        return booksOnLoan.size() < BORROWING_CAPACITY;
    }

    public boolean borrow(Book b) {
        if ((!canBorrow()) || (b.isOnLoan())) {
            return false;
        }
        booksOnLoan.add(b);
        b.setBorrowedBy(this);
        return true;
    }
}

public class Book {
    private Borrower borrowedBy; // 0 或 1 个 Borrower

    public Book() {
        borrowedBy = null;
    }

    void setBorrowedBy(Borrower b) {
        borrowedBy = b;
    }

    public boolean isOnLoan() {
        return borrowedBy != null;
    }
}
```

#### 6）聚合 Aggregation（空心菱形）

* 特殊的 **has-a / part-of** 关系。
* 整体和部分是“**弱拥有**”：部分可以脱离整体单独存在，也可以属于多个整体。
* 图：空心菱形在“整体”这一端。
* 例：`Degree` – `Course`（课程可以被不同 degree 共享）。

#### 7）组合 Composition（实心菱形）

* 更强的“整体–部分”关系：

  * 部分**不能单独存在**；
  * 部分**不能同时属于多个整体**；
  * 整体被删除，部分也随之消失。
* 图：实心菱形在“整体”这一端。
* 例：一个 `Degree` 内部特定设计的 `Course` 只能属于这个 degree。

**总结：**

* Association：普通“知道对方”的关系；
* Aggregation：整体–部分，但部分可独立存在；
* Composition：整体–部分，部分不能脱离整体存在，是“强 has-a”。

### 6.4 好的类图应该怎样？

* 不需要把所有细节画上（可以省略 setter/getter、一些类等）。
* 画图前想清楚目的：

  * 用来设计数据库 → 细节多一点；
  * 用来实现类 → 可以多加注释说明方法逻辑；
  * 用来高层展示系统结构 → 只画类 + 关系就够了。

---

## 7. UML 序列图 Sequence Diagram

### 7.1 基本符号

* **对象 Object**：

  * `object`：不指定类型
  * `object:Class`：特定对象及其类型
  * `:Class`：未命名对象，只有类型
* **生命线 lifeline**：对象下面的一条竖虚线，表示时间轴。
* **激活条 focus of control**：生命线上小长方形，表示对象在处理某个操作。
* **消息 message**：

  * 实线箭头：方法调用（`m()`）
  * 虚线箭头：返回值（可省略）

### 7.2 使用场景

* 展示“为了完成某个场景（如借书、计算利息），对象之间是如何一步步互相调用方法的”。
* 可以在图中表示：

  * **条件**（带 guard 条件 ` [condition]`）
  * **循环**（用 `loop` 或 `*` 标记重复消息）

例子里有：

* 客户、账户等对象之间如何调用 `applyInterest()`，
* 如何创建新的对象（在图中用箭头指向新对象的生命线起点）。

---

如果你愿意，下一步我可以：

* 帮你把这一套知识点再 **出一份中英双语小测题**，
* 或者针对你觉得最不熟的部分（比如 `instanceof` + casting / UML 关系）单独整理一页“超速记忆卡”。


我直接用 **“一段英文 + 一段中文”** 的方式来讲，这样你也可以顺便练英文 😊

---

## 1. What is an abstract class?

### 1.1 English

An **abstract class** is a class that **cannot be instantiated** (you cannot do `new Animal()` if `Animal` is abstract).
It is used to represent a **general concept** and to **share common code** for its subclasses.

Key points:

* Declared with the keyword `abstract`.
* Can have:

  * **abstract methods** (no body, must be overridden by subclasses)
  * **normal methods** (with method body)
  * fields (instance variables), constructors, etc.
* Subclasses `extend` the abstract class and **inherit code**.

Example:

```java
public abstract class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }

    // abstract method: no body
    public abstract void makeSound();

    // normal method: has body
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
}

public class Dog extends Animal {

    public Dog(String name) {
        super(name);   // call abstract class constructor
    }

    @Override
    public void makeSound() {
        System.out.println("Woof!");
    }
}
```

You can’t do:

```java
Animal a = new Animal("xx"); // ❌ error: Animal is abstract
```

But you can:

```java
Animal a = new Dog("Buddy");  // ✅ OK (polymorphism)
a.makeSound();                // Woof!
a.sleep();                    // Buddy is sleeping
```

---

### 1.2 中文解释

**抽象类（abstract class）** 是一种 **不能直接创建对象** 的类（如果 `Animal` 是抽象类，就不能写 `new Animal()`）。
它用来表示 **一个比较抽象的概念**，并且为子类 **提供公共的代码**（字段、普通方法、构造方法等）。

重点：

* 用 `abstract` 关键字声明。
* 可以包含：

  * **抽象方法**（只有方法名，没有方法体；子类必须重写）
  * **普通方法**（有方法体，可以直接用）
  * 成员变量、构造方法等。
* 子类通过 `extends` 继承抽象类，并可以 **复用/重写** 其中的代码。

上面例子中：

* `Animal` 是抽象类，有一个抽象方法 `makeSound()`，还有一个普通方法 `sleep()`。
* `Dog` 继承 `Animal`，必须重写 `makeSound()`，可以直接使用 `sleep()`。
* 抽象类 **不能 new**，但是可以作为 **引用类型**，指向子类对象：`Animal a = new Dog("Buddy");`。

---

## 2. What is an interface?

### 2.1 English

An **interface** is a type that specifies **what a class can do**, but not (traditionally) how it does it.
It is like a **contract**: any class that `implements` the interface must provide implementations for its methods.

Key points (Java 8+):

* Declared with `interface`.
* Methods are **implicitly public and abstract**, unless they are default/static methods.
* Can contain:

  * abstract methods
  * `default` methods (with body)
  * `static` methods
  * constants (`public static final` fields)
* A class `implements` an interface.
* A class can implement **multiple interfaces** (supports multiple inheritance of type).

Example (similar to your labs with `INoise`, `IProductionAnimal`):

```java
public interface INoise {
    String makeNoise();   // implicitly public abstract
}

public class Cat implements INoise {

    @Override
    public String makeNoise() {
        return "Meow";
    }
}

public class Dog implements INoise {

    @Override
    public String makeNoise() {
        return "Woof";
    }
}
```

Usage:

```java
INoise n1 = new Cat();
INoise n2 = new Dog();

System.out.println(n1.makeNoise()); // Meow
System.out.println(n2.makeNoise()); // Woof
```

Here, `INoise` gives a **capability**: “this object can make noise”.

---

### 2.2 中文解释

**接口（interface）** 是一种只定义 **“能做什么”**，不关心 **“怎么做”** 的类型。
可以理解为一个 **“规范 / 合同”**：谁实现了这个接口，谁就必须提供这些方法的实现。

重点（Java 8+）：

* 用 `interface` 声明。
* 方法默认是 **public abstract**（公共的、抽象的），也可以有 `default` / `static` 方法。
* 可以包含：

  * 抽象方法
  * `default` 方法（有方法体，可以给统一默认实现）
  * `static` 方法
  * 常量（`public static final`）
* 类通过 `implements` 来 **实现接口**。
* 一个类可以 **实现多个接口**，所以接口经常用来表达 “某某能力”。

上面例子：

* `INoise` 表示 “会发出声音” 的能力。
* `Cat` 和 `Dog` 都 `implements INoise`，所以都要实现 `makeNoise()`。
* 使用时，可以把它们都当作 `INoise` 来用，这是 **多态**。

---

## 3. Abstract class vs Interface – comparison 对比总结

### 3.1 English summary

**Similarities**:

* Both can contain abstract methods.
* Both support polymorphism (you can use an abstract class or interface as a reference type).
* Both are used to design OOP structure.

**Differences (classic view):**

| Aspect               | Abstract Class                               | Interface                                     |
| -------------------- | -------------------------------------------- | --------------------------------------------- |
| Keyword              | `abstract class`                             | `interface`                                   |
| Instantiation        | Cannot be instantiated                       | Cannot be instantiated                        |
| Methods              | Can have abstract & concrete methods         | Mostly abstract methods (plus default/static) |
| Fields / state       | Can have instance fields (state)             | Typically constants only                      |
| Inheritance          | `extends` (single inheritance only)          | `implements`, and a class can implement many  |
| Constructor          | **Yes**, can have constructors               | **No** constructors                           |
| Relationship meaning | “is-a” base class with shared implementation | “has ability / behavior” (capability / role)  |

---

### 3.2 中文总结

**相同点：**

* 都可以包含抽象方法。
* 都不能直接创建对象。
* 都可以作为引用类型，用于多态。

**主要区别（传统理解）：**

| 方面      | 抽象类 (abstract class)           | 接口 (interface)                     |
| ------- | ------------------------------ | ---------------------------------- |
| 关键字     | `abstract class`               | `interface`                        |
| 是否能创建对象 | 不能                             | 不能                                 |
| 方法      | 可以有 抽象方法 + 普通方法                | 一般是抽象方法，也可以有 `default`、`static` 方法 |
| 字段 / 状态 | 可以有实例变量（真正的对象状态）               | 通常是常量（`public static final`）       |
| 继承方式    | 用 `extends`；一个类 **只能继承一个** 抽象类 | 用 `implements`；**可以实现多个接口**        |
| 构造方法    | 有构造方法，可以给子类复用                  | 没有构造方法                             |
| 含义      | 表示一种 “是什么” 的父类，带有公共实现          | 表示一种 “能做什么” 的能力/角色                 |

---

## 4. When to use abstract class vs interface?

### 4.1 English

Use **abstract class** when:

* You have a strong **“is-a”** relationship (Dog *is an* Animal).
* You want to share **common fields and methods** (state + behavior).
* You need constructors, protected fields, etc.

Use **interface** when:

* You want to define a **capability / role** (INoise, Runnable, Comparable).
* Different classes from different hierarchies can share this ability (e.g., `Dog`, `Robot`, and `Car` can all implement `IMove`).
* You need **multiple inheritance of types** (`class A extends B implements C, D...`).

---

### 4.2 中文

适合用 **抽象类** 的情况：

* 存在比较强的 “**是一个**” 关系（Dog 是一种 Animal）。
* 需要在父类中保存 **共享的属性和方法**（状态 + 行为）。
* 需要构造方法、`protected` 字段等。

适合用 **接口** 的情况：

* 表达一种 **能力 / 角色**（比如会叫 `INoise`、可以比较大小 `Comparable`）。
* 不同继承体系的类都可以拥有同一种能力（狗、机器人、汽车都可以“移动”）。
* 需要 **多实现**（一个类实现多个接口）。

---

如果你愿意，我可以拿你实验里的具体代码（比如 `Animal` 抽象类、`IProductionAnimal` 接口、`INoise` 接口等）逐行标注：

* 哪些是抽象类的特点
* 哪些是接口的用法
* 在那里具体体现了“多态” 👍
