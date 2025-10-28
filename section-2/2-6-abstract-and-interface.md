# 2-6. 抽象クラスとインターフェース

抽象クラスとインターフェースは、設計の柔軟性を高めるための重要な機能です。

## 抽象クラス

**抽象クラス**は、不完全なクラスで、直接インスタンス化できません。サブクラスで実装を完成させることを前提としています。

### 抽象クラスの定義

```java
public abstract class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }

    // 抽象メソッド（実装なし）
    public abstract void makeSound();

    // 具象メソッド（実装あり）
    public void sleep() {
        System.out.println(name + " is sleeping.");
    }
}

// サブクラスで実装
public class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }

    @Override
    public void makeSound() {
        System.out.println(name + " says: Woof!");
    }
}

// 使用例
// Animal animal = new Animal("Generic");  // エラー: 抽象クラスはインスタンス化不可
Animal dog = new Dog("Buddy");  // OK
dog.makeSound();  // Buddy says: Woof!
dog.sleep();      // Buddy is sleeping.
```

### 抽象クラスの特徴

1. **`abstract` キーワード**: クラス宣言に必要
2. **抽象メソッド**: 実装を持たないメソッド（サブクラスで実装必須）
3. **具象メソッド**: 実装を持つメソッド（オプション）
4. **コンストラクタ**: 持つことができる
5. **フィールド**: 通常のフィールドを持てる
6. **インスタンス化**: 不可

### 抽象メソッド

```java
public abstract class Shape {
    protected String color;

    // コンストラクタ
    public Shape(String color) {
        this.color = color;
    }

    // 抽象メソッド（実装なし）
    public abstract double getArea();
    public abstract double getPerimeter();

    // 具象メソッド（実装あり）
    public void displayColor() {
        System.out.println("Color: " + color);
    }
}
```

### サブクラスでの実装

```java
public class Circle extends Shape {
    private double radius;

    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }

    @Override
    public double getArea() {
        return Math.PI * radius * radius;
    }

    @Override
    public double getPerimeter() {
        return 2 * Math.PI * radius;
    }
}

public class Rectangle extends Shape {
    private double width;
    private double height;

    public Rectangle(String color, double width, double height) {
        super(color);
        this.width = width;
        this.height = height;
    }

    @Override
    public double getArea() {
        return width * height;
    }

    @Override
    public double getPerimeter() {
        return 2 * (width + height);
    }
}
```

## インターフェース

**インターフェース**は、クラスが実装すべきメソッドの契約（仕様）を定義します。

### インターフェースの定義

```java
public interface Drawable {
    // 抽象メソッド（publicでabstract、省略可能）
    void draw();
    void erase();
}

// 実装
public class Circle implements Drawable {
    @Override
    public void draw() {
        System.out.println("Drawing a circle");
    }

    @Override
    public void erase() {
        System.out.println("Erasing a circle");
    }
}
```

### インターフェースの特徴

1. **すべてのメソッドはpublic abstract**（Java 8以前）
2. **フィールドは`public static final`**（定数のみ）
3. **多重実装が可能**（複数のインターフェースを実装できる）
4. **インスタンス化不可**

### デフォルトメソッド（Java 8以降）

インターフェースにデフォルト実装を持つメソッドを定義できます。

```java
public interface Drawable {
    void draw();

    // デフォルトメソッド
    default void display() {
        System.out.println("Displaying...");
        draw();
    }
}

public class Circle implements Drawable {
    @Override
    public void draw() {
        System.out.println("Drawing a circle");
    }

    // displayはオーバーライド不要（デフォルト実装を使用）
}

// 使用例
Circle circle = new Circle();
circle.display();  // Displaying... Drawing a circle
```

### staticメソッド（Java 8以降）

インターフェースにstaticメソッドを定義できます。

```java
public interface MathOperations {
    int add(int a, int b);

    // staticメソッド
    static int multiply(int a, int b) {
        return a * b;
    }
}

// 使用例
int result = MathOperations.multiply(5, 3);  // 15
```

### privateメソッド（Java 9以降）

インターフェース内でのコード再利用のために使用します。

```java
public interface Logger {
    default void logInfo(String message) {
        log("INFO", message);
    }

    default void logError(String message) {
        log("ERROR", message);
    }

    // privateメソッド（内部でのみ使用）
    private void log(String level, String message) {
        System.out.println("[" + level + "] " + message);
    }
}
```

## 多重実装

Javaは単一継承ですが、複数のインターフェースを実装できます。

```java
public interface Flyable {
    void fly();
}

public interface Swimmable {
    void swim();
}

// 複数のインターフェースを実装
public class Duck implements Flyable, Swimmable {
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }

    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
}

// 使用例
Duck duck = new Duck();
duck.fly();
duck.swim();
```

## インターフェースの継承

インターフェースは他のインターフェースを継承できます（複数可能）。

```java
public interface Animal {
    void eat();
}

public interface Pet extends Animal {
    void play();
}

public interface ServiceAnimal extends Animal {
    void work();
}

// 複数のインターフェースを継承
public interface Companion extends Pet, ServiceAnimal {
    void comfort();
}

// 実装
public class Dog implements Companion {
    @Override
    public void eat() {
        System.out.println("Eating");
    }

    @Override
    public void play() {
        System.out.println("Playing");
    }

    @Override
    public void work() {
        System.out.println("Working");
    }

    @Override
    public void comfort() {
        System.out.println("Comforting");
    }
}
```

## 抽象クラス vs インターフェース

| | 抽象クラス | インターフェース |
|---|-----------|----------------|
| **目的** | 共通の実装を提供 | 契約（仕様）を定義 |
| **継承/実装** | 単一継承 | 多重実装 |
| **メソッド** | 抽象・具象の両方 | 抽象・デフォルト・static |
| **フィールド** | 通常のフィールド | public static final のみ |
| **コンストラクタ** | あり | なし |
| **アクセス修飾子** | 自由 | publicのみ（メソッド） |

### 使い分けの指針

**抽象クラスを使う場合:**
- "is-a" 関係（共通の性質を持つ）
- 共通の実装を提供したい
- フィールドやコンストラクタが必要

**インターフェースを使う場合:**
- "can-do" 関係（能力や機能を表す）
- 多重実装が必要
- 異なる階層のクラスに共通機能を持たせたい

```java
// 抽象クラス: 共通の性質
public abstract class Vehicle {
    protected String brand;

    public Vehicle(String brand) {
        this.brand = brand;
    }

    public abstract void move();
}

// インターフェース: 能力
public interface Electric {
    void charge();
}

// 組み合わせて使用
public class ElectricCar extends Vehicle implements Electric {
    public ElectricCar(String brand) {
        super(brand);
    }

    @Override
    public void move() {
        System.out.println("Electric car is moving");
    }

    @Override
    public void charge() {
        System.out.println("Charging battery");
    }
}
```

## 実践例: データストレージシステム

```java
// インターフェース: 基本的な操作
public interface DataStorage {
    void save(String data);
    String load();
    void delete();
}

// インターフェース: 暗号化機能
public interface Encryptable {
    String encrypt(String data);
    String decrypt(String data);
}

// ファイルストレージ
public class FileStorage implements DataStorage {
    private String filename;

    public FileStorage(String filename) {
        this.filename = filename;
    }

    @Override
    public void save(String data) {
        System.out.println("Saving to file: " + filename);
        // ファイル書き込み処理
    }

    @Override
    public String load() {
        System.out.println("Loading from file: " + filename);
        return "data from file";
    }

    @Override
    public void delete() {
        System.out.println("Deleting file: " + filename);
    }
}

// データベースストレージ（暗号化機能付き）
public class DatabaseStorage implements DataStorage, Encryptable {
    private String tableName;

    public DatabaseStorage(String tableName) {
        this.tableName = tableName;
    }

    @Override
    public void save(String data) {
        String encrypted = encrypt(data);
        System.out.println("Saving to database: " + tableName);
        // DB保存処理
    }

    @Override
    public String load() {
        System.out.println("Loading from database: " + tableName);
        String data = "encrypted data from db";
        return decrypt(data);
    }

    @Override
    public void delete() {
        System.out.println("Deleting from database: " + tableName);
    }

    @Override
    public String encrypt(String data) {
        return "encrypted_" + data;
    }

    @Override
    public String decrypt(String data) {
        return data.replace("encrypted_", "");
    }
}

// 使用例
public class Main {
    public static void main(String[] args) {
        // 異なる実装を同じインターフェースで扱う
        DataStorage storage1 = new FileStorage("data.txt");
        DataStorage storage2 = new DatabaseStorage("users");

        storage1.save("Hello");
        storage2.save("World");

        System.out.println(storage1.load());
        System.out.println(storage2.load());
    }
}
```

## 実践例: プラグインシステム

```java
// プラグインのインターフェース
public interface Plugin {
    String getName();
    String getVersion();
    void initialize();
    void execute();
    void shutdown();
}

// 抽象クラス: 共通機能を提供
public abstract class AbstractPlugin implements Plugin {
    protected String name;
    protected String version;
    protected boolean initialized = false;

    public AbstractPlugin(String name, String version) {
        this.name = name;
        this.version = version;
    }

    @Override
    public String getName() {
        return name;
    }

    @Override
    public String getVersion() {
        return version;
    }

    @Override
    public void initialize() {
        System.out.println("Initializing " + name + " v" + version);
        initialized = true;
    }

    @Override
    public void shutdown() {
        System.out.println("Shutting down " + name);
        initialized = false;
    }

    // サブクラスで実装
    @Override
    public abstract void execute();
}

// 具体的なプラグイン
public class LoggerPlugin extends AbstractPlugin {
    public LoggerPlugin() {
        super("Logger", "1.0");
    }

    @Override
    public void execute() {
        if (initialized) {
            System.out.println("[" + getName() + "] Logging data...");
        }
    }
}

public class BackupPlugin extends AbstractPlugin {
    public BackupPlugin() {
        super("Backup", "2.0");
    }

    @Override
    public void execute() {
        if (initialized) {
            System.out.println("[" + getName() + "] Creating backup...");
        }
    }
}

// プラグインマネージャー
public class PluginManager {
    private List<Plugin> plugins = new ArrayList<>();

    public void registerPlugin(Plugin plugin) {
        plugins.add(plugin);
        plugin.initialize();
    }

    public void executeAll() {
        for (Plugin plugin : plugins) {
            plugin.execute();
        }
    }

    public void shutdownAll() {
        for (Plugin plugin : plugins) {
            plugin.shutdown();
        }
    }
}

// 使用例
public class Main {
    public static void main(String[] args) {
        PluginManager manager = new PluginManager();

        manager.registerPlugin(new LoggerPlugin());
        manager.registerPlugin(new BackupPlugin());

        manager.executeAll();
        manager.shutdownAll();
    }
}
```

## まとめ

### 抽象クラス
- **目的**: 共通の実装を提供
- **特徴**: 抽象メソッドと具象メソッドの両方を持つ
- **単一継承**: 1つのクラスのみ継承可能

### インターフェース
- **目的**: 契約（仕様）を定義
- **特徴**: 抽象メソッド、デフォルトメソッド、staticメソッド
- **多重実装**: 複数のインターフェースを実装可能

### 使い分け
- **"is-a" 関係**: 抽象クラス
- **"can-do" 関係**: インターフェース
- **多重継承が必要**: インターフェース
- **共通実装を共有**: 抽象クラス

次のセクションからは、Javaの重要機能について学びます。
