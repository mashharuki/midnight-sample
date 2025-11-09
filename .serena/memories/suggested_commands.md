## よく使うコマンド
- 依存関係インストール: `yarn`
- Compact CLI 更新/確認: `compact update 0.25.0`、`compact --version`
- コントラクト関連: `yarn contract compact` (ZKコンパイル)、`yarn contract test`、`yarn contract build`
- CLI関連: `yarn cli build`、`yarn cli test-api`、`yarn cli test-against-testnet`、`yarn cli testnet-remote`
- Docker証明サーバー: `docker run -p 6300:6300 midnightnetwork/proof-server -- 'midnight-proof-server --network testnet'`
- Lint/フォーマット: `yarn format`、`yarn biome:check`
- 型チェック: `yarn cli typecheck`
- スタンドアロン起動: `yarn cli standalone`