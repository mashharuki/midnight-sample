## プロジェクト概要

このリポジトリは、ブロックチェーン技術「Midnight」とそのスマートコントラクト言語「Compact」に関する技術記事を執筆するための検証用プロジェクトです。

### 役割
- Geminiエージェント（伝説のエンジニアブロガー）が、高品質な技術ブログ記事を作成するためのベースとなります。
- 記事で紹介するコードの動作確認、コンパイル、テスト、デプロイ手順の検証を行います。

### 技術的特徴
- **ブロックチェーン**: [Midnight](https://midnight.network/) (Cardanoのプライバシー保護パートナーチェーン)
- **スマートコントラクト言語**: [Compact](https://github.com/midnightntwrk/compact) (TypeScriptベースのZK証明用DSL)
- **構成**: Yarn Workspacesを利用したNode.js + TypeScriptのモノレポです。
  - `pkgs/contract`: `Compact`で書かれたスマートコントラクト。
  - `pkgs/cli`: コントラクトをデプロイ・操作するためのCLIツール。
  - `pkgs/frontend`: 将来的なDAppフロントエンド用のプレースホルダー。
- **ツール**:
  - `compact CLI`: コントラクトのビルドとテスト。
  - `yarn`: パッケージ管理とスクリプト実行。
  - `@biomejs/biome`: コードのフォーマットと静的解析。
