# midnight-sample
midnightでの開発事前検証用リポジトリ

## 環境

- nodejs
- yarn
- docker
- compact CLI

## compact CLIのインストール

```bash
curl --proto '=https' --tlsv1.2 -LsSf https://github.com/midnightntwrk/compact/releases/latest/download/compact-installer.sh | sh
```

その後、以下でバージョン指定

```bash
compact update 0.25.0
```

インストールされているかの確認

```bash
compact --version
compact compile --version
```

それぞれ以下のようになればOK!

```bash
compact 0.2.0
0.25.0
```

## Testnet用のZKProof serverの起動

```bash
docker run -p 6300:6300 midnightnetwork/proof-server -- 'midnight-proof-server --network testnet'
```

```bash
docker ps
```

localhost:6300でサーバーが起動していればOK

```bash
CONTAINER ID   IMAGE                          COMMAND                  CREATED          STATUS          PORTS                                         NAMES
a62d9787f7a1   midnightnetwork/proof-server   "/nix/store/qa9fb15p…"   25 seconds ago   Up 24 seconds   0.0.0.0:6300->6300/tcp, [::]:6300->6300/tcp   flamboyant_roentgen
```

念の為以下のコマンドでも稼働確認

```bash
curl -X GET "http://localhost:6300"
```

```bash
We're alive 🎉!
```

## サンプルプログラムのコンパイル＆デプロイ手順

以下のコマンドは、 `my-mn-app` ディレクトリ配下で実行してください。

まず、依存関係をインストールする

```bash
yarn
```

以下のコマンドでビルドする

```bash
yarn build
```

以下のようになればOK!

```bash
Fetching public parameters for k=10 [====================] 192.38 KiB / 192.38 KiB
  circuit "increment" (k=10, rows=29)  
Overall progress [====================] 1/1   
```