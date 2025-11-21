# 役割

あなたは、Qiita や Zenn などの著名な技術ブログプラットフォームで数々の技術記事をトレンド入りさせてきた、伝説のエンジニアブロガーです。

## 全体的な執筆方針

- 多く人が理解できるように、わかりやすくて簡潔な日本語で執筆すること
- 記事を執筆するときは過去に執筆したブログの内容も参考にできるようにプロジェクトをアクティベートさせること
- 専門用語については説明のために注釈を加えること
- 執筆の際、以下の5つの観点を重点的に考慮してください
	- 論理性
	- 実用性
	- 読みやすさ
	- 独自性
	- 明確性

## 記事作成のワークフロー

### Step 1: テーマのヒアリングと情報収集
- テーマの確認とターゲット読者の設定
- 情報源の取得と分析

### Step 2: タイトル、絵文字、スラッグの提案  
- 魅力的なタイトル案を3〜5個提案
- 各案に対応する絵文字とスラッグも併せて提案

### Step 3: 記事本文の構想案の検討・執筆
- 記事の目的を達成するために最適な記事の構成案を検討
- Zenn Flavored Markdown 形式で生成
- 導入、本文、結論の3部構成

### Step 4: Zenn用CLIコマンドの生成
- 記事ファイル作成のためのCLIコマンドを実行

## 【参考情報】 midnightとcompactについて

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

以下のコマンドでコントラクトを対話式でテストネットにデプロイする

```bash
yarn cli testnet-remote
```

単にデプロイだけ行いたい場合は以下を実行する

事前に環境変数を設定する

```bash
cp .env.example .env
```

```bash
yarn cli deploy
```

以下のようになればOK!

```bash
[12:16:24.603] INFO (39506): Deploying counter contract...
[12:17:27.488] INFO (39506): Deployed contract at address: 020050e6bdae4c9e65023a252a6aba74323c1d9c1ba6e520f00e84a5fc1c75b100f3
[12:17:27.488] INFO (39506): Deployment transaction: 00000000c408a293e4e287285649623774b2be950bf0d385a20117ce79a99eb7315aa547
[12:17:27.489] INFO (39506): Contract address: 020050e6bdae4c9e65023a252a6aba74323c1d9c1ba6e520f00e84a5fc1c75b100f3
Counter contract deployed at: 020050e6bdae4c9e65023a252a6aba74323c1d9c1ba6e520f00e84a5fc1c75b100f3
[12:17:27.489] INFO (39506): Not saving cache as sync cache was not defined
Done in 90.16s.
```

デプロイされたコントラクトアドレスを環境変数に追加する

incrementメソッドだけを呼び出すコマンドを実行する

```bash
yarn cli increment
```

以下のようになればOK!

```bash
[12:33:37.176] INFO (47085): Incrementing...
[12:34:34.270] INFO (47085): Transaction 000000000202acbcd05e9f19e5144acc5f97953255840b8b932fc71b84520e715b7ca900 added in block 2485067
[12:34:34.271] INFO (47085): Increment transaction: 000000000202acbcd05e9f19e5144acc5f97953255840b8b932fc71b84520e715b7ca900 (block 2485067)
Counter incremented. txId=000000000202acbcd05e9f19e5144acc5f97953255840b8b932fc71b84520e715b7ca900 block=2485067
[12:34:34.271] INFO (47085): Checking contract ledger state...
[12:34:34.462] INFO (47085): Ledger state: 1
[12:34:34.463] INFO (47085): Current counter value: 1
[12:34:34.463] INFO (47085): Current counter value: 1
Current counter value: 1
[12:34:34.463] INFO (47085): Not saving cache as sync cache was not defined
Done in 128.20s.
```

## 参考文献
- [ブロックエクスプローラー](https://www.midnightexplorer.com/)

## はじめに：ブロックチェーンの「透明すぎる」問題

**Bitcoin**のホワイトペーパーが公開されてから17年...。

ブロックチェーンが持つ耐改ざん性と透明性は取引の信頼性を担保する画期的な仕組みとして称賛されてきました。

誰もが台帳を検証できることで、改ざん不可能なトラスト（信頼）を築き上げたのです。

しかし、その「完全な透明性」はビジネスや個人のプライバシーが関わる領域において、障壁も生み出しています。  

特にエンタープライズ企業ではこの問題が顕著に見られ、それゆえにプライベートチェーンを構築する動きが流行りました。

- **あなたの銀行取引履歴が、世界中の誰からでも見えてしまったら？**
- **企業の極秘のサプライチェーン情報が、競合他社に筒抜けだったら？**
- **個人の医療情報や投票履歴が、公開されてしまったら？**

考えただけでも恐ろしいですよね。

この「透明すぎる」問題こそが、ブロックチェーン技術が真に社会の隅々まで浸透するのを阻んできた要因の一つにもなっていると思います。

なんでもかんでもオープンに...というわけにはいきませんよね。

そんな中このジレンマを解決すべく、一筋の光が差し込んできました。

それが、今回ご紹介する**midnight**です。

https://midnight.network/

Midnightは、データ保護とプライバシーに特化した、Cardanoのパートナーチェーン（サイドチェーン）です。

この記事では、midnightの世界に飛び込みその革新的な技術と未来の可能性を理解できるようにストーリー仕立てで分かりやすく解説しています！

ぜひ最後まで読んでいってください！

## 第1章：midnightとは？ - プライバシーを守る新世代ブロックチェーン

midnightは、一言で言えば **「選択的な開示（Selective Disclosure）」** を可能にする、データ保護を目的としたブロックチェーンです。

従来の多くのブロックチェーンが「すべてを公開する」しかなかったのに対し、Midnightは **「証明したい事実だけを、それ以外の情報を一切明かさずに証明する」** ことを可能にします。

:::message
**zcash**のような類似のアプローチを採用しているブロックチェーンも存在します。
:::

これを実現するのが、 **ゼロ知識証明（Zero-Knowledge Proofs, ZKP）** という非常に高度な暗号技術です。

```mermaid
graph TD
    subgraph 従来のブロックチェーン
        A[取引データ] --> B{すべて公開};
    end
    subgraph Midnight ゼロ知識証明
        C[取引データ] --> D{必要な情報だけを証明};
        E[個人情報など] --> F(完全に秘匿);
    end
```

**ゼロ知識証明（Zero-Knowledge Proofs, ZKP）** については別記事で紹介していますのでぜひそちらもご覧になってください！

https://zenn.dev/mashharuki/articles/zk_groth16-plonk

### midnightが実現しようとしていること

Midnightの使命は、データ保護、所有権、そしてデータの有用性の間で発生していたトレードオフを解消することです。

- **ユーザーは…** 
  自分のデータを完全にコントロールし、誰に何をどこまで見せるかを自分で決められる。
- **開発者や企業は…**  
  ユーザーのプライバシーを侵害するリスクを負うことなく、データを活用した革新的なサービスを構築できる。

これにより、これまでブロックチェーン上では不可能だった、機密性が求められる領域でのユースケースが増加する可能性があります。

### midnightの心臓部：アーキテクチャとコンセンサス

Midnightは、Cardanoのパートナーチェーンとして、その堅牢なセキュリティインフラの上に構築されています。

- **アーキテクチャ**: 
  Midnightは、スマートコントラクトの状態を2つに分割します。
    1.  **パブリックな状態**: 
      従来通りブロックチェーン上で公開されるデータ。
    2.  **プライベートな状態**:  
      各ユーザーがオフチェーン（自分のローカル環境）で管理するデータ。

- **Kachinaプロトコル**: 
  このパブリックとプライベートの状態を安全に連携させるのが、研究主導の統一フレームワーク「Kachina」です。
  ユーザーはプライベートなデータを使い、ローカルでゼロ知識証明を生成。その「証明」だけをブロックチェーンに送信することで、データの正しさを検証します。
- **コンセンサスアルゴリズム**: 
  Midnightは、**AURA**（ブロック生成）と**GRANDPA**（ファイナリティ）を組み合わせたハイブリッドコンセンサスを採用しており、Cardanoのステークプールオペレーター（SPO）がブロックプロデューサーとして参加することで、分散性とセキュリティを確保しています。

### 2つのネイティブトークン：NIGHT と DUST

Midnightは、ユニークなデュアルトークンモデルを採用しています。

| トークン | 役割                                                                                             | 特徴                                                               |
| :------- | :----------------------------------------------------------------------------------------------- | :----------------------------------------------------------------- |
| **NIGHT**  | ガバナンス、コンセンサス参加、ブロック生成報酬                                                     | **非シールドトークン**。取引所での取引が可能で、ネットワークの安全性を担保します。 |
| **DUST**   | トランザクション手数料（ガス代）                                                                 | **シールドリソース**。転送不可能で、プライバシーを保護します。価格変動が少なく、予測可能な手数料を実現します。 |

`DUST`は「燃料」の役割を果たしますが、それ自体が取引される資産ではないため、トランザクションのメタデータ（誰が誰にいくら払ったか）のプライバシーが守られ、かつ安定した手数料でサービスを運用できるのです。

## 第2章：Cardanoとの絆 - なぜパートナーチェーンなのか？

**midnight**を語る上で欠かせないのが、その親チェーンである**Cardano**との関係です。

**midnight**は独立したブロックチェーンでありながら、**Cardano**と深く連携することで、その真価を発揮します。

```mermaid
graph BT
    subgraph Cardano Ecosystem
        C[Cardano Mainnet]
        SPOs[ステークプール<br/>オペレーター]
    end
    subgraph Midnight Network
        M[Midnight Chain]
        BPs[ブロックプロデューサー]
    end

    C -- セキュリティ基盤を提供 --> M
    SPOs -- が参加 --> BPs
    M -- パートナーチェーン<br/>として連携 --> C
```

### セキュリティの継承

新しいブロックチェーンが直面する最大の課題の一つが、ネットワークのセキュリティの担保です。

midnightは、この課題を「Cardanoのパートナーチェーンになる」という方法で解決しました。

- **セキュリティのブートストラップ**: 
  Midnightは、Cardanoの巨大で分散化されたステークプールオペレーター（SPO）のネットワークを活用します。  
  Midnightのブロックプロデューサーになるには、まずCardanoのSPOである必要があります。

- **信頼のインフラ**: 
  これにより、Midnightはローンチ直後から、世界中に分散した信頼性の高いエンタープライズグレードのインフラを手に入れることができるのです。

### Cardanoとの違い

改めて midnightとCardanoの違いをまとめたいと思います！

- **Cardano**: 
  価値の保存と移転、そして安全な分散型アプリケーションのための「決済レイヤー」および「汎用プラットフォーム」としての役割に重点を置いています。
- **midnight**: 
  Cardanoのセキュリティを基盤としながら、**データ保護とプライバシー**という特定の機能に特化した「計算レイヤー」です。

両者は競合するのではなく、互いの強みを活かし合う補完的な関係なのです。Cardanoが信頼の土台を築き、その上でMidnightがプライバシーを必要とする複雑なアプリケーションを動かします。

## 第3章：Compact言語 - TypeScriptで書けるZKPスマートコントラクト

:::message
「ゼロ知識証明なんて、一部の暗号学者にしか扱えないのでは？」
:::

そう思った方も多いでしょう。

しかし、Midnightはそのハードルを劇的に下げるための切り札を用意しています。  
それが、スマートコントラクトのための新しいプログラミング言語 **`Compact`** です。

https://github.com/midnightntwrk/compact

### Compactの驚くべき特徴

1.  **TypeScriptベースのDSL**: 
  Compactは、世界で最も人気のあるプログラミング言語の一つであるTypeScriptをベースにしたドメイン固有言語（DSL）です。これにより、膨大な数のWeb開発者が、特別な言語を学ぶことなく、慣れ親しんだ構文でプライバシー保護アプリケーションを開発できます。

2.  **ZKの複雑さを抽象化**: 
  開発者は、ゼロ知識証明の複雑な数学や暗号理論を意識する必要がありません。Compactコンパイラが、書かれたロジックを自動的にZK証明の生成に必要な暗号マテリアルに変換してくれます。

3.  **プライバシー・バイ・デザイン（Privacy by Design）**: 
  Compactは、「うっかり情報漏洩」を防ぐための賢い仕組みを持っています。
    - データは**デフォルトでプライベート**として扱われます。
    - プライベートなデータを公開状態にするには、`disclose()`という関数で明示的に囲む必要があります。
    - これを忘れると、コンパイラがエラーを出してくれるため、意図しないデータ漏洩を未然に防げます。

```typescript
// Compactの概念的なコード例

// プライベートなユーザーの投票内容
witness userVote: private Field;

// パブリックな開票結果
let results: public Field;

// 投票回路
circuit vote() {
  // ... 投票の正当性を検証するロジック ...

  // 検証が通れば、結果を更新する
  // この時、誰が何に投票したかは秘匿される
  results = results + 1;

  // もし投票内容を公開したい場合は、明示的にdisclose()を使う
  // disclose(userVote);
}
```

### 開発ワークフロー

開発者は、DAppのフロントエンドやビジネスロジックをTypeScript/JavaScriptで書き、コアとなるプライベートなロジックをCompactで記述します。

Compactコンパイラが両者を繋ぐためのTypeScript APIを自動生成するため、シームレスな開発体験が実現されます。

## 第4章：Midnightが拓くユースケース

プライバシーが保護されることで、どのような未来が実現するのでしょうか？  
**midnight** が可能にするユースケースは無限大です。

- **デジタルID / KYC**: 
  「18歳以上である」ことを証明するために、生年月日を提示する必要がなくなります。  
  年齢や国籍といった事実だけを、個人情報を一切明かすことなく証明できます。

- **匿名投票**: 
  誰が誰に投票したかを秘密にしたまま、有権者であることの証明と投票の正当性を担保した、真に公正な投票システムを構築できます。

- **ヘルスケア**: 
  個人の医療記録のプライバシーを完全に保護しながら、研究機関が統計データとして活用したり、AIが創薬のための学習を行ったりすることが可能になります。

- **DeFi（分散型金融）**: 
  あなたの金融資産や取引戦略を他人に知られることなく、分散型金融サービスを利用できます。

- **AIとLLM**: 
  機密性の高い企業データや個人データをAIに学習させる際、データのプライバシーを維持したまま、モデルのトレーニングを行うことができます。

## まとめ：プライバシー新時代の幕開け

最後に要点をまとめたいと思います！

- ✅**midnight**は、ゼロ知識証明という暗号技術を使ってこれまでブロックチェーンが抱えていた「透明すぎる」という根本的な課題を解決しようとしている。
- ✅**Cardano**との戦略的パートナーシップにより、ローンチ当初から堅牢なセキュリティと分散性を確保。
- ✅**Compact言語**とTypeScriptエコシステムの融合により、世界中の開発者がプライバシー保護アプリケーションを構築する道を開きました。

Midnightは単なる新しいブロックチェーンではありません。  

それは、個人のデータ主権を守り、企業が安心してイノベーションを追求できる、より公正で安全なデジタル社会を実現するための基盤技術です。

もっとmidnightのことが知りたいという方はぜひ公式ドキュメントをチェックしてみてはいかがでしょうか？

ここまで読んでいただきありがとうございました！！

## 開発者向けドキュメントの紹介

midnight上で何かアプリを作りたいという方は以下のサイトが参考になります！

https://midnight.network/developer-hub

https://github.com/midnightntwrk/midnight-awesome-dapps

https://github.com/MeshJS/midnight-starter-template

https://github.com/DEGAorg/midnight-mcp

Deepwikiでも聞けるようにしてみました！

https://deepwiki.com/midnightntwrk/midnight-zk

https://deepwiki.com/midnightntwrk/midnight-docs

https://deepwiki.com/midnightntwrk/compact

https://deepwiki.com/midnightntwrk/midnight-js

https://deepwiki.com/midnightntwrk/midnight-awesome-dapps

https://deepwiki.com/DEGAorg/midnight-mcp

## midnight summit 2025

2025年11月にロンドンにて midnight summit 2025が開催されます！  

最新情報は以下のサイトをチェックしてみてください！

https://midnightsummit.io/

## 参考文献

- [Midnight公式サイト](https://www.midnight.network/)
- [Midnightドキュメント](https://docs.midnight.network/)