# 1-1. 変数とデータ型

Javaは**静的型付け言語**であり、変数の型はコンパイル時に決定されます。データ型は**プリミティブ型**と**参照型**の2つに大別されます。

## 変数の宣言と初期化

### 基本構文

```java
// 宣言
int age;

// 宣言と初期化
int age = 30;

// 複数の変数を同時に宣言
int x = 10, y = 20, z = 30;
```

### 変数の命名規則

- **キャメルケース**: 小文字で始まり、単語の区切りで大文字（例: `firstName`, `totalAmount`）
- 予約語は使用不可（`int`, `class`, `public`など）
- 数字で始めることは不可（`1stPlace` → NG、`firstPlace` → OK）
- `$` と `_` は使用可能だが、通常は避ける

## プリミティブ型（Primitive Types）

Javaには8つのプリミティブ型があります。

| 型 | サイズ | 範囲/説明 | デフォルト値 |
|------|--------|-----------|-------------|
| `byte` | 8bit | -128 ~ 127 | 0 |
| `short` | 16bit | -32,768 ~ 32,767 | 0 |
| `int` | 32bit | -2^31 ~ 2^31-1 | 0 |
| `long` | 64bit | -2^63 ~ 2^63-1 | 0L |
| `float` | 32bit | IEEE 754 単精度浮動小数点 | 0.0f |
| `double` | 64bit | IEEE 754 倍精度浮動小数点 | 0.0d |
| `char` | 16bit | Unicode文字（0 ~ 65,535） | '\u0000' |
| `boolean` | - | `true` または `false` | false |

### 整数型

```java
int count = 100;
long population = 7_000_000_000L;  // アンダースコアで桁区切り（Java 7以降）
byte small = 127;
short medium = 32000;
```

**注意点:**
- `long`型リテラルには `L` を付ける（小文字の `l` は避ける、`1`と紛らわしいため）
- デフォルトの整数リテラルは `int`

### 浮動小数点型

```java
double price = 19.99;
float pi = 3.14f;  // float型には `f` を付ける
```

**注意点:**
- デフォルトの浮動小数点リテラルは `double`
- 金融計算では `double` や `float` は避け、`BigDecimal` を使用（精度の問題）

### 文字型

```java
char letter = 'A';
char unicode = '\u0041';  // Unicodeエスケープ（'A'と同じ）
```

### 真偽値型

```java
boolean isActive = true;
boolean hasError = false;
```

**注意点:**
- 他の言語と異なり、`0` や `null` は `false` とは扱われない
- 条件式では必ず `boolean` 型が必要

## 参照型（Reference Types）

プリミティブ型以外はすべて参照型です。

### 主な参照型

- **クラス**: `String`, `Integer`, `Scanner`など
- **配列**: `int[]`, `String[]`など
- **インターフェース**: `List`, `Map`など
- **列挙型（enum）**: 後述

### String（文字列）

```java
String name = "Alice";
String message = "Hello, World!";

// 文字列の連結
String greeting = "Hello, " + name;

// 複数行文字列（Java 15以降のテキストブロック）
String multiline = """
    This is a
    multi-line
    string.
    """;
```

**Stringの特徴:**
- **イミュータブル（不変）**: 一度作成すると変更不可
- 文字列リテラルは文字列プールで管理される

```java
String s1 = "Hello";
String s2 = "Hello";
System.out.println(s1 == s2);  // true（同じオブジェクトを参照）

String s3 = new String("Hello");
System.out.println(s1 == s3);  // false（異なるオブジェクト）
System.out.println(s1.equals(s3));  // true（内容が同じ）
```

**重要**: 文字列の比較は `.equals()` を使用（`==` は参照の比較）

## 型変換（キャスト）

### 暗黙的な型変換（ワイドニング）

小さい型から大きい型への変換は自動的に行われます。

```java
int i = 100;
long l = i;  // int → long（自動変換）

float f = l;  // long → float（自動変換）
```

**変換順序:**
```text
byte → short → int → long → float → double
       char  →
```

!!! note "なぜfloatはlongより大きい？"
    floatは32bitですがlongは64bitです。しかし、floatは浮動小数点表現のため、より大きな数値範囲を扱えます。そのため、long → float の変換は暗黙的に可能です。ただし、精度は失われる可能性があります。

### 明示的な型変換（ナローイング）

大きい型から小さい型への変換は明示的にキャストが必要です。

```java
double d = 9.99;
int i = (int) d;  // 9（小数点以下切り捨て）

long l = 100L;
int x = (int) l;

// オーバーフロー注意
int bigNumber = 130;
byte b = (byte) bigNumber;  // -126（オーバーフロー）
```

### 文字列との変換

```java
// プリミティブ → String
int num = 123;
String str1 = String.valueOf(num);
String str2 = Integer.toString(num);
String str3 = "" + num;  // 簡易的な方法（非推奨、可読性が低い）

// String → プリミティブ
String str = "456";
int parsed = Integer.parseInt(str);
double d = Double.parseDouble("3.14");

// 例外処理を伴う変換（実践的）
try {
    String input = "abc";
    int value = Integer.parseInt(input);
} catch (NumberFormatException e) {
    System.out.println("数値に変換できません: " + e.getMessage());
}
```

## ラッパークラス（Wrapper Classes）

各プリミティブ型には対応する参照型（ラッパークラス）があります。

| プリミティブ型 | ラッパークラス |
|---------------|---------------|
| `byte` | `Byte` |
| `short` | `Short` |
| `int` | `Integer` |
| `long` | `Long` |
| `float` | `Float` |
| `double` | `Double` |
| `char` | `Character` |
| `boolean` | `Boolean` |

### オートボクシング / アンボクシング

Java 5以降、プリミティブ型とラッパークラスの相互変換が自動的に行われます。

```java
// オートボクシング（プリミティブ → ラッパー）
Integer obj = 100;  // 自動的に Integer.valueOf(100) が呼ばれる

// アンボクシング（ラッパー → プリミティブ）
int num = obj;  // 自動的に obj.intValue() が呼ばれる

// コレクションではラッパークラスを使用
List<Integer> numbers = new ArrayList<>();
numbers.add(10);  // オートボクシング
int first = numbers.get(0);  // アンボクシング
```

**注意点:**
- `null` のアンボクシングは `NullPointerException` を引き起こす

```java
Integer obj = null;
int num = obj;  // NullPointerException
```

## 定数（final）

`final` キーワードで定数を宣言します。

```java
final int MAX_USERS = 100;
final double PI = 3.14159;

// クラスレベルの定数
public static final String APP_NAME = "MyApp";
```

**命名規則**: 定数は大文字とアンダースコアで記述（`MAX_VALUE`, `DEFAULT_SIZE`）

## var（ローカル変数型推論）

Java 10以降、`var` キーワードで型推論が可能です。

```java
var name = "Alice";  // String型と推論
var count = 10;      // int型と推論
var price = 19.99;   // double型と推論

// コレクションでも便利
var list = new ArrayList<String>();
var map = new HashMap<String, Integer>();
```

**制約:**
- ローカル変数のみ（フィールドやメソッド引数には使用不可）
- 初期化が必須
- `null` での初期化は不可

```java
var x;  // エラー: 初期化が必要
var y = null;  // エラー: 型が推論できない
```

## 練習問題

### 問題1: 温度変換プログラム
摂氏から華氏に変換するプログラムを作成してください。

```java
public class TemperatureConverter {
    public static void main(String[] args) {
        double celsius = 25.0;
        // ここに変換処理を実装
        // 公式: F = C × 9/5 + 32
    }
}
```

<details>
<summary>解答例</summary>

```java
public class TemperatureConverter {
    public static void main(String[] args) {
        double celsius = 25.0;
        double fahrenheit = celsius * 9.0 / 5.0 + 32.0;
        System.out.println(celsius + "°C = " + fahrenheit + "°F");
        // 出力: 25.0°C = 77.0°F
    }
}
```
</details>

### 問題2: 円の面積と円周
半径を入力として、円の面積と円周を計算するプログラムを作成してください。

```java
public class Circle {
    public static void main(String[] args) {
        double radius = 5.0;
        // ここに計算処理を実装
        // 面積: π × r²
        // 円周: 2 × π × r
    }
}
```

<details>
<summary>解答例</summary>

```java
public class Circle {
    public static void main(String[] args) {
        double radius = 5.0;
        double area = Math.PI * radius * radius;
        double circumference = 2 * Math.PI * radius;

        System.out.println("半径: " + radius);
        System.out.println("面積: " + area);
        System.out.println("円周: " + circumference);
    }
}
```
</details>

### 問題3: 型変換
`int`, `double`, `String`の相互変換を練習してください。

<details>
<summary>解答例</summary>

```java
public class TypeConversion {
    public static void main(String[] args) {
        // String → int → double → String
        String str = "123";
        int num = Integer.parseInt(str);
        double d = (double) num;
        String result = String.valueOf(d);

        System.out.println("String: " + str);
        System.out.println("int: " + num);
        System.out.println("double: " + d);
        System.out.println("String: " + result);
    }
}
```
</details>

## よくある落とし穴

### 1. 整数除算の罠
```java
int a = 5;
int b = 2;
double result = a / b;  // 2.0（期待値: 2.5）

// 正しい方法
double result = (double) a / b;  // 2.5
```

### 2. 浮動小数点の誤差
```java
double a = 0.1 + 0.2;
System.out.println(a);  // 0.30000000000000004

// 金融計算にはBigDecimalを使用
```

### 3. 文字列比較の間違い
```java
String s1 = new String("Hello");
String s2 = new String("Hello");
System.out.println(s1 == s2);  // false（間違い）
System.out.println(s1.equals(s2));  // true（正しい）
```

## まとめ

- Javaは静的型付け言語
- **プリミティブ型**: 8種類（`int`, `double`, `boolean`など）
- **参照型**: クラス、配列、インターフェース
- **String**: イミュータブルな文字列、比較は `.equals()` を使用
- **ラッパークラス**: プリミティブ型の参照型版、オートボクシング/アンボクシング
- **final**: 定数の宣言
- **var**: ローカル変数の型推論（Java 10以降）

次のセクションでは、演算子について学びます。
