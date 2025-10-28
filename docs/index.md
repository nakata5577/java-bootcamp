# Java Bootcamp

Javaを短期間で集中して学ぶための教科書プロジェクトです。

!!! info "ドキュメントサイトについて"
    このドキュメントは[MkDocs Material](https://squidfunk.github.io/mkdocs-material/)で構築され、GitHub Pagesで公開されています。

    **🔗 オンラインで閲覧**: [https://nakata5577.github.io/java-bootcamp/](https://nakata5577.github.io/java-bootcamp/)

## 対象者

プログラミング経験者を対象としています。基本的な文法は簡潔にまとめつつ、オブジェクト指向の概念や実践的な開発スキルに重点を置いた構成になっています。

**前提知識:**
- 基本的なプログラミング概念（変数、関数、条件分岐、ループなど）
- コマンドラインの基本操作
- テキストエディタまたはIDEの使用経験

## 目次

### Section 0: イントロダクション
Javaの世界へようこそ。まずは開発の準備を整え、最初のプログラムを動かしてみます。

- [0-1. Javaとは](section-0/0-1-what-is-java.md)
- [0-2. 開発環境の構築](section-0/0-2-setup.md)
- [0-3. Hello, World!](section-0/0-3-hello-world.md)
- [0-4. プロジェクトの基本構造](section-0/0-4-project-structure.md)

### Section 1: Javaの基本
プログラミングの基礎となる文法やデータ構造を学びます。

- [1-1. 変数とデータ型](section-1/1-1-variables-and-types.md)
- [1-2. 演算子](section-1/1-2-operators.md)
- [1-3. 制御フロー](section-1/1-3-control-flow.md)
- [1-4. 配列](section-1/1-4-arrays.md)

### Section 2: オブジェクト指向プログラミング (OOP)
Javaの核心であるオブジェクト指向の考え方をマスターします。

- [2-1. クラスとオブジェクト](section-2/2-1-classes-and-objects.md)
- [2-2. メソッドとコンストラクタ](section-2/2-2-methods-and-constructors.md)
- [2-3. カプセル化](section-2/2-3-encapsulation.md)
- [2-4. 継承](section-2/2-4-inheritance.md)
- [2-5. ポリモーフィズム（多態性）](section-2/2-5-polymorphism.md)
- [2-6. 抽象クラスとインターフェース](section-2/2-6-abstract-and-interface.md)

### Section 3: Javaの重要機能
より高度で実践的なアプリケーション開発に不可欠な機能を学びます。

- [3-1. 例外処理](section-3/3-1-exception-handling.md)
- [3-2. コレクションフレームワーク](section-3/3-2-collections.md)
- [3-3. ジェネリクス](section-3/3-3-generics.md)
- [3-4. ファイル入出力 (I/O)](section-3/3-4-file-io.md)

### Section 4: モダンJava
Java 8以降で導入された、より簡潔で強力なコーディングスタイルを習得します。

- [4-1. ラムダ式](section-4/4-1-lambda.md)
- [4-2. Stream API](section-4/4-2-stream-api.md)
- [4-3. Optional](section-4/4-3-optional.md)

### Section 5: 実践開発への一歩
実際の開発現場で使われるツールや技術に触れます。

- [5-1. ビルドツール入門](section-5/5-1-build-tools.md)
- [5-2. ユニットテスト](section-5/5-2-unit-testing.md)
- [5-3. データベース接続 (JDBC)](section-5/5-3-jdbc.md)

### Section 6: 総合演習
これまでの知識を総動員して、小規模なコンソールアプリケーションを設計・実装します。

- [6. 総合演習プロジェクト](section-6/6-final-project.md)

## 学習の進め方

1. **順序を守る**: 各セクションを順番に進めることをおすすめします
2. **実践を重視**: コード例は必ず実際に手を動かして試してみましょう
3. **復習を怠らない**: 理解が曖昧な部分は、前のセクションに戻って復習しましょう
4. **独自の発展**: Section 6の総合演習では、自分なりのアレンジを加えてみましょう
5. **エラーから学ぶ**: エラーが発生したら、エラーメッセージを読んで理解する習慣をつけましょう
6. **コードレビュー**: 書いたコードを見直し、より良い書き方がないか考えてみましょう

## 必要な環境

### 開発環境

- **JDK**: JDK 17以降（**推奨: JDK 21 LTS**）
- **IDE**: 以下のいずれか
    - IntelliJ IDEA Community Edition（推奨）
    - VS Code + Extension Pack for Java
    - Eclipse IDE for Java Developers
- **所要時間**: 1日2-3時間の学習で2-3週間程度

!!! tip "推奨学習環境"
    - **OS**: Windows 10/11、macOS 12以降、Ubuntu 20.04以降
    - **メモリ**: 8GB以上（IDE使用時は16GB推奨）
    - **ディスク空き容量**: 5GB以上
    - **インターネット接続**: 依存関係のダウンロードに必要

## 学習のヒント

### 効果的な学習方法

- **アクティブラーニング**: ただ読むだけでなく、コードを書いて実行してみる
- **エラーハンドリング**: エラーが出ても慌てず、エラーメッセージを注意深く読む
- **ドキュメント参照**: 公式ドキュメント（JavaDoc）を読む習慣をつける
- **実践プロジェクト**: 学んだことを使って小さなプロジェクトを作ってみる

### よくある質問

**Q: プログラミング初心者でも大丈夫ですか？**
A: このBootcampはプログラミング経験者向けです。全くの初心者の方は、まず基本的なプログラミング概念を学ぶことをおすすめします。

**Q: どれくらいの期間で完了できますか？**
A: 個人差がありますが、1日2-3時間の学習で2-3週間程度が目安です。

**Q: 練習問題はありますか？**
A: 各セクションに実践例があり、Section 6で総合演習プロジェクトに取り組めます。

## 学習の進捗管理

各セクションを完了したら、チェックリストで進捗を確認しましょう：

- [ ] Section 0: イントロダクション
- [ ] Section 1: Javaの基本
- [ ] Section 2: オブジェクト指向プログラミング
- [ ] Section 3: Javaの重要機能
- [ ] Section 4: モダンJava
- [ ] Section 5: 実践開発への一歩
- [ ] Section 6: 総合演習

## コミュニティとサポート

質問や議論は以下で行えます：

- **GitHub Issues**: バグ報告や機能提案
- **GitHub Discussions**: 学習に関する質問や議論
- **GitHub Repository**: [nakata5577/java-bootcamp](https://github.com/nakata5577/java-bootcamp)

## ドキュメントサイトの開発

このドキュメントサイトは[MkDocs](https://www.mkdocs.org/)と[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)を使用しています。

### ローカルでのプレビュー

```bash
# 依存関係のインストール
pip install -r requirements.txt

# 開発サーバーを起動（http://127.0.0.1:8000）
mkdocs serve

# 静的サイトのビルド
mkdocs build
```

### GitHub Pagesへのデプロイ

mainブランチへのプッシュ時に、GitHub Actionsによって自動的にビルド＆デプロイされます。

## コントリビューション

改善提案やバグ報告を歓迎します。

### 貢献方法

1. このリポジトリをフォーク
2. 新しいブランチを作成（`git checkout -b feature/improvement`）
3. 変更をコミット（`git commit -am 'Add improvement'`）
4. ブランチにプッシュ（`git push origin feature/improvement`）
5. Pull Requestを作成

## 関連リソース

### 公式ドキュメント

- [Oracle Java Documentation](https://docs.oracle.com/en/java/)
- [OpenJDK](https://openjdk.org/)
- [Java API Specification](https://docs.oracle.com/en/java/javase/21/docs/api/)

### おすすめの書籍

- 『Effective Java 第3版』Joshua Bloch著
- 『Java言語で学ぶデザインパターン入門』結城浩著
- 『スッキリわかるJava入門』中山清喬著

### オンラインリソース

- [Baeldung](https://www.baeldung.com/) - Javaチュートリアル
- [JetBrains Academy](https://www.jetbrains.com/academy/) - インタラクティブ学習
- [Java Brains](https://javabrains.io/) - ビデオチュートリアル
- [Stack Overflow](https://stackoverflow.com/questions/tagged/java) - Q&Aコミュニティ

## ライセンス

このプロジェクトは教育目的で作成されています。

---

!!! quote "学習のモチベーション"
    「プログラミングを学ぶことは、考え方を学ぶことである」

**では、Javaの世界へ飛び込みましょう！** 🚀
