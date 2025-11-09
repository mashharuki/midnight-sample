## コーディングスタイルと規約
- TypeScript/ESMベース。`tsconfig.json` でESM指定、ビルドは `tsconfig.build.json`。
- Lintは各パッケージの `eslint.config.mjs`、フォーマットはリポジトリルートの Biome (`yarn format`) を使用。
- コメントは必要最小限で「なぜ」を説明し、DRYと一貫性を重視（AGENTSガイドライン準拠）。
- 外部APIやネットワーク処理は失敗前提で例外処理・リトライを意識。