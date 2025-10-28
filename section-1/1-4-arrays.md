# 1-4. 配列

配列は、同じ型の複数の値を格納できるデータ構造です。Javaの配列は固定長であり、一度作成するとサイズは変更できません。

## 配列の宣言と初期化

### 宣言方法

```java
// 型[] 変数名 (推奨)
int[] numbers;
String[] names;

// 型 変数名[] (C言語スタイル、非推奨)
int numbers2[];
```

### 初期化

```java
// 1. サイズを指定して作成
int[] numbers = new int[5];  // 要素数5の配列（デフォルト値: 0）

// 2. 値を指定して初期化
int[] numbers = {1, 2, 3, 4, 5};

// 3. new演算子と値の指定を組み合わせる
int[] numbers = new int[]{1, 2, 3, 4, 5};

// 4. 後から初期化
int[] numbers;
numbers = new int[]{1, 2, 3, 4, 5};
```

### デフォルト値

配列の要素は、明示的に初期化しない場合、型に応じたデフォルト値で初期化されます。

| 型 | デフォルト値 |
|-----|-------------|
| `byte`, `short`, `int`, `long` | `0` |
| `float`, `double` | `0.0` |
| `boolean` | `false` |
| `char` | `'\u0000'` |
| 参照型 | `null` |

```java
int[] numbers = new int[3];
// numbers[0] = 0, numbers[1] = 0, numbers[2] = 0

String[] names = new String[3];
// names[0] = null, names[1] = null, names[2] = null
```

## 配列へのアクセス

### 要素の読み書き

```java
int[] numbers = {10, 20, 30, 40, 50};

// 読み取り
int first = numbers[0];  // 10（インデックスは0から開始）
int second = numbers[1]; // 20

// 書き込み
numbers[2] = 35;  // 3番目の要素を35に変更

// 配列の長さ
int length = numbers.length;  // 5（プロパティであり、メソッドではない）
```

**注意**: インデックスは `0` から `length - 1` まで

### 配列の範囲外アクセス

```java
int[] numbers = {1, 2, 3};

int value = numbers[5];  // ArrayIndexOutOfBoundsException
```

## 配列の走査（ループ）

### 通常のforループ

```java
int[] numbers = {10, 20, 30, 40, 50};

for (int i = 0; i < numbers.length; i++) {
    System.out.println("Index " + i + ": " + numbers[i]);
}
```

### 拡張forループ（for-each）

```java
int[] numbers = {10, 20, 30, 40, 50};

for (int num : numbers) {
    System.out.println(num);
}

// String配列
String[] fruits = {"apple", "banana", "orange"};
for (String fruit : fruits) {
    System.out.println(fruit);
}
```

**制限**: インデックスが不要な場合に便利ですが、配列の変更はできません。

```java
int[] numbers = {1, 2, 3};

for (int num : numbers) {
    num = num * 2;  // 元の配列には影響しない
}

// 元の配列を変更するには通常のforループを使用
for (int i = 0; i < numbers.length; i++) {
    numbers[i] = numbers[i] * 2;
}
```

## 多次元配列

### 2次元配列

```java
// 宣言と初期化
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// サイズ指定
int[][] matrix2 = new int[3][3];

// アクセス
int value = matrix[0][1];  // 2（1行目、2列目）
matrix[2][2] = 10;  // 3行目、3列目を10に変更
```

### 不規則な配列（Jagged Array）

Javaでは、各行のサイズが異なる配列も作成できます。

```java
int[][] jagged = {
    {1, 2},
    {3, 4, 5, 6},
    {7}
};

// または
int[][] jagged2 = new int[3][];
jagged2[0] = new int[2];
jagged2[1] = new int[4];
jagged2[2] = new int[1];
```

### 多次元配列のループ

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// ネストしたforループ
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}

// 拡張forループ
for (int[] row : matrix) {
    for (int value : row) {
        System.out.print(value + " ");
    }
    System.out.println();
}
```

## 配列のコピー

### 参照のコピー（浅いコピー）

```java
int[] original = {1, 2, 3};
int[] reference = original;  // 同じ配列を参照

reference[0] = 10;
System.out.println(original[0]);  // 10（元の配列も変更される）
```

### 配列のクローン（深いコピー）

```java
int[] original = {1, 2, 3};
int[] copy = original.clone();  // 新しい配列を作成

copy[0] = 10;
System.out.println(original[0]);  // 1（元の配列は変更されない）
```

### System.arraycopy()

```java
int[] source = {1, 2, 3, 4, 5};
int[] dest = new int[5];

System.arraycopy(source, 0, dest, 0, source.length);
// 引数: (コピー元, コピー元の開始位置, コピー先, コピー先の開始位置, コピーする要素数)
```

### Arrays.copyOf()

```java
import java.util.Arrays;

int[] original = {1, 2, 3, 4, 5};

// 全体をコピー
int[] copy1 = Arrays.copyOf(original, original.length);

// 一部をコピー（長さ指定）
int[] copy2 = Arrays.copyOf(original, 3);  // {1, 2, 3}

// 長く指定すると、残りはデフォルト値で埋められる
int[] copy3 = Arrays.copyOf(original, 7);  // {1, 2, 3, 4, 5, 0, 0}
```

## Arraysクラスのユーティリティ

`java.util.Arrays` クラスには、配列操作のための便利なメソッドが用意されています。

### ソート

```java
import java.util.Arrays;

int[] numbers = {5, 2, 8, 1, 9};
Arrays.sort(numbers);  // 昇順にソート
System.out.println(Arrays.toString(numbers));  // [1, 2, 5, 8, 9]

// 一部をソート
int[] numbers2 = {5, 2, 8, 1, 9};
Arrays.sort(numbers2, 1, 4);  // インデックス1から3までをソート
```

### 検索（二分探索）

```java
int[] numbers = {1, 3, 5, 7, 9};  // ソート済みである必要がある
int index = Arrays.binarySearch(numbers, 5);  // 2
int notFound = Arrays.binarySearch(numbers, 6);  // 負の値（見つからない）
```

### 配列の比較

```java
int[] arr1 = {1, 2, 3};
int[] arr2 = {1, 2, 3};
int[] arr3 = {1, 2, 4};

System.out.println(arr1 == arr2);  // false（参照が異なる）
System.out.println(Arrays.equals(arr1, arr2));  // true（内容が同じ）
System.out.println(Arrays.equals(arr1, arr3));  // false
```

### 配列の埋め込み

```java
int[] numbers = new int[5];
Arrays.fill(numbers, 10);  // すべての要素を10で埋める
System.out.println(Arrays.toString(numbers));  // [10, 10, 10, 10, 10]

// 一部を埋める
Arrays.fill(numbers, 1, 3, 20);  // インデックス1から2までを20で埋める
```

### 配列の文字列表現

```java
int[] numbers = {1, 2, 3, 4, 5};
System.out.println(Arrays.toString(numbers));  // [1, 2, 3, 4, 5]

// 多次元配列
int[][] matrix = {{1, 2}, {3, 4}};
System.out.println(Arrays.deepToString(matrix));  // [[1, 2], [3, 4]]
```

## 配列 vs コレクション

Javaには、より柔軟なコレクションフレームワーク（`ArrayList`, `LinkedList` など）があります。

### 配列の利点
- メモリ効率が良い
- アクセス速度が速い
- プリミティブ型を直接格納可能

### 配列の欠点
- サイズが固定
- 要素の追加・削除が難しい

**実務では**: 可変長が必要な場合は `ArrayList` などのコレクションを使用（Section 3で詳しく学びます）

```java
import java.util.ArrayList;

ArrayList<Integer> list = new ArrayList<>();
list.add(1);
list.add(2);
list.add(3);
list.remove(1);  // インデックス1を削除
System.out.println(list);  // [1, 3]
```

## 実践例

### 配列の合計と平均

```java
int[] scores = {85, 90, 78, 92, 88};
int sum = 0;

for (int score : scores) {
    sum += score;
}

double average = (double) sum / scores.length;
System.out.println("合計: " + sum);
System.out.println("平均: " + average);
```

### 最大値・最小値の検索

```java
int[] numbers = {23, 45, 12, 67, 34};
int max = numbers[0];
int min = numbers[0];

for (int num : numbers) {
    if (num > max) {
        max = num;
    }
    if (num < min) {
        min = num;
    }
}

System.out.println("最大値: " + max);
System.out.println("最小値: " + min);
```

### 配列の反転

```java
int[] numbers = {1, 2, 3, 4, 5};

for (int i = 0; i < numbers.length / 2; i++) {
    int temp = numbers[i];
    numbers[i] = numbers[numbers.length - 1 - i];
    numbers[numbers.length - 1 - i] = temp;
}

System.out.println(Arrays.toString(numbers));  // [5, 4, 3, 2, 1]
```

## まとめ

- **配列**: 固定長の同じ型の要素を格納
- **インデックス**: 0から始まる
- **length**: 配列の長さ（プロパティ）
- **多次元配列**: 配列の配列
- **Arraysクラス**: ソート、検索、比較などのユーティリティメソッド
- **制約**: サイズ固定、可変長が必要な場合はコレクション（`ArrayList`など）を使用

次のセクションからは、Javaの核心であるオブジェクト指向プログラミングについて学びます。
