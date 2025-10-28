# 3-3. ジェネリクス

ジェネリクス（Generics）は、型安全性を保ちながら再利用可能なコードを書くための仕組みです。

## ジェネリクスとは

型をパラメータとして扱うことで、異なる型に対して同じコードを使用できます。

### ジェネリクスなしの問題

```java
// Objectを使った汎用リスト
List list = new ArrayList();
list.add("Hello");
list.add(123);

// 取得時に型キャストが必要
String str = (String) list.get(0);

// 実行時エラーのリスク
String str2 = (String) list.get(1);  // ClassCastException
```

### ジェネリクスを使った解決

```java
// 型パラメータで型安全性を確保
List<String> list = new ArrayList<>();
list.add("Hello");
// list.add(123);  // コンパイルエラー

// キャスト不要
String str = list.get(0);
```

## ジェネリッククラス

型パラメータを持つクラスを定義できます。

### 基本形

```java
public class Box<T> {
    private T content;

    public void set(T content) {
        this.content = content;
    }

    public T get() {
        return content;
    }
}

// 使用例
Box<String> stringBox = new Box<>();
stringBox.set("Hello");
String value = stringBox.get();

Box<Integer> intBox = new Box<>();
intBox.set(123);
int number = intBox.get();
```

### 複数の型パラメータ

```java
public class Pair<K, V> {
    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() {
        return key;
    }

    public V getValue() {
        return value;
    }
}

// 使用例
Pair<String, Integer> pair = new Pair<>("Age", 25);
String key = pair.getKey();
Integer value = pair.getValue();
```

### 型パラメータの命名規則

| 記号 | 意味 |
|------|------|
| `E` | Element（要素） |
| `K` | Key（キー） |
| `V` | Value（値） |
| `N` | Number（数値） |
| `T` | Type（型） |
| `S`, `U`, `V` | 2番目、3番目、4番目の型 |

## ジェネリックメソッド

メソッドレベルで型パラメータを定義できます。

### 基本形

```java
public class Util {
    // ジェネリックメソッド
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.print(element + " ");
        }
        System.out.println();
    }
}

// 使用例
Integer[] intArray = {1, 2, 3, 4, 5};
String[] strArray = {"A", "B", "C"};

Util.printArray(intArray);  // 1 2 3 4 5
Util.printArray(strArray);  // A B C
```

### 戻り値のある例

```java
public class Util {
    public static <T> T getFirst(T[] array) {
        if (array == null || array.length == 0) {
            return null;
        }
        return array[0];
    }
}

// 使用例
String[] names = {"Alice", "Bob", "Charlie"};
String first = Util.getFirst(names);  // "Alice"
```

## 境界型パラメータ（Bounded Type Parameters）

型パラメータに制約を設けることができます。

### extends（上限境界）

```java
// Number型またはそのサブクラスのみ許可
public class NumberBox<T extends Number> {
    private T number;

    public NumberBox(T number) {
        this.number = number;
    }

    public double doubleValue() {
        return number.doubleValue();
    }
}

// 使用例
NumberBox<Integer> intBox = new NumberBox<>(123);
NumberBox<Double> doubleBox = new NumberBox<>(3.14);
// NumberBox<String> strBox = new NumberBox<>("Hello");  // コンパイルエラー
```

### 複数の境界

```java
// ComparableとSerializableの両方を実装
public class SortedBox<T extends Comparable<T> & Serializable> {
    private List<T> items = new ArrayList<>();

    public void add(T item) {
        items.add(item);
        Collections.sort(items);
    }

    public List<T> getItems() {
        return items;
    }
}
```

## ワイルドカード

### 非境界ワイルドカード（?）

任意の型を表します。

```java
public static void printList(List<?> list) {
    for (Object item : list) {
        System.out.println(item);
    }
}

// 使用例
List<Integer> intList = Arrays.asList(1, 2, 3);
List<String> strList = Arrays.asList("A", "B", "C");

printList(intList);
printList(strList);
```

### 上限境界ワイルドカード（? extends Type）

指定した型またはそのサブクラスを表します。

```java
// Numberまたはそのサブクラス
public static double sumOfList(List<? extends Number> list) {
    double sum = 0.0;
    for (Number num : list) {
        sum += num.doubleValue();
    }
    return sum;
}

// 使用例
List<Integer> integers = Arrays.asList(1, 2, 3);
List<Double> doubles = Arrays.asList(1.1, 2.2, 3.3);

double sum1 = sumOfList(integers);  // 6.0
double sum2 = sumOfList(doubles);   // 6.6
```

### 下限境界ワイルドカード（? super Type）

指定した型またはそのスーパークラスを表します。

```java
public static void addNumbers(List<? super Integer> list) {
    list.add(1);
    list.add(2);
    list.add(3);
}

// 使用例
List<Integer> intList = new ArrayList<>();
List<Number> numList = new ArrayList<>();
List<Object> objList = new ArrayList<>();

addNumbers(intList);  // OK
addNumbers(numList);  // OK
addNumbers(objList);  // OK
```

## PECS原則

**PECS**: Producer Extends, Consumer Super

- **読み取り（Producer）**: `? extends T` を使用
- **書き込み（Consumer）**: `? super T` を使用

```java
// Producer: 読み取りのみ
public static void copy(List<? extends Number> source, List<? super Number> dest) {
    for (Number num : source) {  // 読み取り
        dest.add(num);  // 書き込み
    }
}

// 使用例
List<Integer> source = Arrays.asList(1, 2, 3);
List<Number> dest = new ArrayList<>();

copy(source, dest);
```

## 型消去（Type Erasure）

Javaのジェネリクスは、コンパイル時の型チェックのみで、実行時には型情報が消去されます。

```java
List<String> strList = new ArrayList<>();
List<Integer> intList = new ArrayList<>();

// 実行時は同じ型
System.out.println(strList.getClass() == intList.getClass());  // true

// 実行時には型情報がない
// new ArrayList<T>();  // エラー
// if (obj instanceof List<String>) { }  // エラー
```

### 制約

```java
public class MyClass<T> {
    // エラー: Tのインスタンスを作成できない
    // T obj = new T();

    // エラー: Tの配列を作成できない
    // T[] array = new T[10];

    // エラー: 静的フィールドに型パラメータを使用できない
    // private static T value;

    // OK: Objectの配列を使用
    private Object[] array = new Object[10];

    @SuppressWarnings("unchecked")
    public T get(int index) {
        return (T) array[index];
    }
}
```

## 実践例: 汎用的なスタック

```java
public class Stack<T> {
    private List<T> elements = new ArrayList<>();

    public void push(T element) {
        elements.add(element);
    }

    public T pop() {
        if (isEmpty()) {
            throw new IllegalStateException("Stack is empty");
        }
        return elements.remove(elements.size() - 1);
    }

    public T peek() {
        if (isEmpty()) {
            throw new IllegalStateException("Stack is empty");
        }
        return elements.get(elements.size() - 1);
    }

    public boolean isEmpty() {
        return elements.isEmpty();
    }

    public int size() {
        return elements.size();
    }
}

// 使用例
public class Main {
    public static void main(String[] args) {
        Stack<String> stringStack = new Stack<>();
        stringStack.push("A");
        stringStack.push("B");
        stringStack.push("C");

        System.out.println(stringStack.pop());  // C
        System.out.println(stringStack.peek()); // B

        Stack<Integer> intStack = new Stack<>();
        intStack.push(1);
        intStack.push(2);
        intStack.push(3);

        System.out.println(intStack.pop());  // 3
    }
}
```

## 実践例: 汎用的なリポジトリ

```java
// エンティティの基底クラス
public abstract class Entity {
    protected Long id;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }
}

// 汎用リポジトリ
public class Repository<T extends Entity> {
    private Map<Long, T> storage = new HashMap<>();
    private Long nextId = 1L;

    public T save(T entity) {
        if (entity.getId() == null) {
            entity.setId(nextId++);
        }
        storage.put(entity.getId(), entity);
        return entity;
    }

    public T findById(Long id) {
        return storage.get(id);
    }

    public List<T> findAll() {
        return new ArrayList<>(storage.values());
    }

    public void delete(Long id) {
        storage.remove(id);
    }

    public boolean exists(Long id) {
        return storage.containsKey(id);
    }
}

// 具体的なエンティティ
public class User extends Entity {
    private String name;
    private String email;

    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }

    public String getName() { return name; }
    public String getEmail() { return email; }

    @Override
    public String toString() {
        return "User{id=" + id + ", name='" + name + "', email='" + email + "'}";
    }
}

public class Product extends Entity {
    private String name;
    private double price;

    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() { return name; }
    public double getPrice() { return price; }

    @Override
    public String toString() {
        return "Product{id=" + id + ", name='" + name + "', price=" + price + "}";
    }
}

// 使用例
public class Main {
    public static void main(String[] args) {
        Repository<User> userRepo = new Repository<>();
        Repository<Product> productRepo = new Repository<>();

        // ユーザーの保存
        User user1 = userRepo.save(new User("Alice", "alice@example.com"));
        User user2 = userRepo.save(new User("Bob", "bob@example.com"));

        // 商品の保存
        Product product1 = productRepo.save(new Product("Laptop", 999.99));
        Product product2 = productRepo.save(new Product("Mouse", 29.99));

        // 検索
        User foundUser = userRepo.findById(1L);
        System.out.println(foundUser);

        // すべて取得
        System.out.println("All users:");
        for (User user : userRepo.findAll()) {
            System.out.println(user);
        }

        System.out.println("All products:");
        for (Product product : productRepo.findAll()) {
            System.out.println(product);
        }
    }
}
```

## まとめ

- **ジェネリクス**: 型パラメータで型安全性を確保
- **ジェネリッククラス**: `class MyClass<T>`
- **ジェネリックメソッド**: `public <T> void method(T param)`
- **境界型パラメータ**: `<T extends Number>`
- **ワイルドカード**:
  - `?`: 任意の型
  - `? extends T`: 上限境界（Producer）
  - `? super T`: 下限境界（Consumer）
- **PECS原則**: Producer Extends, Consumer Super
- **型消去**: 実行時には型情報が消える

次のセクションでは、ファイル入出力について学びます。
