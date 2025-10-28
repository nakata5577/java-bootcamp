# 2-5. ポリモーフィズム（多態性）

ポリモーフィズム（Polymorphism）は、オブジェクト指向の重要な概念で、同じインターフェースで異なる実装を扱う仕組みです。

## ポリモーフィズムとは

**ポリモーフィズム**（多態性）は、「同じ操作でも、対象によって異なる動作をする」という性質です。

### 2種類のポリモーフィズム

1. **コンパイル時ポリモーフィズム**: メソッドのオーバーロード
2. **実行時ポリモーフィズム**: メソッドのオーバーライド

## 実行時ポリモーフィズム

### 基本概念

スーパークラスの参照でサブクラスのオブジェクトを扱います。

```java
public class Animal {
    public void makeSound() {
        System.out.println("Some sound");
    }
}

public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Woof!");
    }
}

public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Meow!");
    }
}

// ポリモーフィズムの活用
public class Main {
    public static void main(String[] args) {
        // スーパークラスの参照でサブクラスのオブジェクトを保持
        Animal animal1 = new Dog();
        Animal animal2 = new Cat();

        animal1.makeSound();  // Woof!
        animal2.makeSound();  // Meow!
    }
}
```

### 動的束縛（Dynamic Binding）

実行時に、実際のオブジェクトの型に基づいてメソッドが呼ばれます。

```java
Animal animal = new Dog();
animal.makeSound();  // 実行時にDogのmakeSoundが呼ばれる
```

## ポリモーフィズムの利点

### 1. 柔軟なコード

```java
public class AnimalShelter {
    // どのAnimalサブクラスでも受け入れられる
    public void feedAnimal(Animal animal) {
        System.out.println("Feeding...");
        animal.makeSound();
    }
}

// 使用例
AnimalShelter shelter = new AnimalShelter();
shelter.feedAnimal(new Dog());  // Woof!
shelter.feedAnimal(new Cat());  // Meow!
shelter.feedAnimal(new Bird()); // Tweet!
```

### 2. 配列やコレクションでの統一的な処理

```java
public class Main {
    public static void main(String[] args) {
        // 異なる型のオブジェクトを同じ配列に格納
        Animal[] animals = {
            new Dog(),
            new Cat(),
            new Bird()
        };

        // 統一的に処理
        for (Animal animal : animals) {
            animal.makeSound();
        }
        // 出力:
        // Woof!
        // Meow!
        // Tweet!
    }
}
```

### 3. 拡張性

新しいサブクラスを追加しても、既存のコードを変更する必要がありません。

```java
// 新しいサブクラスを追加
public class Cow extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Moo!");
    }
}

// 既存のコードはそのまま使える
AnimalShelter shelter = new AnimalShelter();
shelter.feedAnimal(new Cow());  // Moo!
```

## キャストとinstanceof

### アップキャスト（Upcasting）

サブクラスからスーパークラスへの変換（自動的に行われる）。

```java
Dog dog = new Dog();
Animal animal = dog;  // アップキャスト（自動）
```

### ダウンキャスト（Downcasting）

スーパークラスからサブクラスへの変換（明示的なキャストが必要）。

```java
Animal animal = new Dog();

// ダウンキャスト（明示的）
Dog dog = (Dog) animal;
dog.bark();  // Dogのメソッドを呼び出せる
```

### instanceof演算子

安全なダウンキャストのために型をチェックします。

```java
public class Main {
    public static void main(String[] args) {
        Animal animal = new Dog();

        if (animal instanceof Dog) {
            Dog dog = (Dog) animal;
            dog.bark();
        }

        if (animal instanceof Cat) {  // false
            Cat cat = (Cat) animal;
            cat.meow();
        }
    }
}
```

### パターンマッチング（Java 16以降）

より簡潔に型チェックとキャストを行えます。

```java
Animal animal = new Dog();

if (animal instanceof Dog dog) {  // dogに自動的にキャスト
    dog.bark();
}
```

### ClassCastException

不正なキャストは実行時エラーになります。

```java
Animal animal = new Dog();
Cat cat = (Cat) animal;  // ClassCastException（実行時エラー）
```

## オーバーロード vs オーバーライド

### オーバーロード（コンパイル時ポリモーフィズム）

同じ名前で異なるパラメータを持つメソッドを定義します。

```java
public class Calculator {
    // パラメータの型が異なる
    public int add(int a, int b) {
        return a + b;
    }

    public double add(double a, double b) {
        return a + b;
    }

    // パラメータの数が異なる
    public int add(int a, int b, int c) {
        return a + b + c;
    }
}

// コンパイル時に、パラメータの型に基づいて決定される
Calculator calc = new Calculator();
calc.add(1, 2);        // int版が呼ばれる
calc.add(1.0, 2.0);    // double版が呼ばれる
calc.add(1, 2, 3);     // 3引数版が呼ばれる
```

### オーバーライド（実行時ポリモーフィズム）

サブクラスでスーパークラスのメソッドを再定義します。

```java
public class Animal {
    public void makeSound() {
        System.out.println("Some sound");
    }
}

public class Dog extends Animal {
    @Override
    public void makeSound() {  // オーバーライド
        System.out.println("Woof!");
    }
}

// 実行時に、実際のオブジェクトの型に基づいて決定される
Animal animal = new Dog();
animal.makeSound();  // Woof!（実行時に決定）
```

### 違いのまとめ

| | オーバーロード | オーバーライド |
|---|---------------|---------------|
| **目的** | 同じ名前で異なる処理 | スーパークラスのメソッドを再定義 |
| **パラメータ** | 異なる必要がある | 同じでなければならない |
| **クラス** | 同じクラス内 | 異なるクラス（継承関係） |
| **決定時期** | コンパイル時 | 実行時 |
| **キーワード** | なし | `@Override` |

## 実践例: 支払いシステム

```java
// 抽象的な支払い方法
public abstract class PaymentMethod {
    public abstract void pay(double amount);

    public void showReceipt(double amount) {
        System.out.println("Payment of $" + amount + " successful.");
    }
}

// クレジットカード
public class CreditCard extends PaymentMethod {
    private String cardNumber;

    public CreditCard(String cardNumber) {
        this.cardNumber = cardNumber;
    }

    @Override
    public void pay(double amount) {
        System.out.println("Paying $" + amount + " with Credit Card ending in " +
                          cardNumber.substring(cardNumber.length() - 4));
    }
}

// PayPal
public class PayPal extends PaymentMethod {
    private String email;

    public PayPal(String email) {
        this.email = email;
    }

    @Override
    public void pay(double amount) {
        System.out.println("Paying $" + amount + " via PayPal account: " + email);
    }
}

// 現金
public class Cash extends PaymentMethod {
    @Override
    public void pay(double amount) {
        System.out.println("Paying $" + amount + " in cash.");
    }
}

// 支払い処理システム
public class PaymentProcessor {
    // どの支払い方法でも処理できる（ポリモーフィズム）
    public void processPayment(PaymentMethod method, double amount) {
        method.pay(amount);
        method.showReceipt(amount);
    }
}

// 使用例
public class Main {
    public static void main(String[] args) {
        PaymentProcessor processor = new PaymentProcessor();

        // 異なる支払い方法を同じインターフェースで扱う
        PaymentMethod[] methods = {
            new CreditCard("1234-5678-9012-3456"),
            new PayPal("user@example.com"),
            new Cash()
        };

        for (PaymentMethod method : methods) {
            processor.processPayment(method, 100.0);
            System.out.println();
        }
    }
}
```

## 実践例: 図形描画システム

```java
public abstract class Shape {
    protected String color;

    public Shape(String color) {
        this.color = color;
    }

    // 抽象メソッド（サブクラスで実装）
    public abstract double getArea();
    public abstract void draw();

    // 共通メソッド
    public void displayInfo() {
        System.out.println("Shape: " + getClass().getSimpleName());
        System.out.println("Color: " + color);
        System.out.println("Area: " + getArea());
    }
}

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
    public void draw() {
        System.out.println("Drawing a circle with radius " + radius);
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
    public void draw() {
        System.out.println("Drawing a rectangle " + width + "x" + height);
    }
}

// 描画システム
public class DrawingApp {
    public void renderShapes(Shape[] shapes) {
        for (Shape shape : shapes) {
            shape.draw();
            shape.displayInfo();
            System.out.println("---");
        }
    }
}

// 使用例
public class Main {
    public static void main(String[] args) {
        Shape[] shapes = {
            new Circle("Red", 5),
            new Rectangle("Blue", 4, 6),
            new Circle("Green", 3)
        };

        DrawingApp app = new DrawingApp();
        app.renderShapes(shapes);  // ポリモーフィズムで統一的に処理
    }
}
```

## ポリモーフィズムのベストプラクティス

### 1. インターフェースに対してプログラミングする

```java
// 良い例（抽象型で宣言）
List<String> list = new ArrayList<>();

// 避けるべき（具体型で宣言）
ArrayList<String> list = new ArrayList<>();
```

### 2. 戻り値の型はできるだけ抽象的に

```java
// 良い例
public Animal createAnimal(String type) {
    if (type.equals("dog")) {
        return new Dog();
    } else {
        return new Cat();
    }
}

// 具体的な型を返すと柔軟性が低い
public Dog createDog() {
    return new Dog();
}
```

### 3. instanceofの多用を避ける

```java
// 避けるべき
public void handleAnimal(Animal animal) {
    if (animal instanceof Dog) {
        ((Dog) animal).bark();
    } else if (animal instanceof Cat) {
        ((Cat) animal).meow();
    }
}

// 良い例（ポリモーフィズムを活用）
public void handleAnimal(Animal animal) {
    animal.makeSound();  // サブクラスで適切にオーバーライド
}
```

## まとめ

- **ポリモーフィズム**: 同じインターフェースで異なる実装を扱う
- **実行時ポリモーフィズム**: オーバーライドによる動的束縛
- **コンパイル時ポリモーフィズム**: オーバーロード
- **利点**:
  - 柔軟なコード
  - 拡張性
  - 統一的な処理
- **キャスト**: アップキャスト（自動）、ダウンキャスト（明示的）
- **instanceof**: 型チェック
- **ベストプラクティス**: インターフェースに対してプログラミング

次のセクションでは、抽象クラスとインターフェースについて学びます。
