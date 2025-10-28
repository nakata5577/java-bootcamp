# 4-1. ラムダ式

ラムダ式（Lambda Expression）は、Java 8で導入された関数型プログラミングの要素で、コードを簡潔に記述できます。

## ラムダ式とは

**ラムダ式**は、匿名関数（名前のない関数）を簡潔に記述するための構文です。

### 従来の書き方 vs ラムダ式

```java
// 従来の匿名クラス
Runnable runnable1 = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello");
    }
};

// ラムダ式
Runnable runnable2 = () -> System.out.println("Hello");
```

## ラムダ式の構文

### 基本形

```
(パラメータ) -> { 処理 }
```

### パターン

```java
// パラメータなし
() -> System.out.println("Hello");

// パラメータ1つ（型推論、括弧省略可能）
x -> x * 2
(x) -> x * 2
(int x) -> x * 2

// パラメータ複数
(x, y) -> x + y
(int x, int y) -> x + y

// 複数行の処理
(x, y) -> {
    int sum = x + y;
    return sum * 2;
}
```

## 関数型インターフェース

ラムダ式は、**関数型インターフェース**（抽象メソッドが1つだけのインターフェース）の実装として使用されます。

### 主要な関数型インターフェース

#### Predicate<T>（条件判定）

```java
import java.util.function.Predicate;

Predicate<Integer> isPositive = x -> x > 0;

System.out.println(isPositive.test(5));   // true
System.out.println(isPositive.test(-3));  // false

// 実用例
List<Integer> numbers = Arrays.asList(1, -2, 3, -4, 5);
numbers.stream()
       .filter(x -> x > 0)
       .forEach(System.out::println);  // 1, 3, 5
```

#### Function<T, R>（変換）

```java
import java.util.function.Function;

Function<String, Integer> length = s -> s.length();

System.out.println(length.apply("Hello"));  // 5

// 実用例
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
names.stream()
     .map(s -> s.toUpperCase())
     .forEach(System.out::println);  // ALICE, BOB, CHARLIE
```

#### Consumer<T>（消費）

```java
import java.util.function.Consumer;

Consumer<String> print = s -> System.out.println(s);

print.accept("Hello");  // Hello

// 実用例
List<String> items = Arrays.asList("A", "B", "C");
items.forEach(item -> System.out.println(item));
```

#### Supplier<T>（供給）

```java
import java.util.function.Supplier;

Supplier<Double> random = () -> Math.random();

System.out.println(random.get());  // ランダムな値

// 実用例
Supplier<String> greeting = () -> "Hello, World!";
System.out.println(greeting.get());
```

#### BiFunction<T, U, R>（2つの引数を持つ変換）

```java
import java.util.function.BiFunction;

BiFunction<Integer, Integer, Integer> add = (x, y) -> x + y;

System.out.println(add.apply(3, 5));  // 8
```

### カスタム関数型インターフェース

```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}

// 使用例
Calculator add = (a, b) -> a + b;
Calculator multiply = (a, b) -> a * b;

System.out.println(add.calculate(3, 5));      // 8
System.out.println(multiply.calculate(3, 5)); // 15
```

**`@FunctionalInterface`**: 関数型インターフェースであることを明示（省略可能だが推奨）

## メソッド参照

既存のメソッドをラムダ式のように使用できます。

### 種類

```java
// 静的メソッド参照
Function<String, Integer> parse1 = Integer::parseInt;
// 同等: s -> Integer.parseInt(s)

// インスタンスメソッド参照
String str = "Hello";
Supplier<Integer> length1 = str::length;
// 同等: () -> str.length()

// 任意のオブジェクトのインスタンスメソッド参照
Function<String, Integer> length2 = String::length;
// 同等: s -> s.length()

// コンストラクタ参照
Supplier<List<String>> listSupplier = ArrayList::new;
// 同等: () -> new ArrayList<>()
```

### 実用例

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// ラムダ式
names.forEach(name -> System.out.println(name));

// メソッド参照
names.forEach(System.out::println);

// ラムダ式
names.stream()
     .map(name -> name.toUpperCase())
     .forEach(System.out::println);

// メソッド参照
names.stream()
     .map(String::toUpperCase)
     .forEach(System.out::println);
```

## 実践例: コレクションの操作

### リストのフィルタリングとソート

```java
import java.util.*;

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() { return name; }
    public int getAge() { return age; }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}

public class LambdaExample {
    public static void main(String[] args) {
        List<Person> people = Arrays.asList(
            new Person("Alice", 25),
            new Person("Bob", 30),
            new Person("Charlie", 20),
            new Person("David", 35)
        );

        // 従来の方法
        Collections.sort(people, new Comparator<Person>() {
            @Override
            public int compare(Person p1, Person p2) {
                return Integer.compare(p1.getAge(), p2.getAge());
            }
        });

        // ラムダ式
        people.sort((p1, p2) -> Integer.compare(p1.getAge(), p2.getAge()));

        // メソッド参照とComparator
        people.sort(Comparator.comparing(Person::getAge));

        // 逆順
        people.sort(Comparator.comparing(Person::getAge).reversed());

        // 複数の条件
        people.sort(Comparator.comparing(Person::getAge)
                              .thenComparing(Person::getName));

        people.forEach(System.out::println);
    }
}
```

### Comparatorの便利なメソッド

```java
List<String> names = Arrays.asList("Alice", "bob", "Charlie", "david");

// 自然順序
names.sort(Comparator.naturalOrder());

// 逆順
names.sort(Comparator.reverseOrder());

// 大文字小文字を無視
names.sort(String.CASE_INSENSITIVE_ORDER);

// 長さでソート
names.sort(Comparator.comparing(String::length));

// 複数条件
List<Person> people = ...;
people.sort(
    Comparator.comparing(Person::getAge)
              .thenComparing(Person::getName)
);
```

## 実践例: イベントハンドラ（概念）

ラムダ式は、GUIやイベント駆動プログラミングでも活用されます。

```java
// 従来の方法
button.addActionListener(new ActionListener() {
    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("Button clicked");
    }
});

// ラムダ式
button.addActionListener(e -> System.out.println("Button clicked"));
```

## 実践例: カスタムフィルタ

```java
import java.util.*;
import java.util.function.Predicate;

public class FilterExample {
    // 汎用フィルタメソッド
    public static <T> List<T> filter(List<T> list, Predicate<T> predicate) {
        List<T> result = new ArrayList<>();
        for (T item : list) {
            if (predicate.test(item)) {
                result.add(item);
            }
        }
        return result;
    }

    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

        // 偶数のみ
        List<Integer> evens = filter(numbers, n -> n % 2 == 0);
        System.out.println(evens);  // [2, 4, 6, 8, 10]

        // 5より大きい
        List<Integer> greaterThan5 = filter(numbers, n -> n > 5);
        System.out.println(greaterThan5);  // [6, 7, 8, 9, 10]

        // 文字列にも適用
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");
        List<String> longNames = filter(names, s -> s.length() > 4);
        System.out.println(longNames);  // [Alice, Charlie, David]
    }
}
```

## 変数のキャプチャ

ラムダ式は、外側のスコープの変数を参照できます（クロージャ）。

```java
int factor = 2;  // 実質的にfinal

List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
numbers.forEach(n -> System.out.println(n * factor));

// factor = 3;  // エラー: キャプチャされた変数は変更不可
```

**注意**: キャプチャされる変数は、実質的にfinal（変更されない）でなければなりません。

## ラムダ式のベストプラクティス

### 1. 簡潔さを保つ

```java
// 良い例（簡潔）
list.forEach(item -> System.out.println(item));

// 避けるべき（冗長）
list.forEach(item -> {
    System.out.println(item);
});
```

### 2. メソッド参照を活用

```java
// 良い例
names.forEach(System.out::println);

// 冗長
names.forEach(name -> System.out.println(name));
```

### 3. 複雑なロジックは別メソッドに

```java
// 避けるべき（長すぎるラムダ式）
list.stream()
    .filter(item -> {
        // 複雑な処理が続く...
        return someComplexCondition;
    });

// 良い例
list.stream()
    .filter(this::isValid);

private boolean isValid(Item item) {
    // 複雑な処理
    return someComplexCondition;
}
```

### 4. 型推論を活用

```java
// 良い例
BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;

// 冗長
BiFunction<Integer, Integer, Integer> add = (Integer a, Integer b) -> a + b;
```

## まとめ

- **ラムダ式**: 匿名関数を簡潔に記述 `(params) -> expression`
- **関数型インターフェース**: 抽象メソッドが1つのインターフェース
  - `Predicate<T>`: 条件判定
  - `Function<T, R>`: 変換
  - `Consumer<T>`: 消費
  - `Supplier<T>`: 供給
- **メソッド参照**: 既存メソッドをラムダ式のように使用
  - `Class::staticMethod`
  - `instance::method`
  - `Class::instanceMethod`
  - `Class::new`
- **利点**:
  - コードの簡潔化
  - 関数型プログラミングの要素
  - コレクション操作の柔軟性

次のセクションでは、Stream APIについて学びます。
