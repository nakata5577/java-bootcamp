# 5-1. ビルドツール入門

ビルドツールは、プロジェクトのビルド、依存関係管理、テスト実行などを自動化するツールです。

## ビルドツールの必要性

### 手動管理の問題点

- ライブラリの手動ダウンロードとクラスパス設定
- コンパイル、テスト、パッケージングの手動実行
- 依存関係の競合や欠落
- チーム間での環境の不一致

### ビルドツールの利点

- 依存関係の自動管理
- 標準化されたプロジェクト構造
- ビルドプロセスの自動化
- 再現可能なビルド

## 主要なビルドツール

### Maven

XMLベースの設定ファイル（`pom.xml`）を使用します。

**特徴:**
- 規約優先（Convention over Configuration）
- 豊富なプラグインエコシステム
- 中央リポジトリによる依存関係管理

### Gradle

Groovy/Kotlin DSLベースの設定ファイル（`build.gradle`）を使用します。

**特徴:**
- 柔軟性が高い
- ビルドキャッシュによる高速化
- Android開発の公式ビルドツール

## Maven入門

### プロジェクト構造

```
myproject/
├── pom.xml                 # プロジェクト設定ファイル
├── src/
│   ├── main/
│   │   ├── java/          # アプリケーションコード
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── Main.java
│   │   └── resources/     # 設定ファイル、リソース
│   └── test/
│       ├── java/          # テストコード
│       └── resources/
└── target/                # ビルド成果物（自動生成）
```

### pom.xml の基本

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- プロジェクト情報 -->
    <groupId>com.example</groupId>
    <artifactId>myapp</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>My Application</name>
    <description>A sample Maven project</description>

    <!-- Java バージョン -->
    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <!-- 依存関係 -->
    <dependencies>
        <!-- JUnit 5 -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.9.3</version>
            <scope>test</scope>
        </dependency>

        <!-- Apache Commons Lang -->
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.12.0</version>
        </dependency>
    </dependencies>

    <!-- ビルド設定 -->
    <build>
        <plugins>
            <!-- Maven Compiler Plugin -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.11.0</version>
            </plugin>

            <!-- Maven Surefire Plugin (テスト実行) -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.0.0</version>
            </plugin>
        </plugins>
    </build>
</project>
```

### Mavenの主要コマンド

```bash
# プロジェクトのコンパイル
mvn compile

# テストの実行
mvn test

# パッケージング（JAR/WARの作成）
mvn package

# target ディレクトリのクリーンアップ
mvn clean

# クリーン + パッケージング（一般的）
mvn clean package

# 依存関係のインストール（ローカルリポジトリへ）
mvn install

# 依存関係のダウンロード
mvn dependency:resolve

# 依存関係ツリーの表示
mvn dependency:tree

# プロジェクトの実行（exec-maven-pluginが必要）
mvn exec:java -Dexec.mainClass="com.example.Main"
```

### 依存関係のスコープ

```xml
<dependencies>
    <!-- compile: デフォルト、すべてのクラスパスで利用可能 -->
    <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-lang3</artifactId>
        <version>3.12.0</version>
        <scope>compile</scope>
    </dependency>

    <!-- test: テストコードのみで利用可能 -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.9.3</version>
        <scope>test</scope>
    </dependency>

    <!-- provided: コンパイル時のみ（実行環境が提供） -->
    <dependency>
        <groupId>jakarta.servlet</groupId>
        <artifactId>jakarta.servlet-api</artifactId>
        <version>5.0.0</version>
        <scope>provided</scope>
    </dependency>

    <!-- runtime: 実行時のみ -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.33</version>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

### 実行可能JARの作成

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-jar-plugin</artifactId>
            <version>3.3.0</version>
            <configuration>
                <archive>
                    <manifest>
                        <mainClass>com.example.Main</mainClass>
                    </manifest>
                </archive>
            </configuration>
        </plugin>

        <!-- または、依存関係を含む Fat JAR -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>3.5.0</version>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <transformers>
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                <mainClass>com.example.Main</mainClass>
                            </transformer>
                        </transformers>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

実行:
```bash
mvn clean package
java -jar target/myapp-1.0-SNAPSHOT.jar
```

## Gradle入門

### プロジェクト構造

Mavenと同じ構造を使用します。

```
myproject/
├── build.gradle           # プロジェクト設定ファイル
├── settings.gradle        # プロジェクト名などの設定
├── src/
│   ├── main/
│   │   └── java/
│   └── test/
│       └── java/
└── build/                 # ビルド成果物（自動生成）
```

### build.gradle の基本（Groovy DSL）

```groovy
plugins {
    id 'java'
    id 'application'
}

group = 'com.example'
version = '1.0-SNAPSHOT'

// Java バージョン
java {
    sourceCompatibility = JavaVersion.VERSION_17
    targetCompatibility = JavaVersion.VERSION_17
}

// リポジトリ
repositories {
    mavenCentral()
}

// 依存関係
dependencies {
    // コンパイル時
    implementation 'org.apache.commons:commons-lang3:3.12.0'

    // テスト
    testImplementation 'org.junit.jupiter:junit-jupiter:5.9.3'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}

// アプリケーション設定
application {
    mainClass = 'com.example.Main'
}

// テスト設定
test {
    useJUnitPlatform()
}
```

### Gradleの主要コマンド

```bash
# プロジェクトのコンパイル
./gradlew build

# テストの実行
./gradlew test

# クリーンアップ
./gradlew clean

# クリーン + ビルド
./gradlew clean build

# アプリケーションの実行
./gradlew run

# 依存関係の表示
./gradlew dependencies

# 利用可能なタスクの表示
./gradlew tasks
```

### Gradle Wrapper

プロジェクトに含まれるGradleのバージョン管理ツールです。

```bash
# Unix/Linux/macOS
./gradlew build

# Windows
gradlew.bat build
```

**利点**: チーム全員が同じバージョンのGradleを使用できる

## Maven vs Gradle

| | Maven | Gradle |
|---|-------|--------|
| **設定ファイル** | XML（pom.xml） | Groovy/Kotlin DSL |
| **柔軟性** | 規約重視 | 高い柔軟性 |
| **ビルド速度** | 標準 | 高速（キャッシュ機能） |
| **学習曲線** | 緩やか | やや急 |
| **エコシステム** | 成熟 | 成長中 |
| **適用分野** | エンタープライズ | Android、モダンプロジェクト |

## 依存関係の検索

### Maven Central Repository

https://search.maven.org/

ライブラリを検索し、依存関係の記述をコピーできます。

### 例: Jackson（JSONライブラリ）

```xml
<!-- Maven -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.15.2</version>
</dependency>
```

```groovy
// Gradle
implementation 'com.fasterxml.jackson.core:jackson-databind:2.15.2'
```

## 実践例: Mavenプロジェクト

### プロジェクトの作成

```bash
mvn archetype:generate \
  -DgroupId=com.example \
  -DartifactId=myapp \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DarchetypeVersion=1.4 \
  -DinteractiveMode=false
```

### src/main/java/com/example/Main.java

```java
package com.example;

import org.apache.commons.lang3.StringUtils;

public class Main {
    public static void main(String[] args) {
        String text = "hello world";
        String capitalized = StringUtils.capitalize(text);
        System.out.println(capitalized);  // Hello world
    }
}
```

### ビルドと実行

```bash
# コンパイルとパッケージング
mvn clean package

# 実行
java -cp target/myapp-1.0-SNAPSHOT.jar com.example.Main
```

## まとめ

### ビルドツールの利点
- 依存関係の自動管理
- 標準化されたプロジェクト構造
- ビルドプロセスの自動化
- チーム開発の効率化

### Maven
- XML設定
- 規約優先
- 成熟したエコシステム
- エンタープライズで広く使用

### Gradle
- Groovy/Kotlin DSL
- 高い柔軟性
- 高速なビルド
- モダンなプロジェクト向け

### 選択の指針
- **Maven**: 安定性と標準化を重視
- **Gradle**: 柔軟性と性能を重視

次のセクションでは、ユニットテストについて学びます。
