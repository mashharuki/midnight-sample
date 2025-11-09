## タスク完了時のチェック
- 変更範囲に応じて `yarn contract test` や `yarn cli test-api` を実行しテスト結果を確認。
- CLIユーティリティを触った場合は `yarn cli build` でビルドが通るか確認。
- フォーマッタ/リンタは `yarn format` と `yarn biome:check`、必要ならパッケージごとの `npm run lint`。
- Midnight Testnet 関連を更新した場合は `yarn cli test-against-testnet` や `yarn cli testnet-remote` をドライランで確認。
- 変更点・既知の制約はAGENTSガイドラインに沿って記録。