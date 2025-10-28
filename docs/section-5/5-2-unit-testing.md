# 5-2. ユニットテスト

ユニットテストは、コードの品質を保証し、リファクタリングを安全に行うための重要な実践です。

## ユニットテストとは

**ユニットテスト**は、プログラムの最小単位（メソッドやクラス）が正しく動作するかを検証するテストです。

### テストの重要性

1. **バグの早期発見**: 開発中に問題を検出
2. **リファクタリングの安全性**: 変更後も既存機能が動作することを保証
3. **ドキュメント**: テストコードは使用例となる
4. **設計の改善**: テスト可能なコードは良い設計

### テストの種類

- **ユニットテスト**: 個別の関数やメソッド
- **統合テスト**: 複数のコンポーネントの連携
- **E2Eテスト**: システム全体の動作

このセクションでは、ユニットテストに焦点を当てます。

## JUnit 5

JUnit 5は、Javaで最も広く使われているテストフレームワークです。

### Maven依存関係

```xml
<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.9.3</version>
        <scope>test</scope>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.0.0</version>
        </plugin>
    </plugins>
</build>
```

### Gradle依存関係

```groovy
dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter:5.9.3'
}

test {
    useJUnitPlatform()
}
```

## 基本的なテスト

### テストクラスの作成

```java
// src/main/java/com/example/Calculator.java
package com.example;

public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }

    public int subtract(int a, int b) {
        return a - b;
    }

    public int multiply(int a, int b) {
        return a * b;
    }

    public int divide(int a, int b) {
        if (b == 0) {
            throw new IllegalArgumentException("Division by zero");
        }
        return a / b;
    }
}
```

### テストコード

```java
// src/test/java/com/example/CalculatorTest.java
package com.example;

import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {
    private Calculator calculator = new Calculator();

    @Test
    void testAdd() {
        int result = calculator.add(2, 3);
        assertEquals(5, result);
    }

    @Test
    void testSubtract() {
        assertEquals(2, calculator.subtract(5, 3));
    }

    @Test
    void testMultiply() {
        assertEquals(15, calculator.multiply(3, 5));
    }

    @Test
    void testDivide() {
        assertEquals(2, calculator.divide(6, 3));
    }

    @Test
    void testDivideByZero() {
        assertThrows(IllegalArgumentException.class, () -> {
            calculator.divide(10, 0);
        });
    }
}
```

### テストの実行

```bash
# Maven
mvn test

# Gradle
./gradlew test
```

## アサーション（Assertion）

### 基本的なアサーション

```java
import static org.junit.jupiter.api.Assertions.*;

@Test
void testAssertions() {
    // 等価性
    assertEquals(5, 2 + 3);
    assertEquals("Hello", "Hel" + "lo");

    // 真偽値
    assertTrue(5 > 3);
    assertFalse(5 < 3);

    // null
    assertNull(null);
    assertNotNull("not null");

    // 同一性（同じオブジェクト）
    String str = "test";
    assertSame(str, str);

    // 配列
    int[] expected = {1, 2, 3};
    int[] actual = {1, 2, 3};
    assertArrayEquals(expected, actual);

    // メッセージ付き
    assertEquals(5, 2 + 3, "Addition should work");
}
```

### 例外のテスト

```java
@Test
void testException() {
    Exception exception = assertThrows(
        IllegalArgumentException.class,
        () -> calculator.divide(10, 0)
    );

    assertEquals("Division by zero", exception.getMessage());
}
```

### assertAllによるグループ化

```java
@Test
void testMultipleAssertions() {
    Person person = new Person("Alice", 25);

    assertAll("person",
        () -> assertEquals("Alice", person.getName()),
        () -> assertEquals(25, person.getAge()),
        () -> assertTrue(person.isAdult())
    );
}
```

## テストのライフサイクル

```java
import org.junit.jupiter.api.*;

class LifecycleTest {
    @BeforeAll
    static void setUpAll() {
        // すべてのテスト実行前に1回だけ実行
        System.out.println("Before all tests");
    }

    @BeforeEach
    void setUp() {
        // 各テスト実行前に実行
        System.out.println("Before each test");
    }

    @Test
    void test1() {
        System.out.println("Test 1");
    }

    @Test
    void test2() {
        System.out.println("Test 2");
    }

    @AfterEach
    void tearDown() {
        // 各テスト実行後に実行
        System.out.println("After each test");
    }

    @AfterAll
    static void tearDownAll() {
        // すべてのテスト実行後に1回だけ実行
        System.out.println("After all tests");
    }
}

// 出力:
// Before all tests
// Before each test
// Test 1
// After each test
// Before each test
// Test 2
// After each test
// After all tests
```

## パラメータ化テスト

同じテストを異なる入力で複数回実行します。

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.*;

class ParameterizedTests {
    @ParameterizedTest
    @ValueSource(ints = {2, 4, 6, 8, 10})
    void testEvenNumbers(int number) {
        assertTrue(number % 2 == 0);
    }

    @ParameterizedTest
    @ValueSource(strings = {"", "  ", "\t", "\n"})
    void testBlankStrings(String input) {
        assertTrue(input.isBlank());
    }

    @ParameterizedTest
    @CsvSource({
        "1, 1, 2",
        "2, 3, 5",
        "5, 5, 10"
    })
    void testAdd(int a, int b, int expected) {
        assertEquals(expected, calculator.add(a, b));
    }

    @ParameterizedTest
    @MethodSource("provideTestData")
    void testWithMethodSource(String input, int expected) {
        assertEquals(expected, input.length());
    }

    static Stream<Arguments> provideTestData() {
        return Stream.of(
            Arguments.of("Hello", 5),
            Arguments.of("World", 5),
            Arguments.of("JUnit", 5)
        );
    }
}
```

## テストの整理

### DisplayName

```java
@DisplayName("Calculator Tests")
class CalculatorTest {
    @Test
    @DisplayName("Adding two positive numbers should return their sum")
    void testAdd() {
        assertEquals(5, calculator.add(2, 3));
    }
}
```

### Nested Tests

```java
@DisplayName("User Tests")
class UserTest {
    @Nested
    @DisplayName("When new user is created")
    class WhenNew {
        private User user;

        @BeforeEach
        void createNewUser() {
            user = new User("Alice", 25);
        }

        @Test
        @DisplayName("user should have correct name")
        void shouldHaveCorrectName() {
            assertEquals("Alice", user.getName());
        }

        @Test
        @DisplayName("user should have correct age")
        void shouldHaveCorrectAge() {
            assertEquals(25, user.getAge());
        }
    }

    @Nested
    @DisplayName("When user is adult")
    class WhenAdult {
        @Test
        @DisplayName("should return true for age >= 18")
        void shouldBeAdult() {
            User user = new User("Bob", 30);
            assertTrue(user.isAdult());
        }
    }
}
```

## テスト駆動開発（TDD）

**TDD**は、テストを先に書いてから実装するアプローチです。

### TDDのサイクル

1. **Red**: 失敗するテストを書く
2. **Green**: テストが通る最小限の実装
3. **Refactor**: コードを改善

### 例: StringUtilsの実装

```java
// 1. Red: テストを先に書く
class StringUtilsTest {
    @Test
    void testReverse() {
        assertEquals("olleh", StringUtils.reverse("hello"));
        assertEquals("", StringUtils.reverse(""));
        assertThrows(NullPointerException.class,
            () -> StringUtils.reverse(null));
    }
}

// 2. Green: 最小限の実装
class StringUtils {
    public static String reverse(String str) {
        if (str == null) {
            throw new NullPointerException();
        }
        return new StringBuilder(str).reverse().toString();
    }
}

// 3. Refactor: 必要に応じて改善
```

## モックとスタブ（Mockito）

外部依存をモック化してテストを独立させます。

### Maven依存関係

```xml
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-core</artifactId>
    <version>5.3.1</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-junit-jupiter</artifactId>
    <version>5.3.1</version>
    <scope>test</scope>
</dependency>
```

### 基本的な使用例

```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;

// テスト対象のクラス
class UserService {
    private UserRepository repository;

    public UserService(UserRepository repository) {
        this.repository = repository;
    }

    public User getUser(String id) {
        return repository.findById(id);
    }

    public void deleteUser(String id) {
        repository.delete(id);
    }
}

interface UserRepository {
    User findById(String id);
    void delete(String id);
}

// テストクラス
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    @Mock
    private UserRepository repository;

    @Test
    void testGetUser() {
        // モックの振る舞いを定義
        User mockUser = new User("1", "Alice");
        when(repository.findById("1")).thenReturn(mockUser);

        // テスト対象の実行
        UserService service = new UserService(repository);
        User user = service.getUser("1");

        // 検証
        assertEquals("Alice", user.getName());
        verify(repository).findById("1");  // メソッドが呼ばれたか確認
    }

    @Test
    void testDeleteUser() {
        UserService service = new UserService(repository);
        service.deleteUser("1");

        verify(repository, times(1)).delete("1");
    }
}
```

## テストのベストプラクティス

### 1. 1テスト1アサーション（原則として）

```java
// 良い例
@Test
void testAddPositiveNumbers() {
    assertEquals(5, calculator.add(2, 3));
}

@Test
void testAddNegativeNumbers() {
    assertEquals(-5, calculator.add(-2, -3));
}
```

### 2. 意味のあるテスト名

```java
// 良い例
@Test
void shouldReturnEmptyListWhenNoUsersExist() { }

// 避けるべき
@Test
void test1() { }
```

### 3. AAA パターン（Arrange-Act-Assert）

```java
@Test
void testWithdraw() {
    // Arrange（準備）
    BankAccount account = new BankAccount(100);

    // Act（実行）
    account.withdraw(30);

    // Assert（検証）
    assertEquals(70, account.getBalance());
}
```

### 4. テストの独立性

各テストは他のテストに依存してはいけません。

```java
// 避けるべき: テスト間で状態を共有
static int counter = 0;

@Test
void test1() {
    counter++;
    assertEquals(1, counter);
}

@Test
void test2() {
    // test1の実行順序に依存
    assertEquals(2, counter);  // 危険
}
```

## テストカバレッジ

コードのどれだけがテストされているかを測定します。

### JaCoCo（Javaカバレッジツール）

```xml
<!-- Maven -->
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.10</version>
    <executions>
        <execution>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <id>report</id>
            <phase>test</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

実行:
```bash
mvn clean test
# レポート: target/site/jacoco/index.html
```

## まとめ

### JUnit 5の基本
- `@Test`: テストメソッド
- `assertEquals / assertTrue / assertThrows`: アサーション
- `@BeforeEach / @AfterEach`: セットアップとクリーンアップ
- `@ParameterizedTest`: パラメータ化テスト

### テストのベストプラクティス
- 1テスト1アサーション
- 意味のあるテスト名
- AAAパターン（Arrange-Act-Assert）
- テストの独立性

### TDD（テスト駆動開発）
1. Red: 失敗するテストを書く
2. Green: 最小限の実装
3. Refactor: 改善

次のセクションでは、データベース接続について学びます。
