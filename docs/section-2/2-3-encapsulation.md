# 2-3. カプセル化

カプセル化（Encapsulation）は、オブジェクト指向の重要な原則の1つで、データの隠蔽とアクセス制御を実現します。

## カプセル化とは

**カプセル化**は、オブジェクトの内部状態（フィールド）を隠蔽し、外部からのアクセスを制御する仕組みです。

### 目的

1. **データの保護**: 不正な値の設定を防ぐ
2. **実装の隠蔽**: 内部実装の変更が外部に影響しない
3. **メンテナンス性の向上**: コードの変更範囲を限定

## アクセス修飾子

Javaには4つのアクセス修飾子があります。

| 修飾子 | 同じクラス | 同じパッケージ | サブクラス | すべて |
|--------|-----------|---------------|-----------|--------|
| `private` | ○ | × | × | × |
| (デフォルト) | ○ | ○ | × | × |
| `protected` | ○ | ○ | ○ | × |
| `public` | ○ | ○ | ○ | ○ |

### private

クラス内からのみアクセス可能です。

```java
public class BankAccount {
    private double balance;  // 外部から直接アクセス不可

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public double getBalance() {
        return balance;
    }
}

// 使用例
BankAccount account = new BankAccount();
// account.balance = 1000;  // エラー: privateフィールドにアクセス不可
account.deposit(1000);  // OK
```

### public

どこからでもアクセス可能です。

```java
public class Person {
    public String name;  // どこからでもアクセス可能
}

Person person = new Person();
person.name = "Alice";  // OK
```

### protected

同じパッケージまたはサブクラスからアクセス可能です（詳細は継承のセクションで）。

### デフォルト（パッケージプライベート）

修飾子を指定しない場合、同じパッケージ内からのみアクセス可能です。

```java
class DefaultAccessClass {
    int value;  // デフォルト（パッケージプライベート）
}
```

## ゲッターとセッター

カプセル化を実現する基本的なパターンです。

### 基本形

```java
public class Person {
    private String name;
    private int age;

    // ゲッター（Getter）
    public String getName() {
        return name;
    }

    // セッター（Setter）
    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}

// 使用例
Person person = new Person();
person.setName("Alice");
person.setAge(25);
System.out.println(person.getName());  // Alice
```

### バリデーション（検証）

セッター内でデータの妥当性をチェックできます。

```java
public class Person {
    private String name;
    private int age;
    private String email;

    public void setName(String name) {
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("Name cannot be empty");
        }
        this.name = name;
    }

    public void setAge(int age) {
        if (age < 0 || age > 150) {
            throw new IllegalArgumentException("Age must be between 0 and 150");
        }
        this.age = age;
    }

    public void setEmail(String email) {
        if (email == null || !email.contains("@")) {
            throw new IllegalArgumentException("Invalid email format");
        }
        this.email = email;
    }

    // ゲッター
    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getEmail() {
        return email;
    }
}
```

### 読み取り専用プロパティ

セッターを提供しないことで、読み取り専用にできます。

```java
public class Product {
    private final String id;  // 変更不可
    private String name;
    private double price;

    public Product(String id, String name, double price) {
        this.id = id;
        this.name = name;
        this.price = price;
    }

    // idにはゲッターのみ（セッターなし）
    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }
}
```

### 計算プロパティ

ゲッターで値を計算することも可能です。

```java
public class Rectangle {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    public double getWidth() {
        return width;
    }

    public void setWidth(double width) {
        if (width <= 0) {
            throw new IllegalArgumentException("Width must be positive");
        }
        this.width = width;
    }

    public double getHeight() {
        return height;
    }

    public void setHeight(double height) {
        if (height <= 0) {
            throw new IllegalArgumentException("Height must be positive");
        }
        this.height = height;
    }

    // 計算プロパティ（セッターなし）
    public double getArea() {
        return width * height;
    }

    public double getPerimeter() {
        return 2 * (width + height);
    }
}

// 使用例
Rectangle rect = new Rectangle(5, 3);
System.out.println("Area: " + rect.getArea());  // 15.0
System.out.println("Perimeter: " + rect.getPerimeter());  // 16.0
```

## イミュータブル（不変）オブジェクト

一度作成したら変更できないオブジェクトです。

### 実装方法

1. すべてのフィールドを `private final` にする
2. セッターを提供しない
3. コンストラクタですべてのフィールドを初期化
4. 可変オブジェクトのフィールドは、防御的コピーを行う

```java
public final class ImmutablePerson {
    private final String name;
    private final int age;
    private final String email;

    public ImmutablePerson(String name, int age, String email) {
        this.name = name;
        this.age = age;
        this.email = email;
    }

    // ゲッターのみ
    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getEmail() {
        return email;
    }

    // 変更が必要な場合は、新しいオブジェクトを返す
    public ImmutablePerson withAge(int newAge) {
        return new ImmutablePerson(this.name, newAge, this.email);
    }
}
```

### イミュータブルオブジェクトの利点

- **スレッドセーフ**: 並行処理でも安全
- **予測可能**: 状態が変わらないため、バグが減る
- **キャッシュしやすい**: 変更されないため、安全にキャッシュ可能

## カプセル化のベストプラクティス

### 1. フィールドは原則privateに

```java
// 良い例
public class Person {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

// 悪い例
public class Person {
    public String name;  // 外部から直接変更可能
}
```

### 2. セッターでバリデーションを行う

```java
public class BankAccount {
    private double balance;

    public void setBalance(double balance) {
        if (balance < 0) {
            throw new IllegalArgumentException("Balance cannot be negative");
        }
        this.balance = balance;
    }
}
```

### 3. 可変オブジェクトの防御的コピー

```java
import java.util.Date;

public class Event {
    private String name;
    private Date date;

    public Event(String name, Date date) {
        this.name = name;
        this.date = new Date(date.getTime());  // 防御的コピー
    }

    public Date getDate() {
        return new Date(date.getTime());  // 防御的コピー
    }

    public void setDate(Date date) {
        this.date = new Date(date.getTime());  // 防御的コピー
    }
}
```

### 4. 最小限の公開インターフェース

必要なメソッドのみをpublicにします。

```java
public class Calculator {
    // 内部処理用のprivateメソッド
    private int multiply(int a, int b) {
        return a * b;
    }

    // 外部に公開するpublicメソッド
    public int calculateSquare(int n) {
        return multiply(n, n);
    }
}
```

## 実践例: BankAccountクラス

```java
public class BankAccount {
    private String accountNumber;
    private String ownerName;
    private double balance;

    public BankAccount(String accountNumber, String ownerName) {
        this.accountNumber = accountNumber;
        this.ownerName = ownerName;
        this.balance = 0.0;
    }

    // 預金
    public void deposit(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Deposit amount must be positive");
        }
        balance += amount;
        System.out.println("Deposited: $" + amount);
    }

    // 出金
    public void withdraw(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Withdrawal amount must be positive");
        }
        if (amount > balance) {
            throw new IllegalArgumentException("Insufficient balance");
        }
        balance -= amount;
        System.out.println("Withdrawn: $" + amount);
    }

    // 残高照会（読み取り専用）
    public double getBalance() {
        return balance;
    }

    public String getAccountNumber() {
        return accountNumber;
    }

    public String getOwnerName() {
        return ownerName;
    }

    // 口座情報の表示
    public void displayInfo() {
        System.out.println("Account: " + accountNumber);
        System.out.println("Owner: " + ownerName);
        System.out.println("Balance: $" + balance);
    }
}

// 使用例
public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount("123-456", "Alice");

        account.deposit(1000);
        account.withdraw(200);
        account.displayInfo();

        // account.balance = -500;  // エラー: privateフィールド
    }
}
```

## まとめ

- **カプセル化**: データの隠蔽とアクセス制御
- **アクセス修飾子**: `private`, `default`, `protected`, `public`
- **ゲッター/セッター**: フィールドへの安全なアクセス
- **バリデーション**: セッターで不正な値を防ぐ
- **イミュータブル**: 変更不可のオブジェクト（スレッドセーフ）
- **ベストプラクティス**:
  - フィールドは原則private
  - セッターでバリデーション
  - 最小限の公開インターフェース

次のセクションでは、継承について学びます。
