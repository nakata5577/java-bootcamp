# Java Bootcamp

> Javaを短期間で集中して学ぶための実践的な教科書プロジェクト

[![GitHub Pages](https://img.shields.io/badge/docs-GitHub%20Pages-blue)](https://nakata5577.github.io/java-bootcamp/)
[![License](https://img.shields.io/badge/license-Educational-green)]()
[![Made with MkDocs](https://img.shields.io/badge/Made%20with-MkDocs-526CFE)](https://www.mkdocs.org/)

## プロジェクトについて

このプロジェクトは、**プログラミング経験者がJavaを効率的に習得する**ための体系的な教科書です。基本的な文法から実践的な開発スキルまで、網羅的かつ実践的な内容を提供します。

### 特徴

- **体系的なカリキュラム**: 6つのセクションで段階的に学習
- **実践重視**: 豊富なコード例と練習問題
- **モダンなJava**: Java 17以降の最新機能に対応
- **美しいドキュメント**: MkDocs Materialによる読みやすいウェブサイト
- **日本語対応**: すべて日本語で解説

### 対象者

このBootcampは以下のような方を対象としています：

- 他のプログラミング言語の経験があり、Javaを学びたい方
- Javaの基礎を体系的に学び直したい方
- モダンなJavaの機能（ラムダ式、Stream APIなど）を習得したい方
- エンタープライズ開発のスキルを身につけたい方

## カリキュラム

### Section 0: イントロダクション

Javaの世界へようこそ。まずは開発の準備を整え、最初のプログラムを動かしてみます。

| レッスン | 内容 | 所要時間 |
|---------|------|----------|
| [0-1. Javaとは](docs/section-0/0-1-what-is-java.md) | Javaの特徴、JVMの仕組み、エコシステム | 30分 |
| [0-2. 開発環境の構築](docs/section-0/0-2-setup.md) | JDK、IDEのインストールと設定 | 45分 |
| [0-3. Hello, World!](docs/section-0/0-3-hello-world.md) | 最初のJavaプログラムを作成・実行 | 20分 |
| [0-4. プロジェクトの基本構造](docs/section-0/0-4-project-structure.md) | パッケージ、クラスファイルの構成 | 25分 |

### Section 1: Javaの基本

プログラミングの基礎となる文法やデータ構造を学びます。

| レッスン | 内容 | 所要時間 |
|---------|------|----------|
| [1-1. 変数とデータ型](docs/section-1/1-1-variables-and-types.md) | プリミティブ型、参照型、型変換 | 60分 |
| [1-2. 演算子](docs/section-1/1-2-operators.md) | 算術、比較、論理、ビット演算子 | 40分 |
| [1-3. 制御フロー](docs/section-1/1-3-control-flow.md) | if文、switch文、ループ | 50分 |
| [1-4. 配列](docs/section-1/1-4-arrays.md) | 配列の宣言、操作、多次元配列 | 45分 |

### Section 2: オブジェクト指向プログラミング (OOP)

Javaの核心であるオブジェクト指向の考え方をマスターします。

| レッスン | 内容 | 所要時間 |
|---------|------|----------|
| [2-1. クラスとオブジェクト](docs/section-2/2-1-classes-and-objects.md) | クラスの定義、オブジェクトの生成と使用 | 60分 |
| [2-2. メソッドとコンストラクタ](docs/section-2/2-2-methods-and-constructors.md) | メソッドの定義、オーバーロード、コンストラクタ | 55分 |
| [2-3. カプセル化](docs/section-2/2-3-encapsulation.md) | アクセス修飾子、getter/setter | 40分 |
| [2-4. 継承](docs/section-2/2-4-inheritance.md) | クラスの継承、super、オーバーライド | 50分 |
| [2-5. ポリモーフィズム](docs/section-2/2-5-polymorphism.md) | 多態性の概念と活用 | 55分 |
| [2-6. 抽象クラスとインターフェース](docs/section-2/2-6-abstract-and-interface.md) | 抽象化、インターフェースの設計 | 60分 |

### Section 3: Javaの重要機能

より高度で実践的なアプリケーション開発に不可欠な機能を学びます。

| レッスン | 内容 | 所要時間 |
|---------|------|----------|
| [3-1. 例外処理](docs/section-3/3-1-exception-handling.md) | try-catch、例外の種類、カスタム例外 | 50分 |
| [3-2. コレクションフレームワーク](docs/section-3/3-2-collections.md) | List、Set、Map、Queue | 70分 |
| [3-3. ジェネリクス](docs/section-3/3-3-generics.md) | 型パラメータ、ジェネリッククラス | 55分 |
| [3-4. ファイル入出力](docs/section-3/3-4-file-io.md) | ファイルの読み書き、NIO.2 | 60分 |

### Section 4: モダンJava

Java 8以降で導入された、より簡潔で強力なコーディングスタイルを習得します。

| レッスン | 内容 | 所要時間 |
|---------|------|----------|
| [4-1. ラムダ式](docs/section-4/4-1-lambda.md) | 関数型インターフェース、ラムダ式の構文 | 55分 |
| [4-2. Stream API](docs/section-4/4-2-stream-api.md) | データ処理パイプライン、中間操作・終端操作 | 75分 |
| [4-3. Optional](docs/section-4/4-3-optional.md) | null安全な値の扱い方 | 40分 |

### Section 5: 実践開発への一歩

実際の開発現場で使われるツールや技術に触れます。

| レッスン | 内容 | 所要時間 |
|---------|------|----------|
| [5-1. ビルドツール入門](docs/section-5/5-1-build-tools.md) | Maven、Gradleの基本 | 60分 |
| [5-2. ユニットテスト](docs/section-5/5-2-unit-testing.md) | JUnit 5、テスト駆動開発 | 70分 |
| [5-3. データベース接続](docs/section-5/5-3-jdbc.md) | JDBCによるDB操作 | 65分 |

### Section 6: 総合演習

これまでの知識を総動員して、小規模なコンソールアプリケーションを設計・実装します。

| レッスン | 内容 | 所要時間 |
|---------|------|----------|
| [6. 総合演習プロジェクト](docs/section-6/6-final-project.md) | タスク管理アプリケーションの構築 | 4-6時間 |

**総学習時間**: 約30-40時間（個人差があります）

## クイックスタート

1. **ドキュメントサイトにアクセス**: [https://nakata5577.github.io/java-bootcamp/](https://nakata5577.github.io/java-bootcamp/)
2. **開発環境を構築**: [0-2. 開発環境の構築](docs/section-0/0-2-setup.md) を参照
3. **Hello, World!を実行**: [0-3. Hello, World!](docs/section-0/0-3-hello-world.md) で最初のプログラムを動かす
4. **順番に学習**: Section 1から順に進めていきましょう

## 学習の進め方

### 基本的な学習フロー

```
Section 0: 環境構築 (2時間)
    ↓
Section 1-2: 基礎文法とOOP (1週間)
    ↓
Section 3-4: 重要機能とモダンJava (1週間)
    ↓
Section 5: 実践開発 (数日)
    ↓
Section 6: 総合演習 (1-2日)
```

### 効果的な学習方法

1. **順序を守る**
   - 各セクションは前のセクションの知識を前提としています
   - 基礎をしっかり固めてから次に進みましょう

2. **実践を重視**
   - コード例は必ず自分で入力して実行してみる
   - 単にコピー&ペーストではなく、理解しながら書く
   - 練習問題に必ず取り組む

3. **復習を怠らない**
   - 理解が曖昧な部分は、前のセクションに戻って復習
   - 定期的に過去のコードを見直す
   - 各セクションの「まとめ」で重要ポイントを確認

4. **エラーから学ぶ**
   - エラーメッセージを注意深く読む習慣をつける
   - スタックトレースの読み方を学ぶ
   - よくあるエラーパターンを理解する

5. **独自の発展**
   - Section 6の総合演習では、自分なりのアレンジを加える
   - 学んだ内容を使って小さなプロジェクトを作る
   - 興味のある分野により深く取り組む

6. **コードレビュー**
   - 書いたコードを見直し、より良い書き方がないか考える
   - 命名規則、コードの可読性を意識する
   - リファクタリングの練習をする

### 学習スケジュール例

**集中学習プラン（2-3週間）**
- 週3-4日、1日2-3時間の学習
- 平日: 理論学習とコード練習
- 週末: 練習問題と復習

**通常学習プラン（1-2ヶ月）**
- 週2-3日、1日1-2時間の学習
- 着実に基礎を固めながら進める

## 必要な環境

### ソフトウェア要件

| 項目 | 要件 | 推奨 |
|------|------|------|
| **JDK** | JDK 17以降 | **JDK 21 LTS** |
| **IDE** | 以下のいずれか | IntelliJ IDEA Community |
| | • IntelliJ IDEA Community Edition | |
| | • VS Code + Extension Pack for Java | |
| | • Eclipse IDE for Java Developers | |
| **ビルドツール** | Maven 3.6+ または Gradle 7+ | Maven (Section 5で使用) |

### ハードウェア要件

| 項目 | 最小要件 | 推奨 |
|------|----------|------|
| **OS** | Windows 10、macOS 11、Ubuntu 18.04 | Windows 11、macOS 13、Ubuntu 22.04 |
| **メモリ** | 4GB | 8GB以上（IDE使用時は16GB） |
| **ディスク容量** | 2GB | 5GB以上 |
| **プロセッサ** | 2コア以上 | 4コア以上 |
| **インターネット** | 初回セットアップ時必須 | 常時接続推奨 |

### 前提知識

以下の知識があることを前提としています：

- **プログラミングの基礎**
  - 変数、データ型の概念
  - 関数・メソッドの理解
  - 条件分岐（if文など）とループ（for、whileなど）
  - 基本的なデータ構造（配列など）

- **開発環境の操作**
  - コマンドライン（ターミナル）の基本操作
  - テキストエディタまたはIDEの使用経験
  - ファイルシステムの基本理解

- **あると望ましい知識**（必須ではありません）
  - オブジェクト指向プログラミングの概念
  - バージョン管理（Git）の基本
  - 他のプログラミング言語の経験（Python、JavaScript、C++など）

## ドキュメントサイトの閲覧

このプロジェクトは[MkDocs](https://www.mkdocs.org/)と[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)を使用して、美しいドキュメントサイトとして公開されています。

### オンラインで閲覧

ドキュメントサイトはGitHub Pagesで自動公開されています：

**🔗 https://nakata5577.github.io/java-bootcamp/**

mainブランチへのプッシュ時に自動的にビルド＆デプロイされます。

### ローカルでの開発

#### セットアップ

```bash
# 依存関係のインストール
pip install -r requirements.txt
```

#### ローカルプレビュー

```bash
# 開発サーバーを起動（http://127.0.0.1:8000 で閲覧可能）
mkdocs serve
```

#### 静的サイトのビルド

```bash
# site/ ディレクトリに静的ファイルを生成
mkdocs build
```

## 学習のヒントと実践方法

### 効果的な学習テクニック

1. **アクティブラーニング**
   - ただ読むだけでなく、必ずコードを書いて実行する
   - 動作を予想してから実行し、結果を確認する
   - コードを少し変更して挙動の変化を観察する

2. **エラー駆動学習**
   - エラーが出ても慌てない - これは学習の機会
   - エラーメッセージを注意深く読み、何が問題かを理解する
   - スタックトレースの読み方を学ぶ
   - よくあるエラーパターンをメモしておく

3. **ドキュメント活用**
   - 公式ドキュメント（JavaDoc）を読む習慣をつける
   - 知らないクラスやメソッドに出会ったら、すぐに調べる
   - APIドキュメントの読み方に慣れる

4. **実践プロジェクト**
   - 学んだことを使って小さなプロジェクトを作る
   - 身近な問題を解決するツールを作ってみる
   - GitHubで公開してフィードバックを得る

5. **コードレビューの習慣**
   - 自分のコードを定期的に見直す
   - より良い書き方、より効率的な方法を探す
   - 他の人が書いたコード（オープンソースなど）を読む

### よくある質問（FAQ）

<details>
<summary><strong>Q: プログラミング初心者でも大丈夫ですか？</strong></summary>

A: このBootcampはプログラミング経験者向けの内容です。全くの初心者の方は、まず以下のような基本的なプログラミング概念を学ぶことをおすすめします：
- 変数とデータ型
- 条件分岐とループ
- 関数の概念
- 簡単なアルゴリズム

入門向けのリソース：
- [プログラミング入門](https://www.nhk.or.jp/school/programming/) - NHK for School
- [Progate](https://prog-8.com/) - 基礎から学べるオンライン学習サービス
</details>

<details>
<summary><strong>Q: どれくらいの期間で完了できますか？</strong></summary>

A: 学習ペースによって異なります：
- **集中学習**: 1日2-3時間 × 2-3週間 = 約30-40時間
- **通常ペース**: 週2-3日、1日1-2時間 × 1-2ヶ月
- **マイペース**: 週1-2日 × 3-4ヶ月

重要なのは、理解しながら進めることです。急がず、確実に理解してから次に進みましょう。
</details>

<details>
<summary><strong>Q: 練習問題はありますか？</strong></summary>

A: はい、各セクションに練習問題と実践例があります：
- 各章の最後に練習問題
- Section 6で総合演習プロジェクト
- コード例を改造して実験することも推奨

さらに練習したい方は、以下のリソースもおすすめです：
- [LeetCode](https://leetcode.com/) - アルゴリズム問題
- [HackerRank](https://www.hackerrank.com/domains/java) - Java特化の問題
- [Exercism](https://exercism.org/tracks/java) - メンター付き練習問題
</details>

<details>
<summary><strong>Q: つまずいたときはどうすればいいですか？</strong></summary>

A: 以下の手順で解決を試みましょう：
1. エラーメッセージをよく読む
2. 公式ドキュメントを確認する
3. Google検索で「Java [エラーメッセージ]」で検索
4. Stack Overflowで類似の質問を探す
5. GitHub Discussionsで質問する
6. コミュニティに質問する際は、具体的な問題とコードを示す
</details>

<details>
<summary><strong>Q: このBootcamp修了後、次に何を学べばいいですか？</strong></summary>

A: 修了後の学習パス：
- **Webアプリケーション開発**: Spring Boot、Spring Framework
- **データベース**: SQL、JPA/Hibernate
- **クラウド**: AWS、Azure、GCP
- **マイクロサービス**: Docker、Kubernetes
- **デザインパターン**: GoFパターン、アーキテクチャパターン
- **高度なトピック**: 並行処理、パフォーマンスチューニング
</details>

<details>
<summary><strong>Q: 認定資格は取るべきですか？</strong></summary>

A: Oracle Java認定資格（Oracle Certified Java Programmer）は、基礎知識の証明になります：
- **Bronze**: 入門レベル（日本独自）
- **Silver**: 基礎レベル - このBootcamp修了後に挑戦可能
- **Gold**: 中級レベル - 実務経験後に推奨

資格は必須ではありませんが、体系的な知識の確認には有用です。
</details>

## コントリビューション

改善提案やバグ報告は、GitHubのIssueまたはPull Requestでお願いします。

### 貢献方法

1. このリポジトリをフォーク
2. 新しいブランチを作成（`git checkout -b feature/improvement`）
3. 変更をコミット（`git commit -am 'Add improvement'`）
4. ブランチにプッシュ（`git push origin feature/improvement`）
5. Pull Requestを作成

## ライセンス

このプロジェクトは教育目的で作成されています。

## 関連リソース

### 公式ドキュメント

| リソース | 説明 | URL |
|---------|------|-----|
| **Oracle Java Documentation** | 公式Java開発者向けドキュメント | [docs.oracle.com/java](https://docs.oracle.com/en/java/) |
| **Java API Specification** | Java SE 21のAPIリファレンス | [Java SE 21 API](https://docs.oracle.com/en/java/javase/21/docs/api/) |
| **OpenJDK** | オープンソースJDK実装 | [openjdk.org](https://openjdk.org/) |
| **Java Language Specification** | Java言語仕様書 | [JLS](https://docs.oracle.com/javase/specs/) |
| **Java Tutorial（Oracle）** | 公式チュートリアル | [Oracle Java Tutorials](https://docs.oracle.com/javase/tutorial/) |

### おすすめの書籍

#### 初級〜中級

| 書籍名 | 著者 | 対象レベル | コメント |
|--------|------|-----------|----------|
| 『スッキリわかるJava入門 第4版』 | 中山清喬 | 初級 | 初学者に優しい解説 |
| 『Javaの絵本』 | アンク | 入門 | 図解で理解しやすい |
| 『新わかりやすいJava オブジェクト指向徹底解説』 | 川場隆 | 初級〜中級 | OOPを丁寧に解説 |

#### 中級〜上級

| 書籍名 | 著者 | 対象レベル | コメント |
|--------|------|-----------|----------|
| **『Effective Java 第3版』** | Joshua Bloch | 中級〜上級 | Java開発者必読の名著 |
| 『Java言語で学ぶデザインパターン入門』 | 結城浩 | 中級 | デザインパターンの定番書 |
| 『Java並行処理プログラミング』 | Brian Goetz | 上級 | 並行処理の決定版 |
| 『リファクタリング 第2版』 | Martin Fowler | 中級〜上級 | コード改善の技術 |

### オンラインリソース

#### 学習サイト

| サイト | 説明 | 難易度 |
|--------|------|--------|
| [Baeldung](https://www.baeldung.com/) | 高品質なJavaチュートリアルとガイド | 初級〜上級 |
| [JetBrains Academy](https://www.jetbrains.com/academy/) | インタラクティブなハンズオン学習 | 初級〜中級 |
| [Java Brains](https://javabrains.io/) | 分かりやすいビデオチュートリアル | 初級〜中級 |
| [Codecademy - Learn Java](https://www.codecademy.com/learn/learn-java) | インタラクティブなJava学習 | 初級 |

#### 練習・問題解決

| サイト | 説明 |
|--------|------|
| [LeetCode](https://leetcode.com/) | アルゴリズム・データ構造の問題練習 |
| [HackerRank - Java](https://www.hackerrank.com/domains/java) | Java特化の練習問題 |
| [Exercism - Java Track](https://exercism.org/tracks/java) | メンター付きの練習問題 |
| [CodingBat - Java](https://codingbat.com/java) | 初心者向けの練習問題 |

#### コミュニティ・Q&A

| サイト | 説明 |
|--------|------|
| [Stack Overflow - Java Tag](https://stackoverflow.com/questions/tagged/java) | 最大のプログラミングQ&Aサイト |
| [Reddit - r/java](https://www.reddit.com/r/java/) | Javaコミュニティのディスカッション |
| [dev.to - Java Tag](https://dev.to/t/java) | 技術記事とディスカッション |

#### ブログ・ニュース

| サイト | 説明 |
|--------|------|
| [Inside Java](https://inside.java/) | Oracle公式のJavaニュースとアップデート |
| [Java Weekly (Baeldung)](https://www.baeldung.com/java-weekly-briefing) | 週刊Javaニュースレター |
| [InfoQ - Java](https://www.infoq.com/java/) | エンタープライズJavaのニュースと記事 |

### YouTubeチャンネル

| チャンネル | 内容 |
|-----------|------|
| [Java Brains](https://www.youtube.com/@Java.Brains) | Java、Spring、マイクロサービスのチュートリアル |
| [Amigoscode](https://www.youtube.com/@amigoscode) | Java、Spring Boot、フルスタック開発 |
| [Programming with Mosh](https://www.youtube.com/@programmingwithmosh) | Java基礎から応用まで |

---

## コミュニティとサポート

このプロジェクトへの参加や質問は以下の方法で行えます：

- **GitHub Issues**: [バグ報告や機能提案](https://github.com/nakata5577/java-bootcamp/issues)
- **GitHub Discussions**: [学習に関する質問や議論](https://github.com/nakata5577/java-bootcamp/discussions)
- **GitHub Repository**: [nakata5577/java-bootcamp](https://github.com/nakata5577/java-bootcamp)

---

**Javaの世界へようこそ！楽しい学習をお祈りします。**
