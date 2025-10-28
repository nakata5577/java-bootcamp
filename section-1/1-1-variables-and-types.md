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
```
byte → short → int → long → float → double
       char  →
```

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
String str3 = "" + num;  // 簡易的な方法

// String → プリミティブ
String str = "456";
int parsed = Integer.parseInt(str);
double d = Double.parseDouble("3.14");
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

## まとめ

- Javaは静的型付け言語
- **プリミティブ型**: 8種類（`int`, `double`, `boolean`など）
- **参照型**: クラス、配列、インターフェース
- **String**: イミュータブルな文字列、比較は `.equals()` を使用
- **ラッパークラス**: プリミティブ型の参照型版、オートボクシング/アンボクシング
- **final**: 定数の宣言
- **var**: ローカル変数の型推論（Java 10以降）

次のセクションでは、演算子について学びます。
