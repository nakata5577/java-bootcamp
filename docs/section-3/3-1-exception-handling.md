# 3-1. 例外処理

例外処理は、プログラム実行中のエラーを適切に処理するための仕組みです。

## 例外とは

**例外（Exception）** は、プログラムの実行中に発生する異常な状態です。

### 例外が発生する例

```java
// ArithmeticException
int result = 10 / 0;

// NullPointerException
String str = null;
int length = str.length();

// ArrayIndexOutOfBoundsException
int[] arr = {1, 2, 3};
int value = arr[5];
```

## 例外の階層

```
Throwable
├── Error (システムレベルのエラー、通常は処理しない)
│   ├── OutOfMemoryError
│   └── StackOverflowError
└── Exception (プログラムで処理すべき例外)
    ├── RuntimeException (非チェック例外)
    │   ├── NullPointerException
    │   ├── ArrayIndexOutOfBoundsException
    │   └── IllegalArgumentException
    └── IOException (チェック例外)
        ├── FileNotFoundException
        └── SQLException
```

### チェック例外 vs 非チェック例外

| | チェック例外 | 非チェック例外 |
|---|-------------|---------------|
| **継承元** | `Exception`（`RuntimeException`以外） | `RuntimeException` |
| **処理** | 必須（try-catchまたはthrows） | 任意 |
| **用途** | 回復可能なエラー | プログラムのバグ |
| **例** | `IOException`, `SQLException` | `NullPointerException`, `IllegalArgumentException` |

## try-catch-finally

### 基本構文

```java
try {
    // 例外が発生する可能性のあるコード
    int result = 10 / 0;
} catch (ArithmeticException e) {
    // 例外処理
    System.out.println("Division by zero: " + e.getMessage());
} finally {
    // 必ず実行されるコード（例外の有無に関わらず）
    System.out.println("Cleanup");
}
```

### 例

```java
public class ExceptionExample {
    public static void main(String[] args) {
        try {
            int[] numbers = {1, 2, 3};
            System.out.println(numbers[5]);  // ArrayIndexOutOfBoundsException
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Index out of bounds: " + e.getMessage());
        } finally {
            System.out.println("This always executes");
        }
    }
}
```

### 複数のcatchブロック

```java
try {
    String str = null;
    System.out.println(str.length());
} catch (NullPointerException e) {
    System.out.println("Null pointer: " + e.getMessage());
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Array index: " + e.getMessage());
} catch (Exception e) {
    // すべての例外をキャッチ（最後に配置）
    System.out.println("General exception: " + e.getMessage());
}
```

**注意**: より具体的な例外を先に、一般的な例外を後に配置

### マルチキャッチ（Java 7以降）

同じ処理を複数の例外で行う場合、`|` で区切って記述できます。

```java
try {
    // 何らかの処理
} catch (NullPointerException | ArrayIndexOutOfBoundsException e) {
    System.out.println("Error: " + e.getMessage());
}
```

### try-with-resources（Java 7以降）

リソース（ファイル、接続など）を自動的にクローズします。

```java
// 従来の方法
BufferedReader reader = null;
try {
    reader = new BufferedReader(new FileReader("file.txt"));
    String line = reader.readLine();
} catch (IOException e) {
    e.printStackTrace();
} finally {
    if (reader != null) {
        try {
            reader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// try-with-resources
try (BufferedReader reader = new BufferedReader(new FileReader("file.txt"))) {
    String line = reader.readLine();
} catch (IOException e) {
    e.printStackTrace();
}
// readerは自動的にクローズされる
```

**複数のリソース:**

```java
try (FileInputStream fis = new FileInputStream("input.txt");
     FileOutputStream fos = new FileOutputStream("output.txt")) {
    // 処理
} catch (IOException e) {
    e.printStackTrace();
}
```

## 例外のスロー（throw）

`throw` キーワードで例外を明示的に投げることができます。

```java
public class BankAccount {
    private double balance;

    public void withdraw(double amount) {
        if (amount > balance) {
            throw new IllegalArgumentException("Insufficient balance");
        }
        balance -= amount;
    }
}
```

## メソッドの例外宣言（throws）

チェック例外を処理せずに呼び出し元に委譲する場合、`throws` で宣言します。

```java
public void readFile(String filename) throws IOException {
    BufferedReader reader = new BufferedReader(new FileReader(filename));
    String line = reader.readLine();
    reader.close();
}

// 呼び出し側で処理
public void processFile() {
    try {
        readFile("data.txt");
    } catch (IOException e) {
        System.out.println("File error: " + e.getMessage());
    }
}
```

### 複数の例外を宣言

```java
public void processData() throws IOException, SQLException {
    // 処理
}
```

## カスタム例外

独自の例外クラスを定義できます。

### 基本形

```java
// チェック例外（Exceptionを継承）
public class InsufficientFundsException extends Exception {
    public InsufficientFundsException(String message) {
        super(message);
    }
}

// 非チェック例外（RuntimeExceptionを継承）
public class InvalidAccountException extends RuntimeException {
    public InvalidAccountException(String message) {
        super(message);
    }
}
```

### 使用例

```java
public class BankAccount {
    private String accountNumber;
    private double balance;

    public BankAccount(String accountNumber, double balance) {
        if (accountNumber == null || accountNumber.isEmpty()) {
            throw new InvalidAccountException("Account number cannot be empty");
        }
        this.accountNumber = accountNumber;
        this.balance = balance;
    }

    public void withdraw(double amount) throws InsufficientFundsException {
        if (amount > balance) {
            throw new InsufficientFundsException(
                "Cannot withdraw " + amount + ". Balance: " + balance
            );
        }
        balance -= amount;
    }
}

// 使用例
public class Main {
    public static void main(String[] args) {
        try {
            BankAccount account = new BankAccount("123-456", 1000);
            account.withdraw(1500);
        } catch (InsufficientFundsException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### 追加情報を含むカスタム例外

```java
public class InsufficientFundsException extends Exception {
    private double balance;
    private double requestedAmount;

    public InsufficientFundsException(String message, double balance, double requestedAmount) {
        super(message);
        this.balance = balance;
        this.requestedAmount = requestedAmount;
    }

    public double getBalance() {
        return balance;
    }

    public double getRequestedAmount() {
        return requestedAmount;
    }

    public double getShortfall() {
        return requestedAmount - balance;
    }
}

// 使用例
try {
    // ...
} catch (InsufficientFundsException e) {
    System.out.println(e.getMessage());
    System.out.println("Shortfall: $" + e.getShortfall());
}
```

## 例外処理のベストプラクティス

### 1. 具体的な例外をキャッチ

```java
// 良い例
try {
    // ...
} catch (FileNotFoundException e) {
    // ファイルが見つからない場合の処理
} catch (IOException e) {
    // その他のI/O例外の処理
}

// 避けるべき
try {
    // ...
} catch (Exception e) {
    // すべての例外を一括処理（避けるべき）
}
```

### 2. 例外を握りつぶさない

```java
// 悪い例
try {
    // ...
} catch (Exception e) {
    // 何もしない（例外を握りつぶす）
}

// 良い例
try {
    // ...
} catch (Exception e) {
    e.printStackTrace();  // または適切なロギング
    // 適切な処理
}
```

### 3. finallyでリソースをクローズ（またはtry-with-resources）

```java
// 良い例
try (BufferedReader reader = new BufferedReader(new FileReader("file.txt"))) {
    // 処理
} catch (IOException e) {
    e.printStackTrace();
}
```

### 4. カスタム例外には意味のある名前を

```java
// 良い例
public class InvalidEmailFormatException extends Exception { }
public class UserNotFoundException extends Exception { }

// 悪い例
public class MyException extends Exception { }
public class ErrorX extends Exception { }
```

### 5. 例外メッセージは明確に

```java
// 良い例
throw new IllegalArgumentException("Age must be between 0 and 150, got: " + age);

// 悪い例
throw new IllegalArgumentException("Invalid input");
```

### 6. チェック例外 vs 非チェック例外の使い分け

```java
// 回復可能なエラー → チェック例外
public void connectToDatabase() throws DatabaseConnectionException {
    // データベース接続エラーは回復可能（再試行など）
}

// プログラムのバグ → 非チェック例外
public void setAge(int age) {
    if (age < 0) {
        throw new IllegalArgumentException("Age cannot be negative");
        // プログラムのバグ（修正すべき）
    }
}
```

## 実践例: ファイル処理

```java
import java.io.*;

public class FileProcessor {
    public String readFile(String filename) throws FileNotFoundException, IOException {
        StringBuilder content = new StringBuilder();

        try (BufferedReader reader = new BufferedReader(new FileReader(filename))) {
            String line;
            while ((line = reader.readLine()) != null) {
                content.append(line).append("\n");
            }
        }

        return content.toString();
    }

    public void writeFile(String filename, String content) throws IOException {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(filename))) {
            writer.write(content);
        }
    }

    public void processFile(String inputFile, String outputFile) {
        try {
            String content = readFile(inputFile);
            String processed = content.toUpperCase();
            writeFile(outputFile, processed);
            System.out.println("File processed successfully");
        } catch (FileNotFoundException e) {
            System.err.println("File not found: " + e.getMessage());
        } catch (IOException e) {
            System.err.println("I/O error: " + e.getMessage());
        }
    }
}
```

## 実践例: データ検証

```java
public class UserValidator {
    public void validateUser(String username, String email, int age)
            throws ValidationException {

        if (username == null || username.trim().isEmpty()) {
            throw new ValidationException("Username cannot be empty");
        }

        if (email == null || !email.contains("@")) {
            throw new ValidationException("Invalid email format");
        }

        if (age < 0 || age > 150) {
            throw new ValidationException("Age must be between 0 and 150");
        }
    }
}

// カスタム例外
public class ValidationException extends Exception {
    public ValidationException(String message) {
        super(message);
    }
}

// 使用例
public class Main {
    public static void main(String[] args) {
        UserValidator validator = new UserValidator();

        try {
            validator.validateUser("alice", "alice@example.com", 25);
            System.out.println("User is valid");
        } catch (ValidationException e) {
            System.out.println("Validation error: " + e.getMessage());
        }
    }
}
```

## まとめ

- **例外**: プログラム実行中の異常な状態
- **チェック例外**: 処理が必須（`IOException`など）
- **非チェック例外**: 処理は任意（`NullPointerException`など）
- **try-catch-finally**: 例外処理の基本構文
- **try-with-resources**: リソースの自動クローズ
- **throw**: 例外を投げる
- **throws**: メソッドの例外宣言
- **カスタム例外**: 独自の例外クラスを定義
- **ベストプラクティス**:
  - 具体的な例外をキャッチ
  - 例外を握りつぶさない
  - 意味のある例外メッセージ

次のセクションでは、コレクションフレームワークについて学びます。
