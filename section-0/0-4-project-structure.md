# 0-4. プロジェクトの基本構造

実際の開発では、複数のクラスやファイルを整理して管理する必要があります。このセクションでは、Javaプロジェクトの構造とパッケージの概念を学びます。

## パッケージとは

**パッケージ（Package）** は、関連するクラスやインターフェースをグループ化する仕組みです。

### パッケージの役割

1. **名前空間の提供**
   - クラス名の衝突を防ぐ
   - 例: `com.company.util.List` と `java.util.List` は別物

2. **コードの整理**
   - 機能ごとにクラスを分類
   - プロジェクトの構造を明確化

3. **アクセス制御**
   - パッケージレベルでのアクセス制限が可能

## パッケージの宣言

パッケージを使用するには、Javaファイルの先頭で宣言します。

```java
package com.example.myapp;

public class MyClass {
    // クラスの内容
}
```

### パッケージの命名規則

- 小文字のみを使用
- ドット（`.`）で階層構造を表現
- 慣習: 逆ドメイン名を使用（例: `com.google`, `org.apache`）

**例:**
```
com.example.project         // トップレベル
com.example.project.model   // モデルクラス
com.example.project.service // ビジネスロジック
com.example.project.util    // ユーティリティ
```

## ディレクトリ構造

パッケージはディレクトリ構造に対応します。

### 基本構造

```
myproject/
├── src/
│   └── com/
│       └── example/
│           └── myapp/
│               ├── Main.java
│               ├── model/
│               │   └── User.java
│               └── util/
│                   └── Helper.java
└── bin/  (またはout/) - コンパイル後の.classファイル
```

### ファイルパスとパッケージの対応

- **パッケージ**: `com.example.myapp`
- **ファイルパス**: `src/com/example/myapp/Main.java`

## パッケージの使用例

### ファイル構成

**src/com/example/myapp/Main.java**
```java
package com.example.myapp;

import com.example.myapp.model.User;
import com.example.myapp.util.Helper;

public class Main {
    public static void main(String[] args) {
        User user = new User("Alice", 30);
        Helper.printWelcome(user.getName());
    }
}
```

**src/com/example/myapp/model/User.java**
```java
package com.example.myapp.model;

public class User {
    private String name;
    private int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
```

**src/com/example/myapp/util/Helper.java**
```java
package com.example.myapp.util;

public class Helper {
    public static void printWelcome(String name) {
        System.out.println("Welcome, " + name + "!");
    }
}
```

## インポート（import）

他のパッケージのクラスを使用するには、`import` 文を使います。

### 基本的なインポート

```java
import com.example.myapp.model.User;  // 特定のクラスをインポート
```

### ワイルドカードインポート

```java
import com.example.myapp.model.*;  // パッケージ内のすべてのクラスをインポート
```

**注意**: サブパッケージは含まれません
```java
import com.example.myapp.*;  // com.example.myapp.model.Userは含まれない
```

### 完全修飾名の使用

インポートせずに、完全修飾名で直接参照することもできます。

```java
public class Main {
    public static void main(String[] args) {
        com.example.myapp.model.User user = new com.example.myapp.model.User("Alice", 30);
    }
}
```

### 標準ライブラリのインポート

Javaの標準ライブラリもインポートが必要です（`java.lang`パッケージを除く）。

```java
import java.util.List;
import java.util.ArrayList;
import java.io.File;
```

**例外**: `java.lang`パッケージは自動的にインポートされる
```java
// 以下はインポート不要
String str = "Hello";
System.out.println(str);
Math.sqrt(16);
```

## コンパイルと実行（パッケージ使用時）

### ターミナルでのコンパイル

プロジェクトルートから:

```bash
# コンパイル（すべてのJavaファイル）
javac -d bin src/com/example/myapp/*.java src/com/example/myapp/model/*.java src/com/example/myapp/util/*.java

# または
javac -d bin src/com/example/myapp/**/*.java  # シェルがサポートしている場合
```

- `-d bin`: コンパイル後のクラスファイルを`bin`ディレクトリに出力
- `src/com/example/myapp/**/*.java`: すべてのJavaファイルを指定

### 実行

```bash
java -cp bin com.example.myapp.Main
```

- `-cp bin`: クラスパスを`bin`ディレクトリに設定
- `com.example.myapp.Main`: 実行するクラスの完全修飾名

## IDEでのプロジェクト構造

### IntelliJ IDEA

IntelliJ IDEAでは、プロジェクト構造が自動的に管理されます。

```
MyProject/
├── .idea/           # IDE設定ファイル
├── src/
│   └── com/
│       └── example/
│           └── myapp/
│               └── Main.java
├── out/             # コンパイル済みクラスファイル
└── MyProject.iml    # プロジェクト設定
```

### Maven/Gradleプロジェクトの標準構造

実務では、ビルドツールを使用した以下の構造が一般的です（詳細はSection 5）。

```
myproject/
├── src/
│   ├── main/
│   │   ├── java/          # アプリケーションコード
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── myapp/
│   │   └── resources/     # 設定ファイルなど
│   └── test/
│       ├── java/          # テストコード
│       └── resources/
├── target/ (Mavenの場合) または build/ (Gradleの場合)
└── pom.xml (Maven) または build.gradle (Gradle)
```

## アクセス修飾子とパッケージ

パッケージはアクセス制御にも関係します。

| 修飾子 | 同じクラス | 同じパッケージ | サブクラス | すべて |
|--------|-----------|---------------|-----------|--------|
| `private` | ○ | × | × | × |
| (デフォルト) | ○ | ○ | × | × |
| `protected` | ○ | ○ | ○ | × |
| `public` | ○ | ○ | ○ | ○ |

**デフォルト（修飾子なし）** は、同じパッケージ内からのみアクセス可能です。

```java
package com.example.myapp;

class DefaultAccessClass {  // パッケージプライベート
    void defaultMethod() {
        System.out.println("Same package only");
    }
}
```

## まとめ

- **パッケージ**: クラスをグループ化し、名前空間を提供
- **ディレクトリ構造**: パッケージ名に対応したフォルダ構成
- **import文**: 他のパッケージのクラスを利用
- **完全修飾名**: `com.example.myapp.Main` の形式
- **コンパイル**: `-d` オプションで出力先を指定
- **実行**: `-cp` でクラスパスを指定し、完全修飾名で実行

実務では、MavenやGradleなどのビルドツールを使うことで、これらの管理が自動化されます（Section 5で詳しく学びます）。

次のセクションからは、Javaの基本文法を学んでいきます。
