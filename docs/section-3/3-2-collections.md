# 3-2. コレクションフレームワーク

コレクションフレームワークは、データのグループを効率的に管理するためのクラス群です。

## コレクションフレームワークとは

配列の代わりに、より柔軟で機能的なデータ構造を提供します。

### 主要なインターフェース階層

```
Collection
├── List（順序あり、重複OK）
│   ├── ArrayList
│   ├── LinkedList
│   └── Vector
├── Set（順序なし、重複NG）
│   ├── HashSet
│   ├── LinkedHashSet
│   └── TreeSet
└── Queue（キュー）
    ├── PriorityQueue
    └── LinkedList

Map（キーと値のペア）
├── HashMap
├── LinkedHashMap
├── TreeMap
└── Hashtable
```

## List（リスト）

順序を持ち、重複を許可するコレクションです。

### ArrayList

動的配列の実装で、最も一般的に使用されます。

```java
import java.util.ArrayList;
import java.util.List;

public class ArrayListExample {
    public static void main(String[] args) {
        // 宣言と初期化
        List<String> fruits = new ArrayList<>();

        // 要素の追加
        fruits.add("Apple");
        fruits.add("Banana");
        fruits.add("Orange");
        fruits.add("Apple");  // 重複OK

        // 要素数
        System.out.println("Size: " + fruits.size());  // 4

        // 要素の取得
        String first = fruits.get(0);  // "Apple"

        // 要素の変更
        fruits.set(1, "Mango");

        // 要素の削除
        fruits.remove(0);  // インデックスで削除
        fruits.remove("Orange");  // 値で削除

        // 要素の存在確認
        boolean hasApple = fruits.contains("Apple");

        // インデックスの取得
        int index = fruits.indexOf("Mango");

        // 繰り返し
        for (String fruit : fruits) {
            System.out.println(fruit);
        }

        // クリア
        fruits.clear();
    }
}
```

### LinkedList

双方向リンクリストの実装です。

```java
import java.util.LinkedList;

LinkedList<String> list = new LinkedList<>();

// 先頭に追加
list.addFirst("First");

// 末尾に追加
list.addLast("Last");

// 先頭を取得・削除
String first = list.removeFirst();

// 末尾を取得・削除
String last = list.removeLast();
```

### ArrayList vs LinkedList

| | ArrayList | LinkedList |
|---|-----------|------------|
| **ランダムアクセス** | 高速（O(1)） | 低速（O(n)） |
| **追加・削除（先頭）** | 低速（O(n)） | 高速（O(1)） |
| **追加・削除（末尾）** | 高速（O(1)） | 高速（O(1)） |
| **メモリ** | 効率的 | オーバーヘッドあり |
| **用途** | 一般的な用途 | キュー、頻繁な挿入・削除 |

## Set（セット）

重複を許可しないコレクションです。

### HashSet

ハッシュテーブルを使った実装で、順序は保証されません。

```java
import java.util.HashSet;
import java.util.Set;

Set<String> set = new HashSet<>();

// 要素の追加
set.add("Apple");
set.add("Banana");
set.add("Apple");  // 重複は追加されない

System.out.println(set.size());  // 2

// 要素の存在確認
boolean hasApple = set.contains("Apple");

// 要素の削除
set.remove("Banana");

// 繰り返し（順序は保証されない）
for (String item : set) {
    System.out.println(item);
}
```

### LinkedHashSet

挿入順序を保持するHashSetです。

```java
Set<String> set = new LinkedHashSet<>();
set.add("C");
set.add("A");
set.add("B");

// 挿入順で出力: C, A, B
for (String item : set) {
    System.out.println(item);
}
```

### TreeSet

要素を自然順序（またはComparator）でソートして保持します。

```java
import java.util.TreeSet;

TreeSet<Integer> set = new TreeSet<>();
set.add(5);
set.add(2);
set.add(8);
set.add(1);

// ソートされて出力: 1, 2, 5, 8
for (int num : set) {
    System.out.println(num);
}

// 最小値・最大値
int min = set.first();  // 1
int max = set.last();   // 8
```

## Map（マップ）

キーと値のペアを格納します。

### HashMap

ハッシュテーブルを使った実装で、最も一般的です。

```java
import java.util.HashMap;
import java.util.Map;

Map<String, Integer> scores = new HashMap<>();

// 要素の追加
scores.put("Alice", 95);
scores.put("Bob", 87);
scores.put("Charlie", 92);

// 要素の取得
int aliceScore = scores.get("Alice");  // 95

// 存在しないキー
Integer score = scores.get("David");  // null

// デフォルト値を指定
int davidScore = scores.getOrDefault("David", 0);  // 0

// 要素の存在確認
boolean hasAlice = scores.containsKey("Alice");
boolean has90 = scores.containsValue(90);

// 要素の削除
scores.remove("Bob");

// サイズ
int size = scores.size();

// キーの繰り返し
for (String name : scores.keySet()) {
    System.out.println(name + ": " + scores.get(name));
}

// 値の繰り返し
for (int score : scores.values()) {
    System.out.println(score);
}

// キーと値の繰り返し
for (Map.Entry<String, Integer> entry : scores.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

### LinkedHashMap

挿入順序を保持するHashMapです。

```java
Map<String, Integer> map = new LinkedHashMap<>();
map.put("C", 3);
map.put("A", 1);
map.put("B", 2);

// 挿入順で出力: C=3, A=1, B=2
```

### TreeMap

キーを自然順序でソートして保持します。

```java
import java.util.TreeMap;

TreeMap<String, Integer> map = new TreeMap<>();
map.put("Charlie", 92);
map.put("Alice", 95);
map.put("Bob", 87);

// キーがソートされて出力: Alice=95, Bob=87, Charlie=92
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

## Queue（キュー）

FIFO（First-In-First-Out）の原則で要素を処理します。

### LinkedList（Queueとして）

```java
import java.util.LinkedList;
import java.util.Queue;

Queue<String> queue = new LinkedList<>();

// 要素の追加（末尾）
queue.offer("First");
queue.offer("Second");
queue.offer("Third");

// 先頭を確認（削除しない）
String peek = queue.peek();  // "First"

// 先頭を取得・削除
String poll = queue.poll();  // "First"

// 空かどうか
boolean isEmpty = queue.isEmpty();
```

### PriorityQueue

優先度に基づいて要素を取り出します。

```java
import java.util.PriorityQueue;

PriorityQueue<Integer> pq = new PriorityQueue<>();

pq.offer(5);
pq.offer(2);
pq.offer(8);
pq.offer(1);

// 小さい順に取り出される
while (!pq.isEmpty()) {
    System.out.println(pq.poll());  // 1, 2, 5, 8
}
```

## ユーティリティクラス

### Collections

コレクションを操作する便利なメソッドを提供します。

```java
import java.util.Collections;
import java.util.List;
import java.util.ArrayList;

List<Integer> numbers = new ArrayList<>();
numbers.add(5);
numbers.add(2);
numbers.add(8);
numbers.add(1);

// ソート
Collections.sort(numbers);  // [1, 2, 5, 8]

// 逆順
Collections.reverse(numbers);  // [8, 5, 2, 1]

// シャッフル
Collections.shuffle(numbers);

// 最大値・最小値
int max = Collections.max(numbers);
int min = Collections.min(numbers);

// 検索（ソート済みリスト）
Collections.sort(numbers);
int index = Collections.binarySearch(numbers, 5);

// 頻度
int count = Collections.frequency(numbers, 5);

// 不変リスト
List<String> immutable = Collections.unmodifiableList(numbers);
```

### Arrays

配列とリストの変換などを提供します。

```java
import java.util.Arrays;
import java.util.List;

// 配列 → リスト
String[] array = {"Apple", "Banana", "Orange"};
List<String> list = Arrays.asList(array);

// 注意: Arrays.asListは固定長（追加・削除不可）
// list.add("Mango");  // UnsupportedOperationException

// 可変リストに変換
List<String> mutableList = new ArrayList<>(Arrays.asList(array));
mutableList.add("Mango");  // OK
```

## コレクションの選択ガイド

### 用途に応じた選択

| 用途 | 推奨 |
|------|------|
| 一般的なリスト | `ArrayList` |
| 頻繁な先頭への追加・削除 | `LinkedList` |
| 重複を排除 | `HashSet` |
| ソートされたセット | `TreeSet` |
| 挿入順を保持するセット | `LinkedHashSet` |
| キーと値のペア | `HashMap` |
| ソートされたマップ | `TreeMap` |
| キュー | `LinkedList` |
| 優先度付きキュー | `PriorityQueue` |

## 実践例: 単語の頻度カウント

```java
import java.util.*;

public class WordFrequency {
    public static void main(String[] args) {
        String text = "apple banana apple orange banana apple";
        String[] words = text.split(" ");

        // 単語の頻度をカウント
        Map<String, Integer> frequency = new HashMap<>();

        for (String word : words) {
            frequency.put(word, frequency.getOrDefault(word, 0) + 1);
        }

        // 結果の表示
        for (Map.Entry<String, Integer> entry : frequency.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }

        // 出力:
        // apple: 3
        // banana: 2
        // orange: 1
    }
}
```

## 実践例: 学生管理システム

```java
import java.util.*;

class Student {
    private String id;
    private String name;
    private int score;

    public Student(String id, String name, int score) {
        this.id = id;
        this.name = name;
        this.score = score;
    }

    public String getId() { return id; }
    public String getName() { return name; }
    public int getScore() { return score; }

    @Override
    public String toString() {
        return name + " (" + id + "): " + score;
    }
}

public class StudentManager {
    private Map<String, Student> students = new HashMap<>();

    public void addStudent(Student student) {
        students.put(student.getId(), student);
    }

    public Student getStudent(String id) {
        return students.get(id);
    }

    public List<Student> getAllStudents() {
        return new ArrayList<>(students.values());
    }

    public List<Student> getTopStudents(int n) {
        List<Student> all = getAllStudents();

        // スコアで降順ソート
        all.sort((s1, s2) -> Integer.compare(s2.getScore(), s1.getScore()));

        return all.subList(0, Math.min(n, all.size()));
    }

    public Map<String, Integer> getScoreDistribution() {
        Map<String, Integer> distribution = new TreeMap<>();

        for (Student student : students.values()) {
            int score = student.getScore();
            String grade;

            if (score >= 90) grade = "A";
            else if (score >= 80) grade = "B";
            else if (score >= 70) grade = "C";
            else if (score >= 60) grade = "D";
            else grade = "F";

            distribution.put(grade, distribution.getOrDefault(grade, 0) + 1);
        }

        return distribution;
    }

    public static void main(String[] args) {
        StudentManager manager = new StudentManager();

        manager.addStudent(new Student("001", "Alice", 95));
        manager.addStudent(new Student("002", "Bob", 87));
        manager.addStudent(new Student("003", "Charlie", 92));
        manager.addStudent(new Student("004", "David", 78));

        // トップ3を表示
        System.out.println("Top 3 Students:");
        for (Student student : manager.getTopStudents(3)) {
            System.out.println(student);
        }

        // 成績分布
        System.out.println("\nGrade Distribution:");
        for (Map.Entry<String, Integer> entry : manager.getScoreDistribution().entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
    }
}
```

## まとめ

### List
- **ArrayList**: 一般的な用途、高速なランダムアクセス
- **LinkedList**: 頻繁な挿入・削除

### Set
- **HashSet**: 重複排除、順序不要
- **TreeSet**: ソートされたセット
- **LinkedHashSet**: 挿入順を保持

### Map
- **HashMap**: 一般的なキー・値のペア
- **TreeMap**: ソートされたマップ
- **LinkedHashMap**: 挿入順を保持

### Queue
- **LinkedList**: 基本的なキュー
- **PriorityQueue**: 優先度付きキュー

### ユーティリティ
- **Collections**: ソート、検索、最大・最小値など
- **Arrays**: 配列とリストの変換

次のセクションでは、ジェネリクスについて学びます。
