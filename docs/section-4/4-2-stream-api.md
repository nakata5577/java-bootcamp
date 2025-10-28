# 4-2. Stream API

Stream APIは、コレクションのデータを宣言的かつ効率的に処理するための強力な機能です（Java 8以降）。

## Stream APIとは

**Stream**は、データの連続した流れを表し、関数型プログラミングのスタイルで操作できます。

### 特徴

- **宣言的**: 「何をするか」を記述（「どうやるか」ではない）
- **パイプライン**: 複数の操作を連鎖できる
- **遅延評価**: 終端操作が呼ばれるまで実行されない
- **並列処理**: 簡単に並列化できる

### 従来の方法 vs Stream API

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

// 従来の方法
List<String> filtered = new ArrayList<>();
for (String name : names) {
    if (name.length() > 3) {
        filtered.add(name.toUpperCase());
    }
}
Collections.sort(filtered);

// Stream API
List<String> result = names.stream()
    .filter(name -> name.length() > 3)
    .map(String::toUpperCase)
    .sorted()
    .collect(Collectors.toList());
```

## Streamの作成

### コレクションから

```java
List<String> list = Arrays.asList("A", "B", "C");
Stream<String> stream = list.stream();
```

### 配列から

```java
String[] array = {"A", "B", "C"};
Stream<String> stream = Arrays.stream(array);
```

### 値から

```java
Stream<String> stream = Stream.of("A", "B", "C");
```

### 範囲から

```java
// 1から10まで（10を含まない）
IntStream range = IntStream.range(1, 10);

// 1から10まで（10を含む）
IntStream rangeClosed = IntStream.rangeClosed(1, 10);
```

### 無限ストリーム

```java
// 0, 2, 4, 6, ...
Stream<Integer> evenNumbers = Stream.iterate(0, n -> n + 2);

// ランダムな値
Stream<Double> randoms = Stream.generate(Math::random);

// 実用例（limit で制限）
Stream.iterate(1, n -> n * 2)
      .limit(10)
      .forEach(System.out::println);  // 1, 2, 4, 8, ...
```

## 中間操作（Intermediate Operations）

Streamを返し、複数連鎖できます。

### filter（フィルタリング）

条件に合う要素のみを通過させます。

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// 偶数のみ
numbers.stream()
       .filter(n -> n % 2 == 0)
       .forEach(System.out::println);  // 2, 4, 6, 8, 10
```

### map（変換）

各要素を別の値に変換します。

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// 大文字に変換
names.stream()
     .map(String::toUpperCase)
     .forEach(System.out::println);  // ALICE, BOB, CHARLIE

// 長さに変換
names.stream()
     .map(String::length)
     .forEach(System.out::println);  // 5, 3, 7
```

### flatMap（フラット化）

ネストした構造をフラットにします。

```java
List<List<Integer>> nested = Arrays.asList(
    Arrays.asList(1, 2, 3),
    Arrays.asList(4, 5),
    Arrays.asList(6, 7, 8, 9)
);

// フラット化
List<Integer> flat = nested.stream()
    .flatMap(List::stream)
    .collect(Collectors.toList());

System.out.println(flat);  // [1, 2, 3, 4, 5, 6, 7, 8, 9]

// 文字列を文字に分解
List<String> words = Arrays.asList("Hello", "World");
words.stream()
     .flatMap(word -> Arrays.stream(word.split("")))
     .forEach(System.out::print);  // HelloWorld
```

### distinct（重複除去）

```java
List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 3, 3, 4, 5);

numbers.stream()
       .distinct()
       .forEach(System.out::println);  // 1, 2, 3, 4, 5
```

### sorted（ソート）

```java
List<Integer> numbers = Arrays.asList(5, 2, 8, 1, 9);

// 自然順序
numbers.stream()
       .sorted()
       .forEach(System.out::println);  // 1, 2, 5, 8, 9

// カスタム順序（逆順）
numbers.stream()
       .sorted(Comparator.reverseOrder())
       .forEach(System.out::println);  // 9, 8, 5, 2, 1
```

### limit / skip

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

// 最初の5つ
numbers.stream()
       .limit(5)
       .forEach(System.out::println);  // 1, 2, 3, 4, 5

// 最初の3つをスキップ
numbers.stream()
       .skip(3)
       .forEach(System.out::println);  // 4, 5, 6, 7, 8, 9, 10
```

### peek（デバッグ）

中間結果を確認するために使用します。

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

numbers.stream()
       .peek(n -> System.out.println("Before: " + n))
       .map(n -> n * 2)
       .peek(n -> System.out.println("After: " + n))
       .collect(Collectors.toList());
```

## 終端操作（Terminal Operations）

Streamの処理を開始し、結果を生成します。

### forEach

各要素に対して処理を実行します。

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
names.stream().forEach(System.out::println);
```

### collect

Streamの要素を収集します。

```java
import java.util.stream.Collectors;

List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// リストに収集
List<String> list = names.stream()
    .filter(name -> name.length() > 3)
    .collect(Collectors.toList());

// セットに収集
Set<String> set = names.stream()
    .collect(Collectors.toSet());

// 文字列に結合
String joined = names.stream()
    .collect(Collectors.joining(", "));
System.out.println(joined);  // Alice, Bob, Charlie

// マップに収集
Map<String, Integer> map = names.stream()
    .collect(Collectors.toMap(
        name -> name,
        String::length
    ));
```

### reduce（集約）

要素を1つの値に集約します。

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// 合計
int sum = numbers.stream()
    .reduce(0, (a, b) -> a + b);
// または
int sum2 = numbers.stream()
    .reduce(0, Integer::sum);
System.out.println(sum);  // 15

// 積
int product = numbers.stream()
    .reduce(1, (a, b) -> a * b);
System.out.println(product);  // 120

// 最大値
Optional<Integer> max = numbers.stream()
    .reduce(Integer::max);
```

### count

要素数をカウントします。

```java
long count = names.stream()
    .filter(name -> name.length() > 3)
    .count();
```

### anyMatch / allMatch / noneMatch

条件のマッチング判定を行います。

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// いずれかが偶数か
boolean anyEven = numbers.stream()
    .anyMatch(n -> n % 2 == 0);  // true

// すべてが正の数か
boolean allPositive = numbers.stream()
    .allMatch(n -> n > 0);  // true

// 負の数がないか
boolean noNegative = numbers.stream()
    .noneMatch(n -> n < 0);  // true
```

### findFirst / findAny

最初の要素または任意の要素を取得します。

```java
Optional<Integer> first = numbers.stream()
    .filter(n -> n > 3)
    .findFirst();  // Optional[4]

Optional<Integer> any = numbers.stream()
    .filter(n -> n > 3)
    .findAny();
```

### min / max

最小値・最大値を取得します。

```java
Optional<Integer> min = numbers.stream()
    .min(Integer::compareTo);

Optional<Integer> max = numbers.stream()
    .max(Integer::compareTo);
```

## Collectorsの便利なメソッド

### groupingBy（グループ化）

```java
class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public int getAge() { return age; }
    public String getName() { return name; }
}

List<Person> people = Arrays.asList(
    new Person("Alice", 25),
    new Person("Bob", 30),
    new Person("Charlie", 25),
    new Person("David", 30)
);

// 年齢でグループ化
Map<Integer, List<Person>> byAge = people.stream()
    .collect(Collectors.groupingBy(Person::getAge));

// {25=[Alice, Charlie], 30=[Bob, David]}
```

### partitioningBy（分割）

条件でtrueとfalseに分割します。

```java
Map<Boolean, List<Integer>> partitioned = numbers.stream()
    .collect(Collectors.partitioningBy(n -> n % 2 == 0));

// {false=[1, 3, 5], true=[2, 4]}
```

### summarizingInt（統計情報）

```java
IntSummaryStatistics stats = people.stream()
    .collect(Collectors.summarizingInt(Person::getAge));

System.out.println("Count: " + stats.getCount());
System.out.println("Sum: " + stats.getSum());
System.out.println("Min: " + stats.getMin());
System.out.println("Max: " + stats.getMax());
System.out.println("Average: " + stats.getAverage());
```

## プリミティブStream

プリミティブ型用の特殊なStreamがあります。

```java
// IntStream
IntStream.range(1, 5)
         .forEach(System.out::println);  // 1, 2, 3, 4

// 統計
IntStream numbers = IntStream.of(1, 2, 3, 4, 5);
int sum = numbers.sum();
OptionalDouble average = IntStream.of(1, 2, 3, 4, 5).average();

// mapToInt（IntStreamに変換）
List<String> strings = Arrays.asList("1", "2", "3", "4", "5");
int total = strings.stream()
    .mapToInt(Integer::parseInt)
    .sum();
```

## 並列Stream

簡単に並列処理できます。

```java
// 通常のStream
long count = IntStream.range(1, 1000000)
    .filter(n -> n % 2 == 0)
    .count();

// 並列Stream
long count2 = IntStream.range(1, 1000000)
    .parallel()
    .filter(n -> n % 2 == 0)
    .count();
```

**注意**: 並列化は常に高速化するわけではありません。小さなデータセットやシンプルな操作では、オーバーヘッドの方が大きい場合があります。

!!! warning "並列Streamの注意点"
    - **小規模データ**: 数百件以下では通常のStreamの方が速い
    - **I/O操作**: ディスクやネットワークアクセスを含む場合は効果が薄い
    - **スレッドセーフ**: 共有状態を変更する操作は避ける
    - **順序依存**: 順序が重要な処理には不向き

    **並列化が有効な場合:**
    - データ量が大きい（数万件以上）
    - 計算量の多い処理（数学演算、暗号化など）
    - 独立した処理（副作用なし）

## 実践例: データ分析

```java
class Transaction {
    private String category;
    private double amount;

    public Transaction(String category, double amount) {
        this.category = category;
        this.amount = amount;
    }

    public String getCategory() { return category; }
    public double getAmount() { return amount; }
}

public class StreamExample {
    public static void main(String[] args) {
        List<Transaction> transactions = Arrays.asList(
            new Transaction("Food", 50.0),
            new Transaction("Transport", 30.0),
            new Transaction("Food", 75.0),
            new Transaction("Entertainment", 100.0),
            new Transaction("Transport", 25.0)
        );

        // カテゴリごとの合計
        Map<String, Double> totalByCategory = transactions.stream()
            .collect(Collectors.groupingBy(
                Transaction::getCategory,
                Collectors.summingDouble(Transaction::getAmount)
            ));

        System.out.println(totalByCategory);
        // {Food=125.0, Transport=55.0, Entertainment=100.0}

        // 最高額の取引
        Optional<Transaction> maxTransaction = transactions.stream()
            .max(Comparator.comparing(Transaction::getAmount));

        // 合計金額
        double total = transactions.stream()
            .mapToDouble(Transaction::getAmount)
            .sum();

        System.out.println("Total: " + total);  // 280.0
    }
}
```

## Stream APIのパフォーマンスTips

### 1. 適切な中間操作の順序

```java
// 悪い例: filterの前にmap
list.stream()
    .map(expensiveOperation)  // すべての要素に適用
    .filter(condition)
    .collect(Collectors.toList());

// 良い例: mapの前にfilter
list.stream()
    .filter(condition)  // 必要な要素だけ残す
    .map(expensiveOperation)  // 減った要素にのみ適用
    .collect(Collectors.toList());
```

### 2. 終端操作の選択

```java
// 存在確認だけなら findAny や anyMatch を使用
// 悪い例
boolean hasEven = list.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList())
    .size() > 0;

// 良い例
boolean hasEven = list.stream()
    .anyMatch(n -> n % 2 == 0);  // 最初の一致で終了
```

### 3. プリミティブStreamの活用

```java
// 悪い例: オートボクシングのオーバーヘッド
int sum = list.stream()
    .filter(n -> n % 2 == 0)
    .reduce(0, Integer::sum);

// 良い例: IntStreamを使用
int sum = list.stream()
    .filter(n -> n % 2 == 0)
    .mapToInt(n -> n)
    .sum();
```

## まとめ

### Streamの作成
- `collection.stream()`
- `Arrays.stream(array)`
- `Stream.of(values)`
- `IntStream.range(start, end)`

### 中間操作
- `filter`: フィルタリング
- `map`: 変換
- `flatMap`: フラット化
- `distinct`: 重複除去
- `sorted`: ソート
- `limit / skip`: 制限

### 終端操作
- `forEach`: 各要素に処理
- `collect`: 収集
- `reduce`: 集約
- `count`: カウント
- `anyMatch / allMatch / noneMatch`: 条件判定
- `findFirst / findAny`: 検索
- `min / max`: 最小・最大

### ポイント
- 宣言的で読みやすいコード
- 遅延評価による効率性
- 並列処理の容易さ
- 適切な操作順序でパフォーマンス向上

!!! success "Stream APIをマスターしよう"
    Stream APIはモダンJavaの核心機能です。最初は慣れないかもしれませんが、使い続けることで直感的に書けるようになります。

次のセクションでは、Optionalについて学びます。
