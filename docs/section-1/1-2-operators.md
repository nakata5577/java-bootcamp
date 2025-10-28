# 1-2. 演算子

Javaの演算子は、他の多くのプログラミング言語と似ていますが、いくつかの注意点があります。

## 算術演算子

基本的な数値計算を行う演算子です。

| 演算子 | 意味 | 例 | 結果 |
|--------|------|-----|------|
| `+` | 加算 | `5 + 3` | `8` |
| `-` | 減算 | `5 - 3` | `2` |
| `*` | 乗算 | `5 * 3` | `15` |
| `/` | 除算 | `5 / 2` | `2`（整数除算） |
| `%` | 剰余（モジュロ） | `5 % 2` | `1` |

### 整数除算の注意点

```java
int result = 5 / 2;  // 2（小数点以下切り捨て）
double d = 5 / 2;    // 2.0（整数除算後にdoubleに変換）

// 浮動小数点除算にするには
double correct = 5.0 / 2;  // 2.5
double correct2 = (double) 5 / 2;  // 2.5
```

### インクリメント / デクリメント

```java
int x = 5;

// 後置インクリメント（値を使った後に+1）
int y = x++;  // y = 5, x = 6

// 前置インクリメント（+1してから値を使う）
int z = ++x;  // z = 7, x = 7

// デクリメントも同様
int a = x--;  // a = 7, x = 6
int b = --x;  // b = 5, x = 5
```

## 代入演算子

| 演算子 | 例 | 同等の式 |
|--------|-----|---------|
| `=` | `x = 5` | - |
| `+=` | `x += 3` | `x = x + 3` |
| `-=` | `x -= 3` | `x = x - 3` |
| `*=` | `x *= 3` | `x = x * 3` |
| `/=` | `x /= 3` | `x = x / 3` |
| `%=` | `x %= 3` | `x = x % 3` |

```java
int count = 10;
count += 5;  // count = 15
count *= 2;  // count = 30
count -= 10; // count = 20
```

## 比較演算子（関係演算子)

真偽値（`boolean`）を返します。

| 演算子 | 意味 | 例 | 結果 |
|--------|------|-----|------|
| `==` | 等しい | `5 == 5` | `true` |
| `!=` | 等しくない | `5 != 3` | `true` |
| `>` | より大きい | `5 > 3` | `true` |
| `<` | より小さい | `5 < 3` | `false` |
| `>=` | 以上 | `5 >= 5` | `true` |
| `<=` | 以下 | `5 <= 3` | `false` |

### オブジェクトの比較

**重要**: 参照型の比較には注意が必要です。

```java
String s1 = "Hello";
String s2 = "Hello";
String s3 = new String("Hello");

// == は参照（メモリアドレス）を比較
System.out.println(s1 == s2);  // true（文字列プールの同じオブジェクト）
System.out.println(s1 == s3);  // false（異なるオブジェクト）

// equals() は内容を比較
System.out.println(s1.equals(s3));  // true

// 数値のラッパークラスも同様
Integer i1 = 100;
Integer i2 = 100;
Integer i3 = Integer.valueOf(100);

System.out.println(i1 == i2);  // true（キャッシュされたオブジェクト: -128〜127）
System.out.println(i1 == i3);  // true（キャッシュが使用される）
System.out.println(i1.equals(i3));  // true

// 大きな数値の場合
Integer i4 = 1000;
Integer i5 = 1000;
System.out.println(i4 == i5);  // false（キャッシュ範囲外）
System.out.println(i4.equals(i5));  // true
```

**ベストプラクティス**: オブジェクトの比較には `.equals()` を使用

## 論理演算子

真偽値を組み合わせる演算子です。

| 演算子 | 意味 | 例 | 結果 |
|--------|------|-----|------|
| `&&` | AND（論理積） | `true && false` | `false` |
| `\|\|` | OR（論理和） | `true \|\| false` | `true` |
| `!` | NOT（論理否定） | `!true` | `false` |

### 短絡評価（Short-circuit Evaluation）

`&&` と `||` は短絡評価を行います。

```java
int x = 5;

// && の短絡評価
if (x > 0 && x < 10) {  // 左がtrueなので右も評価
    System.out.println("xは1から9の範囲");
}

// 短絡評価を利用したnullチェック
String str = null;
if (str != null && str.length() > 0) {  // strがnullなので右は評価されない
    System.out.println(str);
}

// || の短絡評価
if (x < 0 || x > 100) {  // 左がfalseなので右を評価
    System.out.println("範囲外");
}
```

**メリット**: `NullPointerException` の回避

### ビット演算子（参考）

通常はあまり使いませんが、低レベルな操作では利用されます。

| 演算子 | 意味 |
|--------|------|
| `&` | ビットAND |
| `\|` | ビットOR |
| `^` | ビットXOR |
| `~` | ビット反転 |
| `<<` | 左シフト |
| `>>` | 右シフト（符号拡張） |
| `>>>` | 右シフト（ゼロ埋め） |

```java
int a = 5;   // 0101
int b = 3;   // 0011

int and = a & b;  // 0001 = 1
int or = a | b;   // 0111 = 7
int xor = a ^ b;  // 0110 = 6
```

## 三項演算子（条件演算子）

条件に基づいて値を選択する演算子です。

### 構文

```java
条件式 ? 真の場合の値 : 偽の場合の値
```

### 例

```java
int age = 20;
String status = (age >= 18) ? "Adult" : "Minor";
System.out.println(status);  // "Adult"

// if-else文と同等
String status2;
if (age >= 18) {
    status2 = "Adult";
} else {
    status2 = "Minor";
}

// ネストも可能（可読性に注意）
int score = 85;
String grade = (score >= 90) ? "A" : (score >= 80) ? "B" : "C";
```

**使い分け**: 単純な条件分岐には三項演算子、複雑な処理には `if-else` を使用

## instanceof 演算子

オブジェクトが特定のクラスのインスタンスかどうかを判定します。

```java
String str = "Hello";

if (str instanceof String) {
    System.out.println("strはString型です");
}

Object obj = "Test";
if (obj instanceof String) {
    String s = (String) obj;  // 安全にキャスト
    System.out.println(s.toUpperCase());
}
```

**Java 16以降**: パターンマッチング

```java
Object obj = "Hello";

if (obj instanceof String s) {  // sに自動的にキャスト
    System.out.println(s.toUpperCase());
}
```

## 演算子の優先順位

演算子には優先順位があります。優先順位の高い順に実行されます。

| 優先度 | 演算子 |
|--------|--------|
| 1 | `()` 括弧 |
| 2 | `!`, `++`, `--` 単項演算子 |
| 3 | `*`, `/`, `%` 乗除算、剰余 |
| 4 | `+`, `-` 加減算 |
| 5 | `<`, `<=`, `>`, `>=` 比較 |
| 6 | `==`, `!=` 等価性 |
| 7 | `&&` 論理AND |
| 8 | `\|\|` 論理OR |
| 9 | `? :` 三項演算子 |
| 10 | `=`, `+=`, `-=` など 代入 |

### 例

```java
int result = 2 + 3 * 4;  // 14（乗算が先）
int result2 = (2 + 3) * 4;  // 20（括弧が優先）

boolean flag = 5 > 3 && 10 < 20;  // true（比較が先、その後AND）
```

**ベストプラクティス**: 可読性のため、複雑な式には括弧を使用

## Stringの連結演算子

`+` 演算子は文字列の連結にも使われます。

```java
String firstName = "Alice";
String lastName = "Smith";
String fullName = firstName + " " + lastName;  // "Alice Smith"

// 数値との連結
int age = 30;
String message = "Age: " + age;  // "Age: 30"

// 注意: 連結順序
System.out.println("Result: " + 1 + 2);  // "Result: 12"（文字列連結）
System.out.println("Result: " + (1 + 2));  // "Result: 3"（算術演算）
```

**パフォーマンス**: 大量の文字列連結には `StringBuilder` を使用

```java
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append("Number: ").append(i).append("\n");
}
String result = sb.toString();
```

## まとめ

- **算術演算子**: `+`, `-`, `*`, `/`, `%`
  - 整数除算に注意
- **比較演算子**: `==`, `!=`, `>`, `<`, `>=`, `<=`
  - オブジェクトの比較には `.equals()` を使用
- **論理演算子**: `&&`, `||`, `!`
  - 短絡評価を活用
- **三項演算子**: `条件 ? 真 : 偽`
- **instanceof**: オブジェクトの型チェック
- 演算子の優先順位を理解し、必要に応じて括弧を使用

次のセクションでは、制御フロー（条件分岐と繰り返し）について学びます。
