# 5-3. データベース接続 (JDBC)

JDBC（Java Database Connectivity）は、Javaからデータベースにアクセスするための標準APIです。

## JDBCとは

**JDBC**は、データベースへの接続、SQL文の実行、結果の取得を行うためのAPIです。

### JDBCの仕組み

```
Javaアプリケーション
    ↓
JDBC API
    ↓
JDBCドライバ（各データベース用）
    ↓
データベース（MySQL, PostgreSQL, Oracle など）
```

## 環境準備

### Maven依存関係（MySQL）

```xml
<dependencies>
    <!-- MySQL Connector -->
    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <version>8.0.33</version>
    </dependency>
</dependencies>
```

### 他のデータベース

```xml
<!-- PostgreSQL -->
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.6.0</version>
</dependency>

<!-- H2 Database (組み込みデータベース、テスト用に便利) -->
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <version>2.2.220</version>
</dependency>
```

## 基本的な接続

### 接続の確立

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DatabaseExample {
    public static void main(String[] args) {
        // 接続情報
        String url = "jdbc:mysql://localhost:3306/mydb";
        String user = "root";
        String password = "password";

        try (Connection conn = DriverManager.getConnection(url, user, password)) {
            System.out.println("Connected to database!");
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

### 接続URLの形式

```java
// MySQL
"jdbc:mysql://localhost:3306/mydb"

// PostgreSQL
"jdbc:postgresql://localhost:5432/mydb"

// H2 (組み込み)
"jdbc:h2:~/test"  // ファイルベース
"jdbc:h2:mem:test"  // インメモリ
```

## データの取得（SELECT）

### Statement（基本形）

```java
import java.sql.*;

public class SelectExample {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/mydb";
        String user = "root";
        String password = "password";

        String sql = "SELECT id, name, age FROM users";

        try (Connection conn = DriverManager.getConnection(url, user, password);
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery(sql)) {

            while (rs.next()) {
                int id = rs.getInt("id");
                String name = rs.getString("name");
                int age = rs.getInt("age");

                System.out.println("ID: " + id + ", Name: " + name + ", Age: " + age);
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

## PreparedStatement（推奨）

**PreparedStatement**は、SQLインジェクション対策とパフォーマンスの向上に有効です。

### SQLインジェクション対策

```java
// 悪い例（SQLインジェクションの危険）
String name = userInput;  // ユーザー入力: "Alice' OR '1'='1"
String sql = "SELECT * FROM users WHERE name = '" + name + "'";

// 良い例（PreparedStatement）
String sql = "SELECT * FROM users WHERE name = ?";
try (PreparedStatement pstmt = conn.prepareStatement(sql)) {
    pstmt.setString(1, name);  // 自動的にエスケープされる
    ResultSet rs = pstmt.executeQuery();
}
```

### データの挿入（INSERT）

```java
String sql = "INSERT INTO users (name, email, age) VALUES (?, ?, ?)";

try (Connection conn = DriverManager.getConnection(url, user, password);
     PreparedStatement pstmt = conn.prepareStatement(sql)) {

    pstmt.setString(1, "Alice");
    pstmt.setString(2, "alice@example.com");
    pstmt.setInt(3, 25);

    int rowsInserted = pstmt.executeUpdate();
    System.out.println(rowsInserted + " row(s) inserted.");

} catch (SQLException e) {
    e.printStackTrace();
}
```

### データの更新（UPDATE）

```java
String sql = "UPDATE users SET age = ? WHERE name = ?";

try (Connection conn = DriverManager.getConnection(url, user, password);
     PreparedStatement pstmt = conn.prepareStatement(sql)) {

    pstmt.setInt(1, 26);
    pstmt.setString(2, "Alice");

    int rowsUpdated = pstmt.executeUpdate();
    System.out.println(rowsUpdated + " row(s) updated.");

} catch (SQLException e) {
    e.printStackTrace();
}
```

### データの削除（DELETE）

```java
String sql = "DELETE FROM users WHERE id = ?";

try (Connection conn = DriverManager.getConnection(url, user, password);
     PreparedStatement pstmt = conn.prepareStatement(sql)) {

    pstmt.setInt(1, 1);

    int rowsDeleted = pstmt.executeUpdate();
    System.out.println(rowsDeleted + " row(s) deleted.");

} catch (SQLException e) {
    e.printStackTrace();
}
```

## トランザクション

複数の操作を1つの単位として扱います。

```java
Connection conn = null;

try {
    conn = DriverManager.getConnection(url, user, password);

    // 自動コミットを無効化
    conn.setAutoCommit(false);

    // 操作1: 口座Aから引き出し
    String sql1 = "UPDATE accounts SET balance = balance - ? WHERE id = ?";
    try (PreparedStatement pstmt1 = conn.prepareStatement(sql1)) {
        pstmt1.setDouble(1, 100.0);
        pstmt1.setInt(2, 1);
        pstmt1.executeUpdate();
    }

    // 操作2: 口座Bへ入金
    String sql2 = "UPDATE accounts SET balance = balance + ? WHERE id = ?";
    try (PreparedStatement pstmt2 = conn.prepareStatement(sql2)) {
        pstmt2.setDouble(1, 100.0);
        pstmt2.setInt(2, 2);
        pstmt2.executeUpdate();
    }

    // すべての操作が成功したらコミット
    conn.commit();
    System.out.println("Transaction committed successfully.");

} catch (SQLException e) {
    // エラーが発生したらロールバック
    if (conn != null) {
        try {
            conn.rollback();
            System.out.println("Transaction rolled back.");
        } catch (SQLException ex) {
            ex.printStackTrace();
        }
    }
    e.printStackTrace();
} finally {
    if (conn != null) {
        try {
            conn.setAutoCommit(true);
            conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

## 実践例: UserDAOパターン

DAO（Data Access Object）パターンで、データベースアクセスをカプセル化します。

### Userエンティティ

```java
public class User {
    private int id;
    private String name;
    private String email;
    private int age;

    // コンストラクタ
    public User() {}

    public User(String name, String email, int age) {
        this.name = name;
        this.email = email;
        this.age = age;
    }

    // ゲッター・セッター
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }

    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }

    @Override
    public String toString() {
        return "User{id=" + id + ", name='" + name + "', email='" + email + "', age=" + age + "}";
    }
}
```

### UserDAO

```java
import java.sql.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

public class UserDAO {
    private String url;
    private String user;
    private String password;

    public UserDAO(String url, String user, String password) {
        this.url = url;
        this.user = user;
        this.password = password;
    }

    private Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url, user, password);
    }

    // すべてのユーザーを取得
    public List<User> findAll() {
        List<User> users = new ArrayList<>();
        String sql = "SELECT id, name, email, age FROM users";

        try (Connection conn = getConnection();
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery(sql)) {

            while (rs.next()) {
                User u = new User();
                u.setId(rs.getInt("id"));
                u.setName(rs.getString("name"));
                u.setEmail(rs.getString("email"));
                u.setAge(rs.getInt("age"));
                users.add(u);
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }

        return users;
    }

    // IDでユーザーを検索
    public Optional<User> findById(int id) {
        String sql = "SELECT id, name, email, age FROM users WHERE id = ?";

        try (Connection conn = getConnection();
             PreparedStatement pstmt = conn.prepareStatement(sql)) {

            pstmt.setInt(1, id);
            ResultSet rs = pstmt.executeQuery();

            if (rs.next()) {
                User u = new User();
                u.setId(rs.getInt("id"));
                u.setName(rs.getString("name"));
                u.setEmail(rs.getString("email"));
                u.setAge(rs.getInt("age"));
                return Optional.of(u);
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }

        return Optional.empty();
    }

    // ユーザーを保存
    public void save(User user) {
        String sql = "INSERT INTO users (name, email, age) VALUES (?, ?, ?)";

        try (Connection conn = getConnection();
             PreparedStatement pstmt = conn.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS)) {

            pstmt.setString(1, user.getName());
            pstmt.setString(2, user.getEmail());
            pstmt.setInt(3, user.getAge());

            int affectedRows = pstmt.executeUpdate();

            if (affectedRows > 0) {
                // 自動生成されたIDを取得
                try (ResultSet generatedKeys = pstmt.getGeneratedKeys()) {
                    if (generatedKeys.next()) {
                        user.setId(generatedKeys.getInt(1));
                    }
                }
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    // ユーザーを更新
    public void update(User user) {
        String sql = "UPDATE users SET name = ?, email = ?, age = ? WHERE id = ?";

        try (Connection conn = getConnection();
             PreparedStatement pstmt = conn.prepareStatement(sql)) {

            pstmt.setString(1, user.getName());
            pstmt.setString(2, user.getEmail());
            pstmt.setInt(3, user.getAge());
            pstmt.setInt(4, user.getId());

            pstmt.executeUpdate();

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    // ユーザーを削除
    public void delete(int id) {
        String sql = "DELETE FROM users WHERE id = ?";

        try (Connection conn = getConnection();
             PreparedStatement pstmt = conn.prepareStatement(sql)) {

            pstmt.setInt(1, id);
            pstmt.executeUpdate();

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

### 使用例

```java
public class Main {
    public static void main(String[] args) {
        UserDAO userDAO = new UserDAO(
            "jdbc:mysql://localhost:3306/mydb",
            "root",
            "password"
        );

        // ユーザーの作成
        User newUser = new User("Alice", "alice@example.com", 25);
        userDAO.save(newUser);
        System.out.println("Created: " + newUser);

        // すべてのユーザーを表示
        System.out.println("\nAll users:");
        userDAO.findAll().forEach(System.out::println);

        // IDで検索
        Optional<User> foundUser = userDAO.findById(newUser.getId());
        foundUser.ifPresent(u -> {
            System.out.println("\nFound user: " + u);

            // 更新
            u.setAge(26);
            userDAO.update(u);
            System.out.println("Updated: " + u);
        });

        // 削除
        // userDAO.delete(newUser.getId());
    }
}
```

## 接続プール

頻繁にデータベースに接続する場合、接続プールを使用すると効率的です。

### HikariCP（推奨）

```xml
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>5.0.1</version>
</dependency>
```

```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

public class DatabaseConfig {
    private static HikariDataSource dataSource;

    static {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:mysql://localhost:3306/mydb");
        config.setUsername("root");
        config.setPassword("password");
        config.setMaximumPoolSize(10);

        dataSource = new HikariDataSource(config);
    }

    public static Connection getConnection() throws SQLException {
        return dataSource.getConnection();
    }
}
```

## まとめ

### JDBCの基本
- **Connection**: データベース接続
- **Statement / PreparedStatement**: SQL文の実行
- **ResultSet**: 結果の取得

### PreparedStatementの利点
- SQLインジェクション対策
- パフォーマンスの向上
- コードの可読性

### トランザクション
- `conn.setAutoCommit(false)`
- `conn.commit()`: 確定
- `conn.rollback()`: 取り消し

### DAOパターン
- データベースアクセスのカプセル化
- ビジネスロジックとの分離
- テストの容易化

### 実務での推奨
- **PreparedStatement**を使用
- **接続プール**（HikariCPなど）の利用
- **ORM**（JPA/Hibernateなど）の検討

これで Section 5 が完了です。次は総合演習に進みます。
