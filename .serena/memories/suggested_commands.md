## よく使うコマンド

### セットアップ
- **依存関係のインストール:**
  `yarn`
- **Compact CLIのインストール/更新:** (初回のみ、または更新時)
  `curl --proto '=https' --tlsv1.2 -LsSf https://github.com/midnightntwrk/compact/releases/latest/download/compact-installer.sh | sh`
  `compact update 0.25.0`
- **ZKProofサーバーの起動 (Testnet用):**
  `docker run -p 6300:6300 midnightnetwork/proof-server -- 'midnight-proof-server --network testnet'`

### 開発 & ビルド
- **コントラクトのZKコンパイル:**
  `yarn contract compact`
- **CLI用のTypeScript APIを生成:**
  `yarn contract build`

### テスト
- **コントラクトのユニットテスト:**
  `yarn contract test`
- **CLIのAPIテスト (ローカル):**
  `yarn cli test-api`
- **CLIのAPIテスト (Testnet):**
  `yarn cli test-against-testnet`

### デプロイ & 実行
- **環境変数のセットアップ:** (初回デプロイ前)
  `cp pkgs/cli/.env.example pkgs/cli/.env`
- **コントラクトのデプロイ:**
  `yarn cli deploy`
- **`increment` メソッドの実行:**
  `yarn cli increment`
- **対話的なTestnet操作:**
  `yarn cli testnet-remote`

### 品質管理
- **コードのフォーマット:**
  `yarn format`
- **コードのチェック (Lint & フォーマット):**
  `yarn biome:check`
