# 2-4. 継承

継承（Inheritance）は、既存のクラスの機能を再利用し、拡張する仕組みです。

## 継承とは

**継承**は、既存のクラス（スーパークラス/親クラス）の機能を引き継いで、新しいクラス（サブクラス/子クラス）を作成する機能です。

### メリット

1. **コードの再利用**: 共通機能を1箇所にまとめる
2. **保守性の向上**: 変更箇所を減らせる
3. **拡張性**: 既存コードを変更せずに機能追加

### 構文

```java
public class SubClass extends SuperClass {
    // サブクラスの内容
}
```

## 基本的な継承

### 例: AnimalとDog

```java
// スーパークラス
public class Animal {
    protected String name;
    protected int age;

    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void eat() {
        System.out.println(name + " is eating.");
    }

    public void sleep() {
        System.out.println(name + " is sleeping.");
    }
}

// サブクラス
public class Dog extends Animal {
    private String breed;

    public Dog(String name, int age, String breed) {
        super(name, age);  // スーパークラスのコンストラクタを呼び出す
        this.breed = breed;
    }

    // Dogクラス独自のメソッド
    public void bark() {
        System.out.println(name + " is barking: Woof! Woof!");
    }

    public String getBreed() {
        return breed;
    }
}

// 使用例
public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog("Buddy", 3, "Golden Retriever");

        // スーパークラスから継承したメソッド
        dog.eat();    // Buddy is eating.
        dog.sleep();  // Buddy is sleeping.

        // サブクラス独自のメソッド
        dog.bark();   // Buddy is barking: Woof! Woof!
    }
}
```

## super キーワード

`super` は、スーパークラスのメンバーにアクセスするために使用します。

### スーパークラスのコンストラクタ呼び出し

```java
public class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }
}

public class Dog extends Animal {
    private String breed;

    public Dog(String name, String breed) {
        super(name);  // スーパークラスのコンストラクタを呼び出す
        this.breed = breed;
    }
}
```

**注意**: `super()` はコンストラクタの最初の行でなければなりません。

### スーパークラスのメソッド呼び出し

```java
public class Animal {
    public void makeSound() {
        System.out.println("Some sound");
    }
}

public class Dog extends Animal {
    @Override
    public void makeSound() {
        super.makeSound();  // スーパークラスのメソッドを呼び出す
        System.out.println("Woof!");
    }
}

// 使用例
Dog dog = new Dog();
dog.makeSound();
// 出力:
// Some sound
// Woof!
```

## protected修飾子

`protected` は、同じパッケージまたはサブクラスからアクセス可能です。

```java
public class Animal {
    protected String name;  // サブクラスからアクセス可能

    private int age;  // サブクラスからもアクセス不可

    public int getAge() {
        return age;
    }
}

public class Dog extends Animal {
    public void displayInfo() {
        System.out.println("Name: " + name);  // OK（protected）
        // System.out.println("Age: " + age);  // エラー（private）
        System.out.println("Age: " + getAge());  // OK（publicメソッド経由）
    }
}
```

## メソッドのオーバーライド

サブクラスでスーパークラスのメソッドを再定義することを**オーバーライド**と言います。

### 基本形

```java
public class Animal {
    public void makeSound() {
        System.out.println("Some generic sound");
    }
}

public class Dog extends Animal {
    @Override  // アノテーション（推奨）
    public void makeSound() {
        System.out.println("Woof! Woof!");
    }
}

public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Meow!");
    }
}

// 使用例
Animal animal1 = new Dog();
Animal animal2 = new Cat();

animal1.makeSound();  // Woof! Woof!
animal2.makeSound();  // Meow!
```

### @Override アノテーション

`@Override` アノテーションは、メソッドが正しくオーバーライドされているかをコンパイラがチェックします。

```java
public class Animal {
    public void makeSound() {
        System.out.println("Some sound");
    }
}

public class Dog extends Animal {
    @Override
    public void makeSond() {  // タイポ
        System.out.println("Woof!");
    }
    // コンパイルエラー: makeSondというメソッドはスーパークラスに存在しない
}
```

### オーバーライドの制約

1. **メソッドシグネチャ**: 名前、パラメータの型と数が同じでなければならない
2. **アクセス修飾子**: 制限を強くできない（public → protected は不可）
3. **戻り値の型**: 同じか、サブタイプでなければならない（共変戻り値型）
4. **例外**: チェック例外をスーパークラスより多く投げられない

```java
public class Animal {
    public void move() {
        System.out.println("Moving");
    }
}

public class Dog extends Animal {
    // エラー: アクセス修飾子を制限できない
    // protected void move() { }

    // OK: 制限を緩めるのは可能
    @Override
    public void move() {
        System.out.println("Running");
    }
}
```

## final キーワード

### finalクラス

継承を禁止します。

```java
public final class String {
    // Stringクラスは継承不可
}

// public class MyString extends String { }  // エラー
```

### finalメソッド

オーバーライドを禁止します。

```java
public class Animal {
    public final void breathe() {
        System.out.println("Breathing");
    }
}

public class Dog extends Animal {
    // エラー: finalメソッドはオーバーライドできない
    // @Override
    // public void breathe() { }
}
```

### final変数

値の変更を禁止します。

```java
public class Constants {
    public static final double PI = 3.14159;  // 定数
}
```

## 継承の階層

複数レベルの継承が可能です。

```java
public class Animal {
    public void eat() {
        System.out.println("Eating");
    }
}

public class Mammal extends Animal {
    public void breathe() {
        System.out.println("Breathing");
    }
}

public class Dog extends Mammal {
    public void bark() {
        System.out.println("Barking");
    }
}

// 使用例
Dog dog = new Dog();
dog.eat();     // Animalから継承
dog.breathe(); // Mammalから継承
dog.bark();    // Dog独自
```

## Objectクラス

すべてのクラスは、明示的に継承を指定しなくても `Object` クラスを継承します。

```java
public class MyClass {
    // 暗黙的に extends Object
}

// 以下と同じ
public class MyClass extends Object {
}
```

### Objectクラスの主なメソッド

| メソッド | 説明 |
|----------|------|
| `toString()` | オブジェクトの文字列表現 |
| `equals(Object obj)` | オブジェクトの等価性判定 |
| `hashCode()` | ハッシュコード |
| `getClass()` | クラス情報の取得 |

### toStringのオーバーライド

```java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }
}

// 使用例
Person person = new Person("Alice", 25);
System.out.println(person);  // Person{name='Alice', age=25}
```

### equalsとhashCodeのオーバーライド

```java
import java.util.Objects;

public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Person person = (Person) obj;
        return age == person.age && Objects.equals(name, person.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}

// 使用例
Person p1 = new Person("Alice", 25);
Person p2 = new Person("Alice", 25);

System.out.println(p1.equals(p2));  // true（内容が同じ）
```

## 継承 vs コンポジション

### 継承の問題点

- **強い結合**: スーパークラスの変更がサブクラスに影響
- **単一継承**: Javaは多重継承不可
- **不適切な継承**: "is-a" 関係でない場合に問題

### コンポジション（委譲）

オブジェクトを内部に持つことで機能を利用します。

```java
// 継承（is-a関係）
public class Car extends Engine {  // Carはエンジンではない（不適切）
}

// コンポジション（has-a関係）
public class Car {
    private Engine engine;  // CarはEngineを持つ（適切）

    public Car() {
        this.engine = new Engine();
    }

    public void start() {
        engine.start();
    }
}
```

**原則**: "is-a" 関係の場合は継承、"has-a" 関係の場合はコンポジション

## 実践例: 図形の階層

```java
// スーパークラス
public class Shape {
    protected String color;

    public Shape(String color) {
        this.color = color;
    }

    public void displayColor() {
        System.out.println("Color: " + color);
    }

    // サブクラスでオーバーライド
    public double getArea() {
        return 0;
    }
}

// サブクラス1
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
}

// サブクラス2
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
}

// 使用例
public class Main {
    public static void main(String[] args) {
        Shape circle = new Circle("Red", 5);
        Shape rectangle = new Rectangle("Blue", 4, 6);

        circle.displayColor();
        System.out.println("Area: " + circle.getArea());

        rectangle.displayColor();
        System.out.println("Area: " + rectangle.getArea());
    }
}
```

## まとめ

- **継承**: `extends` でスーパークラスの機能を引き継ぐ
- **super**: スーパークラスのメンバーにアクセス
- **protected**: サブクラスからアクセス可能
- **オーバーライド**: サブクラスでメソッドを再定義（`@Override`）
- **final**: 継承やオーバーライドを禁止
- **Object**: すべてのクラスの親
- **継承 vs コンポジション**: "is-a" なら継承、"has-a" ならコンポジション

次のセクションでは、ポリモーフィズムについて学びます。
