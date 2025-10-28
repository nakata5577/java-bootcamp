# Section 6: 総合演習

これまで学んだJavaの知識を総動員して、実践的なコンソールアプリケーションを作成します。

## プロジェクトの概要

以下のいずれかのプロジェクトを選択するか、自分でアイデアを考えて実装してみましょう。

## プロジェクト案1: タスク管理ツール

コマンドラインから操作できるToDoリストアプリケーションです。

### 機能要件

#### 基本機能
1. **タスクの追加**: タイトル、説明、優先度を設定
2. **タスク一覧の表示**: すべてのタスクを表示
3. **タスクの完了**: タスクを完了状態にする
4. **タスクの削除**: タスクを削除
5. **データの永続化**: ファイルまたはデータベースに保存

#### 拡張機能（オプション）
- タスクの検索（タイトルで）
- タスクのフィルタリング（優先度、完了状態）
- 期限の設定とリマインダー
- カテゴリ別の整理

### 実装のヒント

#### クラス設計例

```java
// エンティティ
public class Task {
    private int id;
    private String title;
    private String description;
    private Priority priority;
    private boolean completed;
    private LocalDateTime createdAt;
    private LocalDateTime dueDate;

    // コンストラクタ、ゲッター、セッター
}

public enum Priority {
    LOW, MEDIUM, HIGH
}

// リポジトリ（データアクセス層）
public interface TaskRepository {
    void save(Task task);
    Optional<Task> findById(int id);
    List<Task> findAll();
    void update(Task task);
    void delete(int id);
}

// ファイルベースの実装
public class FileTaskRepository implements TaskRepository {
    // JSONまたはシリアライズでファイルに保存
}

// サービス（ビジネスロジック層）
public class TaskService {
    private TaskRepository repository;

    public void addTask(String title, String description, Priority priority) {
        // タスクを作成してリポジトリに保存
    }

    public List<Task> getAllTasks() {
        return repository.findAll();
    }

    public void completeTask(int id) {
        // タスクを完了状態にする
    }

    // その他のメソッド
}

// UI（コンソール）
public class TaskCLI {
    private TaskService service;
    private Scanner scanner;

    public void run() {
        while (true) {
            displayMenu();
            int choice = scanner.nextInt();
            scanner.nextLine(); // 改行を消費

            switch (choice) {
                case 1: addTask(); break;
                case 2: listTasks(); break;
                case 3: completeTask(); break;
                case 4: deleteTask(); break;
                case 0: System.exit(0);
                default: System.out.println("Invalid choice");
            }
        }
    }

    private void displayMenu() {
        System.out.println("\n=== Task Manager ===");
        System.out.println("1. Add Task");
        System.out.println("2. List Tasks");
        System.out.println("3. Complete Task");
        System.out.println("4. Delete Task");
        System.out.println("0. Exit");
        System.out.print("Choice: ");
    }

    // 各機能の実装メソッド
}

// メインクラス
public class Main {
    public static void main(String[] args) {
        TaskRepository repository = new FileTaskRepository("tasks.json");
        TaskService service = new TaskService(repository);
        TaskCLI cli = new TaskCLI(service);
        cli.run();
    }
}
```

### 学習ポイント

- オブジェクト指向設計（エンティティ、リポジトリ、サービス、UI層）
- ファイルI/Oまたはデータベース（JDBC）
- コレクションフレームワーク（`List`, `Map`）
- Optional
- 列挙型（`enum`）
- 例外処理

---

## プロジェクト案2: 簡易住所録

連絡先を管理するコンソールアプリケーションです。

### 機能要件

#### 基本機能
1. **連絡先の追加**: 名前、電話番号、メールアドレス、住所
2. **連絡先の一覧表示**: すべての連絡先を表示
3. **連絡先の検索**: 名前で検索
4. **連絡先の更新**: 情報を更新
5. **連絡先の削除**: 削除機能
6. **データの永続化**: CSVファイルまたはデータベース

#### 拡張機能（オプション）
- グループ分け（家族、友人、仕事など）
- 誕生日の記録とリマインダー
- エクスポート機能（CSV、JSON）
- インポート機能

### 実装のヒント

#### クラス設計例

```java
public class Contact {
    private int id;
    private String name;
    private String phoneNumber;
    private String email;
    private String address;
    private LocalDate birthday;
    private ContactGroup group;

    // コンストラクタ、ゲッター、セッター
}

public enum ContactGroup {
    FAMILY, FRIENDS, WORK, OTHER
}

public interface ContactRepository {
    void save(Contact contact);
    Optional<Contact> findById(int id);
    List<Contact> findAll();
    List<Contact> findByName(String name);
    void update(Contact contact);
    void delete(int id);
}

public class ContactService {
    private ContactRepository repository;

    public void addContact(Contact contact) {
        repository.save(contact);
    }

    public List<Contact> searchByName(String name) {
        return repository.findByName(name);
    }

    // その他のビジネスロジック
}
```

### 学習ポイント

- オブジェクト指向設計
- CSVファイルの読み書き
- Stream APIを使った検索・フィルタリング
- 正規表現による入力検証（メールアドレス、電話番号）
- 日付の扱い（`LocalDate`）

---

## プロジェクト案3: 在庫管理システム

商品の在庫を管理するシステムです。

### 機能要件

#### 基本機能
1. **商品の登録**: 商品名、カテゴリ、価格、在庫数
2. **商品一覧の表示**: すべての商品を表示
3. **在庫の追加**: 入荷処理
4. **在庫の減少**: 出荷処理
5. **在庫アラート**: 在庫が一定数以下の商品を警告
6. **データの永続化**

#### 拡張機能（オプション）
- 入出庫履歴の記録
- 売上レポート
- カテゴリ別集計
- 商品のバーコード管理

### 実装のヒント

```java
public class Product {
    private int id;
    private String name;
    private String category;
    private double price;
    private int quantity;

    public boolean isLowStock(int threshold) {
        return quantity < threshold;
    }
}

public class InventoryService {
    private ProductRepository repository;

    public void addStock(int productId, int quantity) {
        Optional<Product> product = repository.findById(productId);
        product.ifPresent(p -> {
            p.setQuantity(p.getQuantity() + quantity);
            repository.update(p);
        });
    }

    public void removeStock(int productId, int quantity) {
        Optional<Product> product = repository.findById(productId);
        product.ifPresent(p -> {
            if (p.getQuantity() >= quantity) {
                p.setQuantity(p.getQuantity() - quantity);
                repository.update(p);
            } else {
                throw new IllegalStateException("Insufficient stock");
            }
        });
    }

    public List<Product> getLowStockProducts(int threshold) {
        return repository.findAll().stream()
            .filter(p -> p.isLowStock(threshold))
            .collect(Collectors.toList());
    }
}
```

### 学習ポイント

- ビジネスロジックの実装（在庫管理のルール）
- トランザクション処理（データベース使用時）
- Stream APIによるデータ集計
- 例外処理（在庫不足など）

---

## 開発の進め方

### 1. 要件定義

- どんな機能が必要か明確にする
- 画面遷移（コンソールの場合はメニュー構造）を設計

### 2. 設計

#### クラス図
- エンティティ（データモデル）
- リポジトリ（データアクセス）
- サービス（ビジネスロジック）
- UI（コンソール）

#### レイヤー構造
```
┌─────────────────┐
│      UI層       │ (コンソール入出力)
├─────────────────┤
│  サービス層      │ (ビジネスロジック)
├─────────────────┤
│ リポジトリ層     │ (データアクセス)
├─────────────────┤
│   エンティティ   │ (データモデル)
└─────────────────┘
```

### 3. 実装

#### Phase 1: 基本機能
- エンティティクラスの作成
- リポジトリの実装（インメモリで可）
- サービスの基本機能
- 簡単なコンソールUI

#### Phase 2: データ永続化
- ファイルI/OまたはJDBCの実装
- データの読み込み・保存

#### Phase 3: 拡張機能
- 検索・フィルタリング
- バリデーション
- エラーハンドリングの改善

### 4. テスト

- ユニットテストの作成（JUnit）
- サービス層のテスト
- リポジトリ層のテスト

### 5. リファクタリング

- コードの整理
- 共通処理の抽出
- 命名の見直し

---

## 評価ポイント

### 必須項目
- [ ] 基本機能が動作する
- [ ] オブジェクト指向の原則を適用（カプセル化、継承、ポリモーフィズム）
- [ ] データの永続化（ファイルまたはデータベース）
- [ ] 適切な例外処理
- [ ] コードが読みやすい（命名、構造）

### 加点項目
- [ ] Stream APIの活用
- [ ] Optionalの使用
- [ ] ユニットテストの作成
- [ ] ビルドツール（Maven/Gradle）の使用
- [ ] 設計パターンの適用（DAO、Singletonなど）
- [ ] ドキュメント（README、コメント）

---

## 参考: プロジェクト構造例

```
task-manager/
├── pom.xml
├── README.md
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── taskmanager/
│   │   │               ├── Main.java
│   │   │               ├── model/
│   │   │               │   ├── Task.java
│   │   │               │   └── Priority.java
│   │   │               ├── repository/
│   │   │               │   ├── TaskRepository.java
│   │   │               │   └── FileTaskRepository.java
│   │   │               ├── service/
│   │   │               │   └── TaskService.java
│   │   │               └── ui/
│   │   │                   └── TaskCLI.java
│   │   └── resources/
│   └── test/
│       └── java/
│           └── com/
│               └── example/
│                   └── taskmanager/
│                       ├── service/
│                       │   └── TaskServiceTest.java
│                       └── repository/
│                           └── FileTaskRepositoryTest.java
└── data/
    └── tasks.json
```

---

## 開発のヒント

### デバッグのコツ

1. **段階的な実装**: 一度にすべてを作ろうとせず、小さな機能から始める
2. **こまめなテスト**: 機能を追加するたびに動作確認
3. **ログ出力**: `System.out.println()` で変数の値を確認
4. **エラーメッセージを読む**: スタックトレースには有用な情報が含まれている

### コードの品質を高めるために

1. **命名規則**:
   - クラス: `PascalCase`（例: `TaskService`）
   - メソッド/変数: `camelCase`（例: `addTask`）
   - 定数: `UPPER_SNAKE_CASE`（例: `MAX_TASKS`）

2. **DRY原則** (Don't Repeat Yourself):
   - 同じコードを繰り返さない
   - 共通処理はメソッドに抽出

3. **SOLID原則**:
   - 単一責任の原則: 1つのクラスは1つの責任のみ
   - 依存性の逆転: 抽象に依存し、具象に依存しない

### よくある問題と解決策

1. **`Scanner`のバグ**:
   ```java
   // 問題
   int choice = scanner.nextInt();
   String input = scanner.nextLine();  // 空行が読み込まれる

   // 解決策
   int choice = scanner.nextInt();
   scanner.nextLine();  // 改行を消費
   String input = scanner.nextLine();
   ```

2. **ファイルが見つからない**:
   - 相対パスと絶対パスを確認
   - ファイルの存在チェックを追加
   ```java
   File file = new File("data.txt");
   if (!file.exists()) {
       file.createNewFile();
   }
   ```

3. **NullPointerException**:
   - Optionalを使用
   - nullチェックを忘れない
   ```java
   if (task != null) {
       task.execute();
   }
   // または
   Optional.ofNullable(task).ifPresent(Task::execute);
   ```

## まとめ

この総合演習では、これまで学んだ以下の知識を実践します：

- **Section 1**: 変数、制御フロー、配列
- **Section 2**: オブジェクト指向（クラス、継承、インターフェース）
- **Section 3**: 例外処理、コレクション、ファイルI/O
- **Section 4**: ラムダ式、Stream API、Optional
- **Section 5**: ビルドツール、ユニットテスト、JDBC

実際に手を動かして、完成したアプリケーションを作り上げることで、Javaの実践的なスキルを身につけることができます。

### 完成の目安

- **初級**: 基本機能が動作し、データがメモリ上で管理できる（1-2日）
- **中級**: ファイルI/Oでデータを永続化、検索機能を実装（3-5日）
- **上級**: ユニットテスト、エラーハンドリング、ビルドツール使用（1-2週間）

## 次のステップ

総合演習を完了したら、以下のトピックに進むことをおすすめします：

1. **フレームワーク**
   - Spring Boot（Webアプリケーション）
   - Jakarta EE（エンタープライズ）

2. **データベース**
   - JPA/Hibernate（ORM）
   - Spring Data JPA

3. **Web開発**
   - RESTful API
   - Webセキュリティ

4. **並行処理**
   - マルチスレッド
   - CompletableFuture

5. **設計パターン**
   - Gang of Four デザインパターン
   - アーキテクチャパターン（MVC、MVVMなど）

頑張ってください！
