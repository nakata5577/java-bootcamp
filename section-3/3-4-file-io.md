# 3-4. ファイル入出力 (I/O)

ファイルの読み書きは、多くのアプリケーションで必要となる基本的な機能です。

## Javaのファイル入出力の種類

Javaには複数のファイル入出力APIがあります。

1. **java.io**: 古典的なI/O API（ストリームベース）
2. **java.nio.file**: 現代的なファイルAPI（Java 7以降、推奨）

## java.nio.file（現代的なアプローチ）

### Pathクラス

ファイルやディレクトリのパスを表します。

```java
import java.nio.file.Path;
import java.nio.file.Paths;

// Pathの取得
Path path = Paths.get("data.txt");
Path absolutePath = Paths.get("/home/user/data.txt");

// パスの結合
Path dir = Paths.get("/home/user");
Path file = dir.resolve("data.txt");  // /home/user/data.txt

// ファイル名の取得
Path fileName = path.getFileName();

// 親ディレクトリ
Path parent = path.getParent();

// 絶対パスに変換
Path absolute = path.toAbsolutePath();
```

### Filesクラス（推奨）

ファイル操作のための便利なメソッドを提供します。

#### ファイルの読み込み

```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.io.IOException;
import java.util.List;

public class FileReadExample {
    public static void main(String[] args) {
        Path path = Paths.get("data.txt");

        try {
            // すべての行を一度に読み込み
            List<String> lines = Files.readAllLines(path);
            for (String line : lines) {
                System.out.println(line);
            }

            // または
            String content = Files.readString(path);  // Java 11以降
            System.out.println(content);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 大きなファイルの読み込み（ストリーム）

```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.stream.Stream;

public class LargeFileRead {
    public static void main(String[] args) {
        Path path = Paths.get("large_file.txt");

        try (Stream<String> lines = Files.lines(path)) {
            lines.filter(line -> line.contains("ERROR"))
                 .forEach(System.out::println);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### ファイルの書き込み

```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.Arrays;
import java.util.List;

public class FileWriteExample {
    public static void main(String[] args) {
        Path path = Paths.get("output.txt");

        try {
            // 文字列の書き込み
            String content = "Hello, World!\nThis is a test.";
            Files.writeString(path, content);  // Java 11以降

            // 複数行の書き込み
            List<String> lines = Arrays.asList(
                "Line 1",
                "Line 2",
                "Line 3"
            );
            Files.write(path, lines);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 追記モード

```java
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.StandardOpenOption;

public class AppendExample {
    public static void main(String[] args) {
        Path path = Paths.get("output.txt");

        try {
            String newContent = "\nAppended line";
            Files.writeString(path, newContent, StandardOpenOption.APPEND);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### ファイル・ディレクトリ操作

```java
import java.nio.file.*;
import java.io.IOException;

public class FileOperations {
    public static void main(String[] args) {
        Path source = Paths.get("source.txt");
        Path dest = Paths.get("destination.txt");
        Path dir = Paths.get("mydir");

        try {
            // ファイルの存在確認
            boolean exists = Files.exists(source);

            // ディレクトリの確認
            boolean isDir = Files.isDirectory(dir);

            // ファイルのコピー
            Files.copy(source, dest, StandardCopyOption.REPLACE_EXISTING);

            // ファイルの移動
            Files.move(source, dest, StandardCopyOption.REPLACE_EXISTING);

            // ファイルの削除
            Files.delete(dest);

            // 削除（存在しない場合は無視）
            Files.deleteIfExists(dest);

            // ディレクトリの作成
            Files.createDirectory(dir);

            // 親ディレクトリも含めて作成
            Files.createDirectories(Paths.get("path/to/dir"));

            // ファイル情報の取得
            long size = Files.size(source);
            boolean readable = Files.isReadable(source);
            boolean writable = Files.isWritable(source);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### ディレクトリの走査

```java
import java.nio.file.*;
import java.io.IOException;
import java.util.stream.Stream;

public class DirectoryTraversal {
    public static void main(String[] args) {
        Path dir = Paths.get(".");

        try {
            // ディレクトリ内のファイル一覧
            try (Stream<Path> paths = Files.list(dir)) {
                paths.filter(Files::isRegularFile)
                     .forEach(System.out::println);
            }

            // 再帰的に走査
            try (Stream<Path> paths = Files.walk(dir)) {
                paths.filter(Files::isRegularFile)
                     .filter(p -> p.toString().endsWith(".java"))
                     .forEach(System.out::println);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## java.io（古典的なアプローチ）

### バッファ付きテキストファイルの読み込み

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class BufferedReaderExample {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("data.txt"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### バッファ付きテキストファイルの書き込み

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class BufferedWriterExample {
    public static void main(String[] args) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("output.txt"))) {
            writer.write("Line 1");
            writer.newLine();
            writer.write("Line 2");
            writer.newLine();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### バイナリファイルの読み書き

```java
import java.io.*;

public class BinaryFileExample {
    // 書き込み
    public static void writeData(String filename, byte[] data) throws IOException {
        try (FileOutputStream fos = new FileOutputStream(filename);
             BufferedOutputStream bos = new BufferedOutputStream(fos)) {
            bos.write(data);
        }
    }

    // 読み込み
    public static byte[] readData(String filename) throws IOException {
        try (FileInputStream fis = new FileInputStream(filename);
             BufferedInputStream bis = new BufferedInputStream(fis)) {
            return bis.readAllBytes();
        }
    }
}
```

## データストリーム

プリミティブ型のデータを読み書きします。

```java
import java.io.*;

public class DataStreamExample {
    // 書き込み
    public static void writeData(String filename) throws IOException {
        try (DataOutputStream dos = new DataOutputStream(
                new FileOutputStream(filename))) {
            dos.writeInt(123);
            dos.writeDouble(3.14);
            dos.writeUTF("Hello");
            dos.writeBoolean(true);
        }
    }

    // 読み込み
    public static void readData(String filename) throws IOException {
        try (DataInputStream dis = new DataInputStream(
                new FileInputStream(filename))) {
            int intValue = dis.readInt();
            double doubleValue = dis.readDouble();
            String stringValue = dis.readUTF();
            boolean boolValue = dis.readBoolean();

            System.out.println(intValue);
            System.out.println(doubleValue);
            System.out.println(stringValue);
            System.out.println(boolValue);
        }
    }
}
```

## オブジェクトのシリアライゼーション

オブジェクトをファイルに保存・読み込みできます。

```java
import java.io.*;

// Serializableインターフェースを実装
class Person implements Serializable {
    private static final long serialVersionUID = 1L;

    private String name;
    private int age;
    private transient String password;  // transient: シリアライズしない

    public Person(String name, int age, String password) {
        this.name = name;
        this.age = age;
        this.password = password;
    }

    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + ", password='" + password + "'}";
    }
}

public class SerializationExample {
    // オブジェクトの保存
    public static void saveObject(String filename, Person person) throws IOException {
        try (ObjectOutputStream oos = new ObjectOutputStream(
                new FileOutputStream(filename))) {
            oos.writeObject(person);
        }
    }

    // オブジェクトの読み込み
    public static Person loadObject(String filename) throws IOException, ClassNotFoundException {
        try (ObjectInputStream ois = new ObjectInputStream(
                new FileInputStream(filename))) {
            return (Person) ois.readObject();
        }
    }

    public static void main(String[] args) {
        try {
            Person person = new Person("Alice", 25, "secret123");
            saveObject("person.ser", person);

            Person loaded = loadObject("person.ser");
            System.out.println(loaded);  // passwordはnull（transient）

        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

## 実践例: CSVファイルの読み書き

```java
import java.io.*;
import java.nio.file.*;
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

    public String toCsv() {
        return id + "," + name + "," + score;
    }

    public static Student fromCsv(String line) {
        String[] parts = line.split(",");
        return new Student(parts[0], parts[1], Integer.parseInt(parts[2]));
    }

    @Override
    public String toString() {
        return "Student{id='" + id + "', name='" + name + "', score=" + score + "}";
    }
}

public class CsvExample {
    // CSVファイルに保存
    public static void saveStudents(String filename, List<Student> students) throws IOException {
        List<String> lines = new ArrayList<>();
        lines.add("ID,Name,Score");  // ヘッダー

        for (Student student : students) {
            lines.add(student.toCsv());
        }

        Files.write(Paths.get(filename), lines);
    }

    // CSVファイルから読み込み
    public static List<Student> loadStudents(String filename) throws IOException {
        List<Student> students = new ArrayList<>();
        List<String> lines = Files.readAllLines(Paths.get(filename));

        // ヘッダーをスキップ
        for (int i = 1; i < lines.size(); i++) {
            students.add(Student.fromCsv(lines.get(i)));
        }

        return students;
    }

    public static void main(String[] args) {
        List<Student> students = Arrays.asList(
            new Student("001", "Alice", 95),
            new Student("002", "Bob", 87),
            new Student("003", "Charlie", 92)
        );

        try {
            // 保存
            saveStudents("students.csv", students);
            System.out.println("Saved successfully");

            // 読み込み
            List<Student> loaded = loadStudents("students.csv");
            System.out.println("Loaded students:");
            for (Student student : loaded) {
                System.out.println(student);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 実践例: 設定ファイルの読み書き（Properties）

```java
import java.io.*;
import java.util.Properties;

public class ConfigExample {
    // 設定の保存
    public static void saveConfig(String filename, Properties props) throws IOException {
        try (FileOutputStream fos = new FileOutputStream(filename)) {
            props.store(fos, "Application Configuration");
        }
    }

    // 設定の読み込み
    public static Properties loadConfig(String filename) throws IOException {
        Properties props = new Properties();
        try (FileInputStream fis = new FileInputStream(filename)) {
            props.load(fis);
        }
        return props;
    }

    public static void main(String[] args) {
        Properties config = new Properties();
        config.setProperty("database.url", "jdbc:mysql://localhost:3306/mydb");
        config.setProperty("database.user", "admin");
        config.setProperty("database.password", "secret");
        config.setProperty("app.port", "8080");

        try {
            // 保存
            saveConfig("config.properties", config);

            // 読み込み
            Properties loaded = loadConfig("config.properties");
            System.out.println("Database URL: " + loaded.getProperty("database.url"));
            System.out.println("Port: " + loaded.getProperty("app.port"));

            // デフォルト値
            String timeout = loaded.getProperty("timeout", "30");
            System.out.println("Timeout: " + timeout);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## ベストプラクティス

### 1. try-with-resourcesを使用

```java
// 良い例
try (BufferedReader reader = new BufferedReader(new FileReader("file.txt"))) {
    // 処理
}  // 自動的にクローズされる

// 悪い例
BufferedReader reader = new BufferedReader(new FileReader("file.txt"));
// ...
reader.close();  // 例外が発生するとクローズされない
```

### 2. 大きなファイルはストリームで処理

```java
// メモリ効率が良い
try (Stream<String> lines = Files.lines(path)) {
    lines.filter(line -> line.contains("ERROR"))
         .forEach(System.out::println);
}
```

### 3. パスの処理にはPathsとFilesを使用

```java
// 推奨（java.nio.file）
Path path = Paths.get("file.txt");
List<String> lines = Files.readAllLines(path);

// 古い（java.io）
File file = new File("file.txt");
```

### 4. 例外処理を適切に

```java
try {
    // ファイル操作
} catch (FileNotFoundException e) {
    System.err.println("File not found: " + e.getMessage());
} catch (IOException e) {
    System.err.println("I/O error: " + e.getMessage());
}
```

## まとめ

### 推奨API（java.nio.file）
- **Files.readAllLines()**: 小さなファイルの読み込み
- **Files.lines()**: 大きなファイルのストリーム処理
- **Files.writeString() / write()**: ファイルの書き込み
- **Files.copy() / move() / delete()**: ファイル操作

### 古典的API（java.io）
- **BufferedReader / BufferedWriter**: テキストファイル
- **FileInputStream / FileOutputStream**: バイナリファイル
- **ObjectInputStream / ObjectOutputStream**: シリアライゼーション

### ポイント
- try-with-resourcesを使用してリソースを自動的にクローズ
- 大きなファイルはストリームで処理
- 例外処理を適切に行う
- 現代的なjava.nio.file APIを優先

次のセクションでは、モダンJavaの機能について学びます。
