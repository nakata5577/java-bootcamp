# 4-3. Optional

Optionalは、値が存在しない可能性がある場合に使用するコンテナクラスで、NullPointerExceptionを避けるための現代的なアプローチです（Java 8以降）。

## Optionalとは

**Optional<T>** は、値が存在するかどうかを明示的に表現するラッパークラスです。

### 従来の問題

```java
// nullチェックが必要
public String getUserName(User user) {
    if (user != null) {
        return user.getName();
    } else {
        return "Unknown";
    }
}

// nullチェックを忘れるとNullPointerException
String name = user.getName();  // userがnullの場合エラー
```

### Optionalを使った解決

```java
public Optional<String> getUserName(Optional<User> user) {
    return user.map(User::getName);
}

// 安全に値を取得
String name = getUserName(optionalUser).orElse("Unknown");
```

## Optionalの作成

### 値がある場合

```java
Optional<String> optional = Optional.of("Hello");

// nullを渡すとNullPointerException
// Optional<String> bad = Optional.of(null);  // エラー
```

### 値がないかもしれない場合

```java
String value = "Hello";
Optional<String> optional = Optional.ofNullable(value);

String nullValue = null;
Optional<String> empty = Optional.ofNullable(nullValue);  // 空のOptional
```

### 空のOptional

```java
Optional<String> empty = Optional.empty();
```

## 値の存在確認

### isPresent / isEmpty

```java
Optional<String> optional = Optional.of("Hello");

// 値が存在するか
if (optional.isPresent()) {
    System.out.println(optional.get());
}

// 値が存在しないか（Java 11以降）
if (optional.isEmpty()) {
    System.out.println("No value");
}
```

### ifPresent

値が存在する場合のみ処理を実行します。

```java
Optional<String> optional = Optional.of("Hello");

// 従来の方法
if (optional.isPresent()) {
    System.out.println(optional.get());
}

// ifPresentを使用（推奨）
optional.ifPresent(System.out::println);
```

### ifPresentOrElse（Java 9以降）

値が存在する場合と存在しない場合の両方を処理します。

```java
Optional<String> optional = Optional.of("Hello");

optional.ifPresentOrElse(
    value -> System.out.println("Value: " + value),
    () -> System.out.println("No value")
);
```

## 値の取得

### get()（非推奨）

値を直接取得しますが、値がない場合はNoSuchElementExceptionが発生します。

```java
Optional<String> optional = Optional.of("Hello");
String value = optional.get();  // "Hello"

Optional<String> empty = Optional.empty();
// String value2 = empty.get();  // NoSuchElementException
```

**注意**: `get()` の使用は避け、以下のメソッドを使用してください。

### orElse

値がない場合のデフォルト値を指定します。

```java
Optional<String> optional = Optional.of("Hello");
String value1 = optional.orElse("Default");  // "Hello"

Optional<String> empty = Optional.empty();
String value2 = empty.orElse("Default");  // "Default"
```

### orElseGet

値がない場合にSupplierを使ってデフォルト値を生成します。

```java
Optional<String> empty = Optional.empty();

// orElse: 常にデフォルト値を評価
String value1 = empty.orElse(generateDefault());

// orElseGet: 値がない場合のみデフォルト値を評価（効率的）
String value2 = empty.orElseGet(() -> generateDefault());
```

**使い分け**: デフォルト値の生成コストが高い場合は `orElseGet` を使用

### orElseThrow

値がない場合に例外をスローします。

```java
Optional<String> empty = Optional.empty();

// デフォルトでNoSuchElementException
String value1 = empty.orElseThrow();

// カスタム例外
String value2 = empty.orElseThrow(
    () -> new IllegalStateException("Value not found")
);
```

## 値の変換と操作

### map

値が存在する場合、変換を適用します。

```java
Optional<String> optional = Optional.of("hello");

Optional<String> upper = optional.map(String::toUpperCase);
System.out.println(upper.orElse(""));  // "HELLO"

Optional<Integer> length = optional.map(String::length);
System.out.println(length.orElse(0));  // 5
```

### flatMap

ネストしたOptionalをフラット化します。

```java
class Person {
    private Optional<Address> address;

    public Optional<Address> getAddress() {
        return address;
    }
}

class Address {
    private String city;

    public String getCity() {
        return city;
    }
}

Optional<Person> person = Optional.of(new Person());

// mapを使うとOptional<Optional<String>>になってしまう
// Optional<Optional<String>> nestedCity = person.map(p -> p.getAddress().map(Address::getCity));

// flatMapを使う
Optional<String> city = person
    .flatMap(Person::getAddress)
    .map(Address::getCity);
```

### filter

条件に合う値のみを残します。

```java
Optional<Integer> number = Optional.of(25);

Optional<Integer> filtered = number.filter(n -> n > 20);
System.out.println(filtered.orElse(0));  // 25

Optional<Integer> filtered2 = number.filter(n -> n > 30);
System.out.println(filtered2.orElse(0));  // 0（条件に合わない）
```

### or（Java 9以降）

値がない場合に別のOptionalを返します。

```java
Optional<String> optional1 = Optional.empty();
Optional<String> optional2 = Optional.of("Alternative");

Optional<String> result = optional1.or(() -> optional2);
System.out.println(result.orElse(""));  // "Alternative"
```

## 実践例: ユーザー検索

```java
class User {
    private String id;
    private String name;
    private String email;

    public User(String id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }

    public String getId() { return id; }
    public String getName() { return name; }
    public String getEmail() { return email; }
}

class UserRepository {
    private Map<String, User> users = new HashMap<>();

    public UserRepository() {
        users.put("1", new User("1", "Alice", "alice@example.com"));
        users.put("2", new User("2", "Bob", "bob@example.com"));
    }

    // Optionalを返すメソッド
    public Optional<User> findById(String id) {
        return Optional.ofNullable(users.get(id));
    }

    public Optional<User> findByEmail(String email) {
        return users.values().stream()
            .filter(user -> user.getEmail().equals(email))
            .findFirst();
    }
}

public class OptionalExample {
    public static void main(String[] args) {
        UserRepository repo = new UserRepository();

        // 存在するユーザー
        Optional<User> user1 = repo.findById("1");
        user1.ifPresent(u -> System.out.println("Found: " + u.getName()));

        // 存在しないユーザー
        Optional<User> user2 = repo.findById("999");
        String name = user2.map(User::getName).orElse("Unknown");
        System.out.println("Name: " + name);  // "Unknown"

        // メールアドレスで検索
        String email = repo.findByEmail("alice@example.com")
            .map(User::getEmail)
            .orElse("Not found");
        System.out.println("Email: " + email);
    }
}
```

## 実践例: チェーン処理

```java
class Order {
    private String id;
    private Customer customer;

    public Optional<Customer> getCustomer() {
        return Optional.ofNullable(customer);
    }
}

class Customer {
    private String name;
    private Address address;

    public String getName() { return name; }

    public Optional<Address> getAddress() {
        return Optional.ofNullable(address);
    }
}

class Address {
    private String city;

    public String getCity() { return city; }
}

public class OrderProcessor {
    public String getOrderCity(Order order) {
        // 従来の方法（nullチェックの連鎖）
        if (order != null) {
            Customer customer = order.getCustomer();
            if (customer != null) {
                Address address = customer.getAddress();
                if (address != null) {
                    return address.getCity();
                }
            }
        }
        return "Unknown";

        // Optionalを使った方法（簡潔）
        return Optional.ofNullable(order)
            .flatMap(Order::getCustomer)
            .flatMap(Customer::getAddress)
            .map(Address::getCity)
            .orElse("Unknown");
    }
}
```

## Optionalのベストプラクティス

### 1. 戻り値としてOptionalを使用

```java
// 良い例
public Optional<User> findUser(String id) {
    return Optional.ofNullable(users.get(id));
}

// 避けるべき（nullを返す）
public User findUser(String id) {
    return users.get(id);  // nullの可能性
}
```

### 2. フィールドにOptionalを使わない

```java
// 避けるべき
public class User {
    private Optional<String> email;  // 推奨されない
}

// 良い例
public class User {
    private String email;  // nullableなフィールド

    public Optional<String> getEmail() {
        return Optional.ofNullable(email);
    }
}
```

### 3. get()を避ける

```java
// 避けるべき
if (optional.isPresent()) {
    String value = optional.get();
    // ...
}

// 良い例
optional.ifPresent(value -> {
    // ...
});

// または
String value = optional.orElse("default");
```

### 4. orElse vs orElseGet

```java
// コストの低いデフォルト値
String value1 = optional.orElse("default");

// コストの高いデフォルト値
String value2 = optional.orElseGet(() -> expensiveOperation());
```

### 5. コレクションにはOptionalを使わない

```java
// 避けるべき
Optional<List<String>> optionalList = ...;

// 良い例（空のリストを返す）
List<String> list = ...;  // 空リストか要素があるリスト
```

## Stream APIとの連携

```java
List<Optional<String>> optionals = Arrays.asList(
    Optional.of("A"),
    Optional.empty(),
    Optional.of("B"),
    Optional.empty(),
    Optional.of("C")
);

// Optional.stream()を使ってフラット化（Java 9以降）
List<String> values = optionals.stream()
    .flatMap(Optional::stream)
    .collect(Collectors.toList());

System.out.println(values);  // [A, B, C]

// Java 8の場合
List<String> values2 = optionals.stream()
    .filter(Optional::isPresent)
    .map(Optional::get)
    .collect(Collectors.toList());
```

## まとめ

### Optionalの作成
- `Optional.of(value)`: 値がある（nullは不可）
- `Optional.ofNullable(value)`: 値があるかもしれない
- `Optional.empty()`: 空のOptional

### 値の取得
- `orElse(default)`: デフォルト値
- `orElseGet(supplier)`: デフォルト値を生成
- `orElseThrow()`: 例外をスロー

### 値の操作
- `map(function)`: 変換
- `flatMap(function)`: ネストしたOptionalをフラット化
- `filter(predicate)`: 条件フィルタ

### ベストプラクティス
- 戻り値としてOptionalを使用
- フィールドには使わない
- `get()` を避ける
- コレクションには使わない

次のセクションからは、実践開発に必要なツールと技術について学びます。
