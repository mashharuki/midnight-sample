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

以下のコマンドでコントラクトをビルドする

```bash
yarn contract compact
```

以下のようになればOK!

```bash
Fetching public parameters for k=10 [====================] 192.38 KiB / 192.38 KiB
  circuit "increment" (k=10, rows=29)  
Overall progress [====================] 1/1   
```

コントラクトのユニットテストコードを実行する

```bash
yarn contract test
```

以下のようになればOK!

```bash
 RUN  v4.0.8 /workspaces/midnight-sample/my-mn-app/pkgs/contract

 ✓ test/counter.test.ts (3 tests) 44ms
   ✓ Counter smart contract (3)
     ✓ generates initial ledger state deterministically 36ms
     ✓ properly initializes ledger state and private state 3ms
     ✓ increments the counter correctly 4ms

 Test Files  1 passed (1)
      Tests  3 passed (3)
   Start at  08:27:47
   Duration  421ms (transform 95ms, setup 0ms, collect 233ms, tests 44ms, environment 0ms, prepare 13ms)

JUNIT report written to /workspaces/midnight-sample/my-mn-app/pkgs/contract/reports/report.xml
Done in 1.34s.
```

次にCLI用のTypeScriptファイルを以下のコマンドで生成する

```bash
yarn contract build
```

そしてCLI用のユニットテストコードを実行する

```bash
yarn cli test-api
```

以下のようになればOK!

```bash
Test Files  1 passed (1)
      Tests  1 passed (1)
   Start at  08:41:12
   Duration  200.97s (transform 180ms, setup 72ms, collect 1.11s, tests 199.62s, environment 0ms, prepare 10ms)
```

さらにテストネット上でもユニットテストコードを実行するコマンド

```bash
yarn cli test-against-testnet
```

以下のようになればOK!

```bash
 ✓ src/test/counter.api.test.ts (1 test) 151857ms
   ✓ API (1)
     ✓ should deploy the contract and increment the counter [@slow]  125059ms

 Test Files  1 passed (1)
      Tests  1 passed (1)
   Start at  08:47:54
   Duration  153.65s (transform 205ms, setup 93ms, collect 1.56s, tests 151.86s, environment 0ms, prepare 8ms)
```

以下のコマンドでコントラクトをテストネットにデプロイする

```bash
yarn cli testnet-remote
```