# 0-2. 開発環境の構築

Javaの開発を始めるには、JDK（Java Development Kit）とコードエディタ/IDEが必要です。

## JDKのインストール

### JDKとは

- **JDK（Java Development Kit）**: Javaアプリケーションを開発するためのツールキット
  - Javaコンパイラ（`javac`）
  - JVM（Java Virtual Machine）
  - 標準ライブラリ
  - デバッガーやその他の開発ツール

- **JRE（Java Runtime Environment）**: Javaアプリケーションを実行するための環境（JDKに含まれる）

### JDKのバージョン選択

現代の開発では、**JDK 17以降のLTS（Long-Term Support）バージョン**を推奨します。

- **LTSバージョン**: 11 (2018), 17 (2021), 21 (2023)
- **推奨**: JDK 17またはJDK 21
- **最新機能を試したい場合**: 最新のリリース版

**LTSとは？**
- 長期間のサポートが保証されるバージョン
- 本番環境での使用に適している
- セキュリティアップデートが長期間提供される

### インストール方法

#### 1. Oracle JDK

公式サイトからダウンロード:
https://www.oracle.com/java/technologies/downloads/

#### 2. OpenJDK（推奨）

オープンソース版のJava。複数のディストリビューションが存在します。

**主要なディストリビューション:**
- **Temurin（旧AdoptOpenJDK）**: Eclipse Foundationが提供
- **Amazon Corretto**: AWS提供の無料ディストリビューション
- **Microsoft Build of OpenJDK**: Microsoft提供

**インストール例（Ubuntu/Debian）:**
```bash
# Temurin (推奨)
sudo apt update
sudo apt install -y wget apt-transport-https
wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo apt-key add -
echo "deb https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
sudo apt update
sudo apt install temurin-17-jdk
```

**インストール例（macOS）:**
```bash
# Homebrewを使用
brew install openjdk@17

# パスの設定
echo 'export PATH="/opt/homebrew/opt/openjdk@17/bin:$PATH"' >> ~/.zshrc
```

**インストール例（Windows）:**
1. https://adoptium.net/ からインストーラーをダウンロード
2. インストーラーを実行
3. 環境変数 `JAVA_HOME` を設定

### インストール確認

ターミナル/コマンドプロンプトで以下を実行:

```bash
java -version
```

出力例:
```text
openjdk version "17.0.8" 2023-07-18
OpenJDK Runtime Environment Temurin-17.0.8+7 (build 17.0.8+7)
OpenJDK 64-Bit Server VM Temurin-17.0.8+7 (build 17.0.8+7, mixed mode, sharing)
```

```bash
javac -version
```

出力例:
```text
javac 17.0.8
```

!!! tip "複数のJavaバージョンを管理したい場合"
    **SDKMan!**（Linux/macOS）や**jEnv**を使用すると、複数のJavaバージョンを簡単に切り替えられます。
    ```bash
    # SDKMan!のインストール
    curl -s "https://get.sdkman.io" | bash

    # Javaのインストールと切り替え
    sdk install java 17.0.8-tem
    sdk install java 21.0.1-tem
    sdk use java 17.0.8-tem
    ```

## IDE（統合開発環境）のセットアップ

### 1. IntelliJ IDEA（推奨）

JetBrains社が提供する高機能IDE。

- **Community Edition（無料）**: Javaの基本的な開発に十分
- **Ultimate Edition（有料）**: WebフレームワークやDB連携機能が充実

**ダウンロード:**
https://www.jetbrains.com/idea/download/

**主な機能:**
- 強力なコード補完
- リファクタリング支援
- デバッガー
- 統合されたビルドツール（Maven、Gradle）
- Git連携

### 2. Visual Studio Code

軽量で拡張性の高いエディタ。

**必須拡張機能:**
- **Extension Pack for Java** (Microsoftが提供するJava開発パック)
  - Language Support for Java (Red Hat)
  - Debugger for Java
  - Test Runner for Java
  - Maven for Java
  - Project Manager for Java

**ダウンロード:**
https://code.visualstudio.com/

### 3. Eclipse

長い歴史を持つオープンソースIDE。

**ダウンロード:**
https://www.eclipse.org/downloads/

## IDEの基本設定

### IntelliJ IDEAの初期設定

1. **新規プロジェクトの作成**
   - `File > New > Project`
   - `Java` を選択
   - JDKを指定（インストール済みのものが自動検出される）

2. **JDKの設定確認**
   - `File > Project Structure > Project`
   - SDKが正しく設定されているか確認

3. **コードスタイルの設定（任意）**
   - `File > Settings > Editor > Code Style > Java`

### VS Codeの初期設定

1. **Extension Pack for Javaをインストール**
2. **JDKの設定**
   - `Ctrl/Cmd + ,` で設定を開く
   - "java.configuration.runtimes"で検索
   - JDKのパスを設定

3. **Javaプロジェクトの作成**
   - `Ctrl/Cmd + Shift + P` でコマンドパレットを開く
   - `Java: Create Java Project` を選択

## プロジェクト管理ツール（簡易紹介）

本格的な開発では、ビルドツールを使用します（詳細はSection 5で解説）。

- **Maven**: XML形式の設定ファイル（`pom.xml`）
- **Gradle**: Groovy/Kotlin DSLによる柔軟な設定

## まとめ

- JDK 17以降（LTS版）をインストール
- IDEは **IntelliJ IDEA Community Edition** または **VS Code + Extension Pack for Java** を推奨
- インストール後は `java -version` と `javac -version` で確認

これで開発環境の準備が整いました。次のセクションで最初のJavaプログラムを作成しましょう。
