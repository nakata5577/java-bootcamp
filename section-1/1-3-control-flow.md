# 1-3. 制御フロー

プログラムの実行フローを制御する構文について学びます。

## 条件分岐

### if 文

```java
int age = 20;

if (age >= 18) {
    System.out.println("成人です");
}
```

### if-else 文

```java
int score = 75;

if (score >= 60) {
    System.out.println("合格");
} else {
    System.out.println("不合格");
}
```

### if-else if-else 文

```java
int score = 85;

if (score >= 90) {
    System.out.println("A");
} else if (score >= 80) {
    System.out.println("B");
} else if (score >= 70) {
    System.out.println("C");
} else if (score >= 60) {
    System.out.println("D");
} else {
    System.out.println("F");
}
```

### ネストしたif文

```java
int age = 25;
boolean hasLicense = true;

if (age >= 18) {
    if (hasLicense) {
        System.out.println("運転できます");
    } else {
        System.out.println("免許を取得してください");
    }
} else {
    System.out.println("年齢が足りません");
}
```

**注意**: Javaでは条件式は必ず `boolean` 型である必要があります。

```java
int x = 5;
if (x) {  // コンパイルエラー！
    // ...
}

// 正しい書き方
if (x != 0) {
    // ...
}
```

## switch 文

複数の値に基づいて分岐する場合に使用します。

### 基本構文

```java
int dayOfWeek = 3;

switch (dayOfWeek) {
    case 1:
        System.out.println("月曜日");
        break;
    case 2:
        System.out.println("火曜日");
        break;
    case 3:
        System.out.println("水曜日");
        break;
    default:
        System.out.println("その他");
        break;
}
```

### break の重要性

`break` を忘れると、次の `case` に処理が流れます（フォールスルー）。

```java
int month = 3;

switch (month) {
    case 12:
    case 1:
    case 2:
        System.out.println("冬");
        break;
    case 3:
    case 4:
    case 5:
        System.out.println("春");
        break;
    case 6:
    case 7:
    case 8:
        System.out.println("夏");
        break;
    case 9:
    case 10:
    case 11:
        System.out.println("秋");
        break;
    default:
        System.out.println("不正な月");
}
```

### switch文で使える型

- `byte`, `short`, `int`, `char`
- `String`（Java 7以降）
- 列挙型（`enum`）
- ラッパークラス（`Byte`, `Short`, `Integer`, `Character`）

```java
String fruit = "apple";

switch (fruit) {
    case "apple":
        System.out.println("りんご");
        break;
    case "banana":
        System.out.println("バナナ");
        break;
    case "orange":
        System.out.println("オレンジ");
        break;
    default:
        System.out.println("不明な果物");
}
```

### Switch式（Java 14以降）

より簡潔に記述できる新しい構文です。

```java
int dayOfWeek = 3;

String dayName = switch (dayOfWeek) {
    case 1 -> "月曜日";
    case 2 -> "火曜日";
    case 3 -> "水曜日";
    case 4 -> "木曜日";
    case 5 -> "金曜日";
    case 6 -> "土曜日";
    case 7 -> "日曜日";
    default -> "不正な値";
};

System.out.println(dayName);  // "水曜日"
```

**利点:**
- `break` 不要
- 値を返せる
- フォールスルーが起きない
- すべてのケースをカバーする必要がある（コンパイラがチェック）

複数行のブロックも可能:

```java
int month = 3;

String season = switch (month) {
    case 12, 1, 2 -> "冬";
    case 3, 4, 5 -> "春";
    case 6, 7, 8 -> "夏";
    case 9, 10, 11 -> "秋";
    default -> {
        System.out.println("不正な月です");
        yield "不明";  // yieldで値を返す
    }
};
```

## 繰り返し（ループ）

### for ループ

```java
// 基本形
for (int i = 0; i < 5; i++) {
    System.out.println(i);  // 0, 1, 2, 3, 4
}

// カウントダウン
for (int i = 5; i > 0; i--) {
    System.out.println(i);  // 5, 4, 3, 2, 1
}

// 複数の変数
for (int i = 0, j = 10; i < j; i++, j--) {
    System.out.println("i=" + i + ", j=" + j);
}
```

### 拡張for文（for-each）

配列やコレクションの要素を順に処理する際に便利です。

```java
int[] numbers = {1, 2, 3, 4, 5};

for (int num : numbers) {
    System.out.println(num);
}

// String配列
String[] fruits = {"apple", "banana", "orange"};
for (String fruit : fruits) {
    System.out.println(fruit);
}
```

**注意**: インデックスは取得できません。必要な場合は通常の `for` ループを使用。

### while ループ

条件が真の間、繰り返します。

```java
int count = 0;

while (count < 5) {
    System.out.println(count);
    count++;
}

// 無限ループ（注意して使用）
while (true) {
    // 何らかの条件でbreak
    if (someCondition) {
        break;
    }
}
```

### do-while ループ

最低1回は実行されるループです。

```java
int count = 0;

do {
    System.out.println(count);
    count++;
} while (count < 5);

// 必ず1回は実行される
int x = 10;
do {
    System.out.println("実行される");  // xが10でも実行される
} while (x < 5);
```

**使い分け**: ユーザー入力のバリデーションなどで使用

```java
Scanner scanner = new Scanner(System.in);
int input;

do {
    System.out.print("1から10の数値を入力してください: ");
    input = scanner.nextInt();
} while (input < 1 || input > 10);
```

## ループ制御文

### break

ループを即座に終了します。

```java
for (int i = 0; i < 10; i++) {
    if (i == 5) {
        break;  // i=5でループを抜ける
    }
    System.out.println(i);  // 0, 1, 2, 3, 4
}

// switch文内でも使用
switch (value) {
    case 1:
        // ...
        break;  // switch文を抜ける
}
```

### continue

現在の繰り返しをスキップし、次の繰り返しに進みます。

```java
for (int i = 0; i < 10; i++) {
    if (i % 2 == 0) {
        continue;  // 偶数はスキップ
    }
    System.out.println(i);  // 1, 3, 5, 7, 9
}
```

### ラベル付きbreak/continue

ネストしたループから抜ける際に使用します。

```java
outer:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (i == 1 && j == 1) {
            break outer;  // 外側のループを抜ける
        }
        System.out.println("i=" + i + ", j=" + j);
    }
}

// continue も同様
outer:
for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 3; j++) {
        if (j == 1) {
            continue outer;  // 外側のループの次の繰り返しへ
        }
        System.out.println("i=" + i + ", j=" + j);
    }
}
```

**注意**: ラベルの使用は可読性を下げる可能性があるため、慎重に使用

## 実践例

### 1. FizzBuzz

```java
for (int i = 1; i <= 100; i++) {
    if (i % 15 == 0) {
        System.out.println("FizzBuzz");
    } else if (i % 3 == 0) {
        System.out.println("Fizz");
    } else if (i % 5 == 0) {
        System.out.println("Buzz");
    } else {
        System.out.println(i);
    }
}
```

### 2. 九九の表

```java
for (int i = 1; i <= 9; i++) {
    for (int j = 1; j <= 9; j++) {
        System.out.print(i * j + "\t");
    }
    System.out.println();
}
```

### 3. 素数判定

```java
int number = 29;
boolean isPrime = true;

if (number <= 1) {
    isPrime = false;
} else {
    for (int i = 2; i <= Math.sqrt(number); i++) {
        if (number % i == 0) {
            isPrime = false;
            break;
        }
    }
}

System.out.println(number + " is " + (isPrime ? "prime" : "not prime"));
```

## まとめ

### 条件分岐
- **if-else**: 基本的な条件分岐
- **switch**: 複数の値に基づく分岐
- **switch式**（Java 14以降）: 値を返すswitch

### 繰り返し
- **for**: 回数が決まっている場合
- **for-each**: 配列/コレクションの全要素を処理
- **while**: 条件が真の間繰り返す
- **do-while**: 最低1回は実行

### ループ制御
- **break**: ループを終了
- **continue**: 現在の繰り返しをスキップ
- **ラベル**: ネストしたループの制御

次のセクションでは、配列について学びます。
