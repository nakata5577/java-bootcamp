# 2-1. クラスとオブジェクト

オブジェクト指向プログラミング（OOP）の基本概念であるクラスとオブジェクトについて学びます。

## クラスとは

**クラス**は、オブジェクトの設計図（テンプレート）です。データ（フィールド）と動作（メソッド）をまとめて定義します。

**オブジェクト**は、クラスから生成された実体（インスタンス）です。

**例え:**
- クラス = 「車の設計図」
- オブジェクト = 「実際の車」（トヨタのカローラ、ホンダのシビックなど）

## クラスの定義

### 基本構文

```java
public class Car {
    // フィールド（属性、状態）
    String brand;
    String model;
    int year;

    // メソッド（動作、振る舞い）
    void start() {
        System.out.println(brand + " " + model + " is starting...");
    }

    void stop() {
        System.out.println(brand + " " + model + " is stopping...");
    }
}
```

### フィールド（インスタンス変数）

クラスの状態を表すデータです。

```java
public class Person {
    // インスタンス変数
    String name;
    int age;
    String email;
}
```

### メソッド

クラスの動作を定義します。

```java
public class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    int subtract(int a, int b) {
        return a - b;
    }
}
```

## オブジェクトの生成と使用

### オブジェクトの生成

`new` キーワードを使ってオブジェクトを生成します。

```java
public class Main {
    public static void main(String[] args) {
        // オブジェクトの生成
        Car myCar = new Car();

        // フィールドへのアクセス
        myCar.brand = "Toyota";
        myCar.model = "Corolla";
        myCar.year = 2023;

        // メソッドの呼び出し
        myCar.start();  // Toyota Corolla is starting...
        myCar.stop();   // Toyota Corolla is stopping...
    }
}
```

### 複数のオブジェクト

同じクラスから複数のオブジェクトを生成できます。

```java
Car car1 = new Car();
car1.brand = "Toyota";
car1.model = "Corolla";

Car car2 = new Car();
car2.brand = "Honda";
car2.model = "Civic";

// car1とcar2は独立したオブジェクト
```

## オブジェクトとメモリ

### 参照とオブジェクト

```java
Car myCar = new Car();
```

- `myCar` は**参照変数**（メモリ上のオブジェクトの場所を指す）
- `new Car()` は**オブジェクト**（ヒープメモリに確保される）

### 参照のコピー

```java
Car car1 = new Car();
car1.brand = "Toyota";

Car car2 = car1;  // 参照をコピー（オブジェクトは1つ）

car2.brand = "Honda";
System.out.println(car1.brand);  // "Honda"（同じオブジェクトを参照）
```

### null

オブジェクトが存在しないことを表します。

```java
Car car = null;  // オブジェクトを参照していない

// car.start();  // NullPointerException

if (car != null) {
    car.start();  // 安全
}
```

## クラスの設計例

### Personクラス

```java
public class Person {
    // フィールド
    String name;
    int age;
    String email;

    // メソッド
    void introduce() {
        System.out.println("私の名前は" + name + "、" + age + "歳です。");
    }

    void sendEmail(String message) {
        System.out.println("Sending email to " + email + ": " + message);
    }

    boolean isAdult() {
        return age >= 18;
    }
}
```

### 使用例

```java
public class Main {
    public static void main(String[] args) {
        Person person = new Person();
        person.name = "Alice";
        person.age = 25;
        person.email = "alice@example.com";

        person.introduce();  // 私の名前はAlice、25歳です。

        if (person.isAdult()) {
            System.out.println(person.name + " is an adult.");
        }

        person.sendEmail("Hello!");
    }
}
```

## thisキーワード

`this` は、現在のオブジェクト自身を参照します。

### フィールドとパラメータの区別

```java
public class Person {
    String name;
    int age;

    void setData(String name, int age) {
        // this.name はフィールド、name はパラメータ
        this.name = name;
        this.age = age;
    }
}
```

### メソッドチェーン

`this` を返すことで、メソッドチェーンが可能になります。

```java
public class Person {
    String name;
    int age;

    Person setName(String name) {
        this.name = name;
        return this;
    }

    Person setAge(int age) {
        this.age = age;
        return this;
    }

    void introduce() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

// 使用例
Person person = new Person();
person.setName("Alice").setAge(25).introduce();
```

## クラス変数とクラスメソッド（static）

### インスタンス変数 vs クラス変数

```java
public class Counter {
    // インスタンス変数（各オブジェクトごと）
    int instanceCount = 0;

    // クラス変数（すべてのオブジェクトで共有）
    static int totalCount = 0;

    void increment() {
        instanceCount++;
        totalCount++;
    }
}

// 使用例
Counter c1 = new Counter();
c1.increment();
System.out.println(c1.instanceCount);  // 1
System.out.println(Counter.totalCount); // 1

Counter c2 = new Counter();
c2.increment();
System.out.println(c2.instanceCount);  // 1
System.out.println(Counter.totalCount); // 2（共有されている）
```

### staticメソッド

オブジェクトを生成せずに呼び出せるメソッドです。

```java
public class MathUtil {
    // staticメソッド
    public static int add(int a, int b) {
        return a + b;
    }

    public static int multiply(int a, int b) {
        return a * b;
    }
}

// 使用例
int result = MathUtil.add(5, 3);  // オブジェクト生成不要
System.out.println(result);  // 8
```

**制約**: staticメソッド内では、インスタンス変数やインスタンスメソッドにアクセスできません。

```java
public class Example {
    int instanceVar = 10;
    static int staticVar = 20;

    static void staticMethod() {
        // System.out.println(instanceVar);  // エラー
        System.out.println(staticVar);  // OK
    }

    void instanceMethod() {
        System.out.println(instanceVar);  // OK
        System.out.println(staticVar);    // OK
    }
}
```

### 定数の定義

`static final` を組み合わせて定数を定義します。

```java
public class Constants {
    public static final double PI = 3.14159;
    public static final int MAX_USERS = 100;
    public static final String APP_NAME = "MyApp";
}

// 使用例
double area = Constants.PI * radius * radius;
```

## オブジェクトの比較

### == vs equals()

```java
Person p1 = new Person();
p1.name = "Alice";

Person p2 = new Person();
p2.name = "Alice";

Person p3 = p1;

// == は参照の比較
System.out.println(p1 == p2);  // false（異なるオブジェクト）
System.out.println(p1 == p3);  // true（同じオブジェクトを参照）

// equals() はデフォルトで == と同じ（Objectクラスの実装）
System.out.println(p1.equals(p2));  // false

// 内容を比較するには、equals()をオーバーライド（後述）
```

## 実践例: Bookクラス

```java
public class Book {
    // フィールド
    String title;
    String author;
    int pages;
    double price;

    // メソッド
    void displayInfo() {
        System.out.println("Title: " + title);
        System.out.println("Author: " + author);
        System.out.println("Pages: " + pages);
        System.out.println("Price: $" + price);
    }

    boolean isExpensive() {
        return price > 30.0;
    }

    void applyDiscount(double percentage) {
        price = price * (1 - percentage / 100);
    }
}

// 使用例
public class Main {
    public static void main(String[] args) {
        Book book = new Book();
        book.title = "Effective Java";
        book.author = "Joshua Bloch";
        book.pages = 416;
        book.price = 45.0;

        book.displayInfo();

        if (book.isExpensive()) {
            book.applyDiscount(10);  // 10%割引
            System.out.println("New price: $" + book.price);
        }
    }
}
```

## まとめ

- **クラス**: オブジェクトの設計図（フィールドとメソッドを定義）
- **オブジェクト**: クラスから生成された実体（`new` キーワードで生成）
- **フィールド**: オブジェクトの状態（データ）
- **メソッド**: オブジェクトの動作（処理）
- **this**: 現在のオブジェクト自身を参照
- **static**: クラスレベルの変数やメソッド（すべてのインスタンスで共有）
- **参照**: オブジェクトのメモリ上の位置を指す

次のセクションでは、メソッドとコンストラクタについて詳しく学びます。
