# 2-2. メソッドとコンストラクタ

メソッドとコンストラクタは、クラスの動作を定義し、オブジェクトを初期化するための重要な要素です。

## メソッドの詳細

### メソッドの構造

```java
アクセス修飾子 戻り値の型 メソッド名(パラメータリスト) {
    // メソッド本体
    return 戻り値;  // 戻り値の型がvoid以外の場合
}
```

### 例

```java
public class Calculator {
    // 戻り値あり、パラメータあり
    public int add(int a, int b) {
        return a + b;
    }

    // 戻り値なし（void）
    public void printSum(int a, int b) {
        System.out.println("Sum: " + (a + b));
    }

    // パラメータなし
    public String getWelcomeMessage() {
        return "Welcome to Calculator!";
    }
}
```

## パラメータと引数

### 値渡し（Pass by Value）

Javaはすべて**値渡し**です。

**プリミティブ型の場合:**

```java
public class Example {
    public static void main(String[] args) {
        int x = 10;
        modify(x);
        System.out.println(x);  // 10（変更されない）
    }

    static void modify(int num) {
        num = 20;  // ローカル変数のみ変更
    }
}
```

**参照型の場合:**

```java
public class Example {
    public static void main(String[] args) {
        Person person = new Person();
        person.name = "Alice";

        modifyPerson(person);
        System.out.println(person.name);  // "Bob"（オブジェクトの内容は変更される）

        reassignPerson(person);
        System.out.println(person.name);  // "Bob"（参照自体は変更されない）
    }

    static void modifyPerson(Person p) {
        p.name = "Bob";  // オブジェクトの内容を変更
    }

    static void reassignPerson(Person p) {
        p = new Person();  // ローカル変数のみ変更
        p.name = "Charlie";
    }
}
```

**ポイント**: 参照のコピーが渡されるため、オブジェクトの内容は変更できますが、参照自体は変更できません。

### 可変長引数（Varargs）

メソッドに任意の個数の引数を渡せます。

```java
public class MathUtil {
    // 可変長引数
    public static int sum(int... numbers) {
        int total = 0;
        for (int num : numbers) {
            total += num;
        }
        return total;
    }
}

// 使用例
int result1 = MathUtil.sum(1, 2, 3);  // 6
int result2 = MathUtil.sum(1, 2, 3, 4, 5);  // 15
int result3 = MathUtil.sum();  // 0
```

**制約:**
- 可変長引数はパラメータの最後に1つだけ指定可能

```java
// OK
public void method(String name, int... numbers) { }

// エラー
public void method(int... numbers, String name) { }
public void method(int... numbers1, int... numbers2) { }
```

## メソッドのオーバーロード

同じ名前で異なるパラメータを持つメソッドを複数定義できます。

```java
public class Printer {
    // パラメータの型が異なる
    public void print(int value) {
        System.out.println("Integer: " + value);
    }

    public void print(double value) {
        System.out.println("Double: " + value);
    }

    public void print(String value) {
        System.out.println("String: " + value);
    }

    // パラメータの数が異なる
    public void print(String message, int times) {
        for (int i = 0; i < times; i++) {
            System.out.println(message);
        }
    }
}

// 使用例
Printer printer = new Printer();
printer.print(10);           // Integer: 10
printer.print(3.14);         // Double: 3.14
printer.print("Hello");      // String: Hello
printer.print("Hi", 3);      // Hi（3回）
```

**オーバーロードの条件:**
- パラメータの**数**、**型**、**順序**が異なる必要がある
- 戻り値の型だけが異なるのはNG

```java
// エラー: 戻り値の型だけ異なる
public int getValue() { return 1; }
public double getValue() { return 1.0; }  // コンパイルエラー
```

## コンストラクタ

コンストラクタは、オブジェクトの初期化を行う特別なメソッドです。

### コンストラクタの特徴

- クラス名と同じ名前
- 戻り値の型を指定しない（`void` も不要）
- `new` キーワードでオブジェクト生成時に自動的に呼ばれる

### デフォルトコンストラクタ

コンストラクタを定義しない場合、コンパイラが自動的にデフォルトコンストラクタを生成します。

```java
public class Person {
    String name;
    int age;
}

// デフォルトコンストラクタが自動生成される
Person person = new Person();
```

### カスタムコンストラクタ

```java
public class Person {
    String name;
    int age;

    // コンストラクタ
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}

// 使用例
Person person = new Person("Alice", 25);
System.out.println(person.name);  // Alice
```

**注意**: カスタムコンストラクタを定義すると、デフォルトコンストラクタは自動生成されません。

```java
public class Person {
    String name;

    public Person(String name) {
        this.name = name;
    }
}

// Person person = new Person();  // エラー: 引数なしのコンストラクタが存在しない
Person person = new Person("Alice");  // OK
```

### コンストラクタのオーバーロード

複数のコンストラクタを定義できます。

```java
public class Person {
    String name;
    int age;
    String email;

    // コンストラクタ1
    public Person() {
        this.name = "Unknown";
        this.age = 0;
    }

    // コンストラクタ2
    public Person(String name) {
        this.name = name;
        this.age = 0;
    }

    // コンストラクタ3
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // コンストラクタ4
    public Person(String name, int age, String email) {
        this.name = name;
        this.age = age;
        this.email = email;
    }
}

// 使用例
Person p1 = new Person();
Person p2 = new Person("Alice");
Person p3 = new Person("Bob", 30);
Person p4 = new Person("Charlie", 25, "charlie@example.com");
```

### コンストラクタチェーン（this()）

他のコンストラクタを呼び出すことで、重複を避けられます。

```java
public class Person {
    String name;
    int age;
    String email;

    // コンストラクタ1
    public Person() {
        this("Unknown", 0, null);  // コンストラクタ3を呼び出す
    }

    // コンストラクタ2
    public Person(String name) {
        this(name, 0, null);  // コンストラクタ3を呼び出す
    }

    // コンストラクタ3（メイン）
    public Person(String name, int age, String email) {
        this.name = name;
        this.age = age;
        this.email = email;
    }
}
```

**注意**: `this()` はコンストラクタの最初の行でなければなりません。

```java
public Person(String name) {
    System.out.println("Creating person...");
    this(name, 0, null);  // エラー: this()は最初の行でなければならない
}
```

## 初期化ブロック

### インスタンス初期化ブロック

```java
public class Person {
    String name;
    int age;

    // インスタンス初期化ブロック（コンストラクタより前に実行）
    {
        System.out.println("Initializing...");
        age = 0;
    }

    public Person(String name) {
        this.name = name;
    }
}
```

### static初期化ブロック

クラスがロードされる際に1度だけ実行されます。

```java
public class Config {
    static String dbUrl;
    static int maxConnections;

    // static初期化ブロック
    static {
        System.out.println("Loading configuration...");
        dbUrl = "jdbc:mysql://localhost:3306/mydb";
        maxConnections = 10;
    }
}
```

### 初期化の順序

```java
public class Demo {
    static int staticVar = initStaticVar();
    int instanceVar = initInstanceVar();

    static {
        System.out.println("3. Static block");
    }

    {
        System.out.println("5. Instance block");
    }

    public Demo() {
        System.out.println("6. Constructor");
    }

    static int initStaticVar() {
        System.out.println("2. Static variable initialization");
        return 1;
    }

    int initInstanceVar() {
        System.out.println("4. Instance variable initialization");
        return 1;
    }

    public static void main(String[] args) {
        System.out.println("1. Main method");
        new Demo();
    }
}

// 出力:
// 1. Main method
// 2. Static variable initialization
// 3. Static block
// 4. Instance variable initialization
// 5. Instance block
// 6. Constructor
```

## 実践例: Rectangleクラス

```java
public class Rectangle {
    private double width;
    private double height;

    // コンストラクタ1: デフォルト（正方形）
    public Rectangle() {
        this(1.0, 1.0);
    }

    // コンストラクタ2: 正方形
    public Rectangle(double side) {
        this(side, side);
    }

    // コンストラクタ3: 長方形
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    // メソッド
    public double getArea() {
        return width * height;
    }

    public double getPerimeter() {
        return 2 * (width + height);
    }

    public void scale(double factor) {
        width *= factor;
        height *= factor;
    }

    public void displayInfo() {
        System.out.println("Width: " + width + ", Height: " + height);
        System.out.println("Area: " + getArea());
        System.out.println("Perimeter: " + getPerimeter());
    }
}

// 使用例
public class Main {
    public static void main(String[] args) {
        Rectangle r1 = new Rectangle();  // 1x1の正方形
        Rectangle r2 = new Rectangle(5);  // 5x5の正方形
        Rectangle r3 = new Rectangle(4, 6);  // 4x6の長方形

        r3.displayInfo();
        r3.scale(2);  // 2倍に拡大
        r3.displayInfo();
    }
}
```

## まとめ

### メソッド
- **構造**: アクセス修飾子、戻り値の型、メソッド名、パラメータ
- **値渡し**: すべて値渡し（参照型は参照のコピーが渡される）
- **可変長引数**: `int... numbers` で任意個数の引数
- **オーバーロード**: 同じ名前で異なるパラメータ

### コンストラクタ
- **目的**: オブジェクトの初期化
- **特徴**: クラス名と同じ、戻り値なし
- **デフォルト**: 定義しない場合は自動生成
- **オーバーロード**: 複数のコンストラクタを定義可能
- **チェーン**: `this()` で他のコンストラクタを呼び出し

次のセクションでは、カプセル化について学びます。
