# 0-3. Hello, World!

最初のJavaプログラムを作成し、コンパイルと実行の流れを理解しましょう。

## Javaプログラムの基本構造

以下のコードを `HelloWorld.java` という名前で保存します。

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

### コードの解説

1. **`public class HelloWorld`**
   - クラスの定義。Javaではすべてのコードはクラス内に記述される
   - `public`: このクラスは他のクラスからアクセス可能
   - クラス名とファイル名は一致させる必要がある（`HelloWorld.java`）
   - **重要**: Javaでは1つのファイルに1つのpublicクラスのみ定義可能

2. **`public static void main(String[] args)`**
   - プログラムのエントリーポイント（開始地点）
   - `public`: どこからでもアクセス可能（JVMから呼び出されるため必須）
   - `static`: クラスのインスタンス化なしで呼び出し可能
   - `void`: 戻り値なし
   - `main`: メソッド名（この名前は固定、変更不可）
   - `String[] args`: コマンドライン引数を受け取る配列

3. **`System.out.println("Hello, World!");`**
   - 標準出力に文字列を出力
   - `System`: Javaの標準クラス
   - `out`: 標準出力ストリーム（Systemクラスのstaticフィールド）
   - `println`: 改行付きで文字列を出力するメソッド
   - `print`: 改行なしで出力する場合はこちらを使用

## コンパイルと実行

### ターミナル/コマンドプロンプトを使う場合

#### 1. ファイルの作成

テキストエディタで `HelloWorld.java` を作成し、上記のコードを記述します。

#### 2. コンパイル

```bash
javac HelloWorld.java
```

- `javac`: Javaコンパイラ
- 成功すると `HelloWorld.class` ファイルが生成される
- エラーがある場合はエラーメッセージが表示される

#### 3. 実行

```bash
java HelloWorld
```

- `java`: Java仮想マシンを起動してプログラムを実行
- **注意**: 拡張子 `.class` は不要

#### 4. 出力

```text
Hello, World!
```

### IDEを使う場合（IntelliJ IDEA）

1. **ファイルの作成**
   - プロジェクト内で右クリック > `New > Java Class`
   - クラス名を `HelloWorld` として作成

2. **コードの記述**
   - 上記のコードを入力

3. **実行**
   - エディタ上で右クリック > `Run 'HelloWorld.main()'`
   - または、`Ctrl/Cmd + Shift + F10`

4. **出力確認**
   - 下部の実行ウィンドウに `Hello, World!` が表示される

### IDEを使う場合（VS Code）

1. **ファイルの作成**
   - エクスプローラーで `HelloWorld.java` を作成

2. **コードの記述**
   - 上記のコードを入力

3. **実行**
   - エディタ右上の `Run` ボタンをクリック
   - または、`F5` でデバッグ実行

4. **出力確認**
   - ターミナルに `Hello, World!` が表示される

## コマンドライン引数の利用

プログラムに引数を渡すこともできます。

```java
public class HelloArgs {
    public static void main(String[] args) {
        if (args.length > 0) {
            System.out.println("Hello, " + args[0] + "!");
        } else {
            System.out.println("Hello, World!");
        }
    }
}
```

### 実行例

```bash
javac HelloArgs.java
java HelloArgs Java
```

出力:
```text
Hello, Java!
```

引数なしの場合:
```bash
java HelloArgs
```

出力:
```text
Hello, World!
```

## よくあるエラーと対処法

### 1. `javac: command not found`

**原因**: JDKがインストールされていないか、パスが通っていない

**対処法**:
- JDKのインストールを確認（`java -version`）
- 環境変数 `PATH` にJDKのbinディレクトリが含まれているか確認

### 2. `error: class HelloWorld is public, should be declared in a file named HelloWorld.java`

**原因**: ファイル名とクラス名が一致していない

**対処法**:
- ファイル名を `HelloWorld.java` に変更
- または、クラス名をファイル名に合わせる

### 3. `Error: Could not find or load main class HelloWorld`

**原因**:
- `.class` ファイルが生成されていない
- 実行時に `.class` 拡張子を付けている
- カレントディレクトリが間違っている

**対処法**:
- `javac HelloWorld.java` でコンパイルを確認
- `java HelloWorld`（拡張子なし）で実行
- `.class` ファイルがあるディレクトリで実行

## Java 11以降: シングルファイルでの実行

Java 11以降では、コンパイルと実行を同時に行うことができます。

```bash
java HelloWorld.java
```

- 小規模なスクリプトやテストに便利
- `.class` ファイルは生成されない（メモリ上で実行）
- 複数ファイルのプロジェクトには不向き
- 実行が高速で、学習や簡単なテストに最適

**従来の方法との比較:**
```bash
# 従来の方法
javac HelloWorld.java  # コンパイル
java HelloWorld        # 実行

# Java 11以降の簡易実行
java HelloWorld.java   # コンパイル + 実行
```

## まとめ

- Javaプログラムは `.java` ファイルに記述
- `javac` でコンパイルし、`.class` ファイルを生成
- `java` で `.class` ファイルを実行
- すべてのプログラムは `main` メソッドから開始
- ファイル名とクラス名は一致させる
- Java 11以降では `java FileName.java` で直接実行可能

!!! success "おめでとうございます！"
    最初のJavaプログラムが動作しました。これでJavaプログラミングの第一歩を踏み出しました。

次のセクションでは、プロジェクトの構造とパッケージの概念について学びます。
