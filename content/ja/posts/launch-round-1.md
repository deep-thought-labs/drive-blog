---
title: "Launch Round 1 - 最初のフロー"
date: 2025-12-25T14:00:00Z
draft: false
tags: ["testing", "gentx", "creative", "chain-launch", "round-1"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'Launch Round 1 - 最初のフロー'
---

{{< figure src="cover" caption="alt" >}}

これは**Infinite Improbability Drive**チェーンのローンチに向けた**最初のテストラウンド**です。このラウンドは本日、**2025年12月25日（木曜日）**に開始します。クリスマスが木曜日というのは、クリスマスだから、そして木曜日は変な日だからです。

このラウンドでは、チェーンの**Creativeネットワーク**で作業し、最終的なジェネシスに含まれるgentx（ジェネシストランザクション）を作成する手順を段階的に案内します。

> 📖 **テストフェーズのコンテキスト:** このラウンドは、12月20日に発表された[テストフェーズ：一般ガイド](/ja/posts/testing-phase-guide/)の一部です。まだ一般ガイドを読んでいない場合は、テストラウンドの完全なコンテキストを理解するために、確認することをお勧めします。

**Infinite Improbability Driveネットワークについて:**
- **Creative**: テストと開発に使用されるネットワーク（この最初のラウンド）
- **Testnet**: より広範なテストネットワーク
- **Mainnet**: メインの本番ネットワーク

Creativeネットワークは、最初のチェーンローンチテストラウンドに使用されます。このラウンドに参加できない場合でも、心配ありません。安定した結果を常に確保し、すべての人が参加する機会を持てるように、他のチェーンでこの同じ反復ラウンドを繰り返し実行します。

## 📅 参加期間

**公開日:** 2025年12月25日（木曜日）  
**提出期間:** 2025年12月25日から28日（4日間）

この4日間、参加者は以下の機会があります：

1. チームが提供するベースジェネシスをダウンロード
2. 具体的な手順に従ってgentxを作成
3. 開発チームにgentxファイルを提出

**12月28日以降**、開発チームは受け取ったすべてのgentxを最終ジェネシスにコンパイルし、すべての参加者に再配布して、チェーンのローンチを進めます。

---

## 🎯 このラウンドの目的

この最初のラウンドの目的は：

- **実際の環境でのgentx作成プロセス全体を検証**
- **公式ローンチ前にCreative Networkのインフラストラクチャをテスト**
- **すべての参加者がgentxを正しく作成して提出できることを確保**
- **すべての参加バリデーターを含む最終ジェネシスをコンパイル**

---

## 📋 前提条件

開始する前に、以下を確認してください：

- ✅ **Driveがインストールされ、設定されている**（[Driveのインストール](/ja/posts/drive-installation/)を参照）
- ✅ **Creativeサービスが設定されている**（`node2-infinite-creative`）
- ✅ **バリデーターのシードフレーズを使用したリカバリープロセスでノードが初期化されている**
- ✅ **ノードの初期化に使用したのと同じバリデーターのシードフレーズを使用してキーリングにキーが追加されている**
- ✅ **キーリングに追加したキーの名前を知っている**（キーを追加したときに選択した名前）
- ✅ **Driveが実行されているサーバーへのアクセス**

**⚠️ キーに関する重要事項:**
- バリデーターのシードフレーズを使用したリカバリープロセスでノードを初期化している必要があります
- その同じシードフレーズを特定の名前（例：`validator`、`my-validator`など）でキーリングにキーとして追加している必要があります
- **そのキーの名前を覚えて明確にしておく必要があります**。このプロセスのすべてのコマンドで必要になります
- このキー名は、`add-genesis-account`と`genesis gentx`コマンドで使用します

> 📖 **完全なドキュメント:** gentx作成プロセス全体を理解するには、[ドキュメントの完全なガイド](https://docs.infinitedrive.xyz/ja/blockchain/genesis/create-gentx/)を参照してください。

---

## 🚀 Creative向けの具体的な手順

### ステップ1: Creativeコンテナにアクセス

Creativeサービスディレクトリに移動し、コンテナのbashにアクセスします：

```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative bash
```

コンテナ内に入ると、`infinited`バイナリが直接利用可能になります。

---

### ステップ2: ベースジェネシスをダウンロード

コンテナ内で次のコマンドを実行して、Creative用のベースジェネシスをダウンロードします：

```bash
curl -o ~/.infinited/config/genesis.json https://assets.infinitedrive.xyz/tests-round1/genesis-base.json
```

**⚠️ 重要:**
- このコマンドは、ベースジェネシスを正しい場所（`~/.infinited/config/genesis.json`）に直接ダウンロードします
- 初期化中に生成された既存のジェネシスを置き換えます
- コマンドを実行する前に、コンテナ内にいることを確認してください

### ダウンロードしたジェネシスを確認

ジェネシスが正しくダウンロードされ、チェーンIDが正しいことを確認します：

```bash
cat ~/.infinited/config/genesis.json | jq -r '.chain_id'
```

**Creativeの期待されるチェーンID:** `infinite_421018002-1`

**注:** ダウンロードコマンドが変更された場合、開発チームが公式コミュニケーションチャネルを通じて通知します。

### ベースジェネシスを検証

続行する前に、ダウンロードしたベースジェネシスが正しいことを検証します：

```bash
infinited genesis validate-genesis --home ~/.infinited
```

検証が成功した場合、自信を持って続行できます。

---

### ステップ3: キーリングでキーを確認

続行する前に、キーリングにキーが存在することを確認し、その名前を覚えておきます：

```bash
infinited keys list --keyring-backend file --home ~/.infinited
```

このコマンドは、キーリングにあるすべてのキーを表示します。**バリデーターに対応するキーの名前を識別して記録してください**（バリデーターのシードフレーズを使用して追加したキー）。

**出力例:**
```
- name: validator
  type: local
  address: infinite1abc123...
  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"..."}'
```

この例では、キー名は`validator`です。**次のステップでこの同じ名前を使用します**。`validator`を実際のキー名に置き換えてください。

---

### ステップ4: ジェネシスにアカウントに資金を追加

**💡 ヒント:** コマンドを実行する前に、プレーンテキストエディタで準備すると、編集が簡単になります。これにより、コンソールにコピー＆ペーストする前に、コマンド全体（キー名を含む）を確認および編集できます。

初期残高でアカウントをジェネシスに追加します。**この最初のCreativeラウンドでは**、次の金額を使用します：

```bash
infinited genesis add-genesis-account <your-key-name> 1000000000000000000000cdrop \
  --keyring-backend file \
  --home ~/.infinited
```

**⚠️ 重要:** `<your-key-name>`を、ステップ3で確認したキーの正確な名前に置き換えてください（例：`validator`、`my-validator`など）。

**Creative向けの特定パラメータ:**
- 通貨単位: `cdrop`（Creative drop）
- 金額: `1000000000000000000000cdrop`（原子単位で1000トークン）

### アカウントが正しく追加されたことを確認

gentxを生成する前に、アカウントがジェネシスに正しく追加されたことを確認することをお勧めします。ジェネシスの内容を確認することでこれを行うことができます：

```bash
cat ~/.infinited/config/genesis.json | jq '.app_state.bank.balances'
```

このコマンドは、ジェネシス内のすべての残高を表示します。公開アドレス（キーをリストしたときに表示されたものと同じ）を探し、正しい金額があることを確認します。

**Creativeの期待される出力例:**
```json
[
  {
    "address": "infinite1rs3s0jx0rvnsjwfjch59lg9ypp6k3vmg2cn68j",
    "coins": [
      {
        "denom": "cdrop",
        "amount": "1000000000000000000000"
      }
    ]
  }
]
```

アカウントセクションでアカウント情報を確認することもできます：

```bash
cat ~/.infinited/config/genesis.json | jq '.app_state.auth.accounts'
```

**期待される出力例:**
```json
[
  {
    "@type": "/cosmos.auth.v1beta1.BaseAccount",
    "address": "infinite1rs3s0jx0rvnsjwfjch59lg9ypp6k3vmg2cn68j",
    "pub_key": null,
    "account_number": "0",
    "sequence": "0"
  }
]
```

**注:** 表示されている値は例です。Mainnetの通貨単位は`drop`、Testnetは`tdrop`、Creativeは`cdrop`になります。

アドレスに正しい金額が表示されている場合、自信を持ってgentxを生成できます。

---

### ステップ5: Gentxを生成

**💡 ヒント:** コマンドを実行する前に、プレーンテキストエディタで準備すると、編集が簡単になります。これにより、コンソールにコピー＆ペーストする前に、コマンド全体を確認および編集できます。

Creative向けの特定パラメータでgentxを生成します：

```bash
infinited genesis gentx <your-key-name> 10000000000000000000cdrop \
  --chain-id infinite_421018002-1 \
  --moniker "<your-moniker>" \
  --commission-rate "0.01" \
  --commission-max-rate "0.05" \
  --commission-max-change-rate "0.01" \
  --min-self-delegation "1000000000000000000" \
  --keyring-backend file \
  --home ~/.infinited
```

**⚠️ 重要:** 
- `<your-key-name>`を、ステップ3で確認したキーの正確な名前に置き換えてください
- `<your-moniker>`を、バリデーターに使用したい名前に置き換えてください（例：`"My Validator"`または`"validator-01"`）。これは、ネットワーク上でバリデーターを識別する公開名になります

**このラウンド向けの特定パラメータ:**
- **チェーンID:** `infinite_421018002-1`（Creative）
- **モニカー:** バリデーターの公開名（一意で説明的である必要があります）
- **自己委任:** `10000000000000000000cdrop`（10トークン）
- **初期手数料率:** `0.01`（1%）
- **最大手数料率:** `0.05`（5%）
- **最大レート変更:** `0.01`（1%）
- **最小自己委任:** `1000000000000000000`（1トークン）

---

### ステップ6: Gentxを検証

gentxを提出する前に、正しく機能することを検証します：

```bash
# gentxを収集（あなたのものを含む）
infinited genesis collect-gentxs --home ~/.infinited

# 結果のジェネシスを検証
infinited genesis validate-genesis --home ~/.infinited
```

検証が成功した場合、gentxは提出の準備ができています。

---

### ステップ7: Gentxファイルの場所を特定

gentxは、コンテナ内の次のディレクトリに生成されました：

```bash
~/.infinited/config/gentx/
```

gentxファイルには、一意のハッシュ形式があり、例：`gentx-adba573456c82908c3221163185703c421a2dd1f.json`

**⚠️ 重要:** ファイル名にはモニカーは含まれず、自動生成された一意のハッシュが含まれます。**このJSONファイルの名前を変更しないでください**。

ファイルの正確な名前を確認するには：

```bash
ls -la ~/.infinited/config/gentx/
```

`gentx-<hash>.json`形式のファイルが表示されます。次のステップのために、このファイルの完全な名前を記録してください。

---

### ステップ8: サーバーからGentxファイルを抽出

gentxファイルはDockerの永続ボリュームに保存されているため、手動でコピーする必要なく、ホストシステムからアクセスできます。

**ホストシステムから（コンテナの外）:**

1. **Creativeサービスディレクトリに移動:**
   ```bash
   cd services/node2-infinite-creative
   ```

2. **gentxファイルの場所を特定:**
   ```bash
   ls -la persistent-data/.infinited/config/gentx/
   ```

3. **元の名前を維持してgentxファイルをコピー:**
   
   **⚠️ 重要:** gentxファイルにはハッシュ形式の名前があります（例：`gentx-adba573456c82908c3221163185703c421a2dd1f.json`）。**このJSONファイルの名前を変更しないでください**。生成された元の名前を維持してください。
   
   ```bash
   # gentx用の特定ディレクトリを作成
   mkdir -p ~/gentx-round-1
   
   # 元の名前を維持してコピー（<hash>をファイルの実際のハッシュに置き換える）
   cp persistent-data/.infinited/config/gentx/gentx-<hash>.json ~/gentx-round-1/
   ```
   
   **例:** ファイル名が`gentx-adba573456c82908c3221163185703c421a2dd1f.json`の場合：
   ```bash
   mkdir -p ~/gentx-round-1
   cp persistent-data/.infinited/config/gentx/gentx-adba573456c82908c3221163185703c421a2dd1f.json ~/gentx-round-1/
   ```

**リモートサーバーにいる場合**、元の名前を維持してローカルコンピューターにダウンロードするために`scp`を使用できます：

```bash
# ローカルコンピューターから（<hash>をファイルの実際のハッシュに置き換える）
scp user@server:/path/to/drive/services/node2-infinite-creative/persistent-data/.infinited/config/gentx/gentx-<hash>.json ~/
```

**`scp`コマンドの説明:**
- `user`: サーバーにログインするために使用するユーザー名
- `@server`: サーバーのIPアドレスまたはドメイン名を指します（例：`@192.168.1.100`または`@my-server.com`）
- コロン（`:`）の後のパスは、サーバー上のファイルへの完全なパスです
- `~/`は、ローカルコンピューター上の宛先ディレクトリ（ホームディレクトリ）です

**⚠️ 重要:** このコマンドを実行すると、転送を実行するための認証情報または承認をシステムが要求する可能性が非常に高くなります。これらは、サーバーにログインするときに使用するのと同じ認証情報（パスワードまたはSSHキー）です。

**完全な例:** ユーザーが`ubuntu`で、サーバーのIPが`192.168.1.100`で、ファイル名が`gentx-adba573456c82908c3221163185703c421a2dd1f.json`の場合：
```bash
mkdir -p ~/gentx-round-1
scp ubuntu@192.168.1.100:/home/ubuntu/drive/services/node2-infinite-creative/persistent-data/.infinited/config/gentx/gentx-adba573456c82908c3221163185703c421a2dd1f.json ~/gentx-round-1/
```

---

### ステップ9: Gentxを圧縮して送信

**ファイルを圧縮:**

**⚠️ 重要:** 
- JSON gentxファイルは元の名前を維持する必要があります（ハッシュを含む、名前を変更しない）
- 圧縮ファイルには、識別を容易にするためにモニカーを含める必要があります

```bash
# モニカーを含む圧縮ファイルを作成（<moniker>を実際のモニカーに、<hash>をファイルのハッシュに置き換える）
tar -czf gentx-<moniker>-round-1.tar.gz gentx-<hash>.json

# またはzipを使用
zip gentx-<moniker>-round-1.zip gentx-<hash>.json
```

**例:** モニカーが`my-validator`で、ファイル名が`gentx-adba573456c82908c3221163185703c421a2dd1f.json`の場合：
```bash
cd ~/gentx-round-1
tar -czf gentx-my-validator-round-1.tar.gz gentx-adba573456c82908c3221163185703c421a2dd1f.json
```

**圧縮ファイルの構造:**
- **圧縮ファイル名:** `gentx-<your-moniker>-round-1.tar.gz`（識別のためにモニカーを含む）
- **圧縮ファイルの内容:** `gentx-<hash>.json`（元の名前の元のJSONファイル）

**Telegramで送信:**

圧縮ファイルを**Cypher Xenia**（ユーザー名**@XeniaCypher88**）にTelegramで直接送信します。

1. Telegramを開く
2. ユーザー**@XeniaCypher88**（Cypher Xenia）を検索
3. 圧縮ファイルを直接送信

**⚠️ 重要:**
- gentxファイルのみを送信し、**完全なジェネシスは送信しない**
- 送信する前に正しいファイルを送信していることを確認
- gentxのバックアップコピーを保持
- チームが要求した場合、メッセージにモニカーまたは識別子を含める

---

## 📝 追加情報

### 送信前にGentxを確認

gentxが正しいことを確認するために、gentxの内容を確認できます：

```bash
# コンテナから（<hash>をファイルのハッシュに置き換える）
cat ~/.infinited/config/gentx/gentx-<hash>.json | jq .
```

またはホストから：

```bash
# <hash>をファイルのハッシュに置き換える
cat services/node2-infinite-creative/persistent-data/.infinited/config/gentx/gentx-<hash>.json | jq .
```

以下を確認します：
- `chain_id`が`infinite_421018002-1`であること
- JSONコンテンツ内の`moniker`が正しいこと
- 金額とパラメータが指定どおりであること

---

## 🔄 提出後のプロセス

開発チームがすべてのgentxを受け取った後（12月28日まで）：

1. **最終ジェネシスのコンパイル:**
   - チームは受け取ったすべてのgentxを最終ジェネシスにコンパイルします
   - 最終ジェネシスが正しく完全であることが検証されます

2. **最終ジェネシスの再配布:**
   - すべての参加者は、コンパイルされた最終ジェネシスを受け取ります
   - ローカルジェネシスを置き換えるための手順が提供されます

3. **チェーンのローンチ:**
   - 全員が最終ジェネシスを取得したら、ローンチが進行します
   - バリデーターはブロック1からアクティブになります

---

## ❓ よくある質問

### gentxを時間内に提出しないとどうなりますか？

12月28日までにgentxを提出しない場合、Creativeの初期ローンチでバリデーターとして参加できません。ただし、他のチェーンでこの同じ反復ラウンドを繰り返し実行するため、将来参加する機会があります。

### gentxを送信した後、変更できますか？

いいえ、gentxを送信すると、変更できません。送信する前に正しく検証してください。

### gentxを送信する際にどのような情報を含める必要がありますか？

圧縮されたgentxファイルのみを送信する必要があります。開発チームが必要に応じて追加情報を要求する場合があります。

### gentxが正しく受信されたことをどのように確認できますか？

開発チームがgentxの受信を確認します。確認を受け取らない場合は、チームに連絡してください。

---

## 📚 参照

- [完全ガイド：Gentxを作成](https://docs.infinitedrive.xyz/ja/blockchain/genesis/create-gentx/) - 完全な技術ドキュメント
- [Driveのインストール](/ja/posts/drive-installation/) - Driveのインストール方法
- [バリデーターの準備](/ja/posts/validator-preparation/) - キー管理とセキュリティ

---

## 🎯 プロセスの概要

```
1. Creativeコンテナにアクセス
   ↓
2. ベースジェネシスをダウンロード
   ↓
3. ベースジェネシスを検証
   ↓
4. キーリングにキーが存在することを確認し、その名前を覚える
   ↓
5. キー名を使用してジェネシスに資金付きアカウントを追加（指定された金額）
   ↓
6. キー名を使用してCreative固有のパラメータでgentxを生成
   ↓
7. gentxとジェネシスを検証
   ↓
8. サーバーからコンピューターにgentxを抽出（元の名前を維持）
   ↓
9. 元の名前を維持して圧縮し、Telegramで送信
   ↓
10. 確認と最終ジェネシスを待つ
```

---

**この最初のテストラウンドをあなたと一緒に開始できることを楽しみにしています！** プロセス中に質問や問題がある場合は、開発チームに連絡することを躊躇しないでください。

**提出期間:** 2025年12月25日 - 28日  
**サービス:** Creative（node2-infinite-creative）  
**チェーンID:** `infinite_421018002-1`

---

<div style="border: 2px solid currentColor; border-left: 6px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px;">

## 🔄 更新: 最終ジェネシスと同期ローンチ

> **📅 これはgentx提出期間後の更新です。**

</div>

---

gentxファイルの提出期限が終了し、すべてのファイルが正常に収集されました。最終ジェネシスファイルは、このフェーズに参加するすべての新しいバリデーターに配布する準備が整いました。

### 最終ジェネシスのダウンロード

開発チームは、最終ジェネシスファイルをダウンロードして置き換えるスクリプトを提供します。このスクリプトは、受け取ったすべてのgentxでコンパイルされたジェネシスをダウンロードし、コンテナ内の正しい場所に配置します。

> **📝 注:** ダウンロードコマンドと最終ジェネシスファイルのURLはまだ定義されておらず、利用可能になったらこのドキュメントで更新されます。**以下の例のコマンドはまだ利用できないため、使用しないでください。**

**このフェーズの参加者向け:**

1. **Creativeコンテナにアクセス:**
   - Creativeサービスディレクトリに移動し、コンテナのbashにアクセスします（ステップ1で行ったように）：
   ```bash
   cd services/node2-infinite-creative
   ./drive.sh exec infinite-creative bash
   ```

2. **最終ジェネシスをダウンロード:**
   - コンテナ内に入ったら、開発チームが提供するコマンドを実行します（期待される形式の例）：
   ```bash
   curl -o ~/.infinited/config/genesis.json [URL_TO_BE_DEFINED]
   ```
   
   **⚠️ 重要:**
   - **このコマンドは形式の例にすぎません。** 実際のURLは利用可能になったら提供されます
   - **まだこのコマンドを実行しないでください**。URLはまだ利用できません
   - コマンドが利用可能になったら、最終ジェネシスを正しい場所（`~/.infinited/config/genesis.json`）に直接ダウンロードします
   - 既存のジェネシスをコンパイルされた最終ジェネシスに置き換えます
   - コマンドが利用可能になったら、実行する前にコンテナ内にいることを確認してください

3. **ダウンロードしたジェネシスを確認:**
   - ジェネシスが正しくダウンロードされ、チェーンIDが正しいことを確認します：
   ```bash
   cat ~/.infinited/config/genesis.json | jq -r '.chain_id'
   ```
   
   **Creativeの期待されるチェーンID:** `infinite_421018002-1`

4. **最終ジェネシスを検証:**
   - 続行する前に、最終ジェネシスが正しいことを検証します：
   ```bash
   infinited genesis validate-genesis --home ~/.infinited
   ```
   
   検証が成功した場合、自信を持って続行できます。

5. **ローンチの準備:**
   - 最終ジェネシスをダウンロードして検証したら、バリデーターノードを開始する準備が整います
   - すべてのバリデーターがノードを同期して開始すると、チェーンが誕生します

### ⏰ 同期ローンチ

**⚠️ 重要:** チェーンのローンチは**同期して**実行する必要があります。チェーンが正しく誕生するには、すべての参加者が同時にバリデーターノードを開始する必要があります。

> **📝 注:** スケジュールの具体的な時刻はまだ定義されておらず、確認されたらこのドキュメントで更新されます。

**ローンチスケジュール:**

1. **スクリプトの利用可能時刻:**
   - **時刻:** [定義予定] UTC
   - **その時点で**、すべての参加者が最終ジェネシスファイルをダウンロードして置き換えるスクリプトを実行できるようになります
   - その時点でコマンドを実行する準備ができているように、コンテナへのアクセスを確保してください

2. **ノード開始時刻:**
   - **時刻:** [定義予定] UTC（スクリプトの利用可能時刻の5分後）
   - これが**すべての参加者が同時にバリデーターノードを開始する必要がある**時点です
   - この同期は、チェーンの成功した誕生にとって重要です

**スケジュールの例（確認待ちの時刻）:**
- スクリプトが**[HH:MM] UTC**に利用可能な場合、その時点ですべての参加者が最終ジェネシスファイルをダウンロードするスクリプトを実行できるようになります
- **[HH:MM] UTC**（5分後）に、すべての参加者が同時にバリデーターノードを開始する必要があります

> **📢 注:** 正確な時刻は、このドキュメントで更新され、開発チームが公式コミュニケーションチャネルを通じて通知します。これらの発表に注意を払い、同期ローンチの準備をしてください。

---

**Creativeチェーンに命を吹き込む準備が整いました！** すべての準備が整い、同期ローンチの準備ができていることを確認してください。

---

<div style="border: 2px solid currentColor; border-left: 6px solid currentColor; padding: 15px; margin: 30px 0; border-radius: 4px;">

## 📢 更新: エラー検出と修正

> **📅 更新:** このラウンドのプロセス中、初期フローに根本的なエラーが検出され、修正が必要です。

プロセスフローでエラーが検出され、修正と継続の手順を含む別のドキュメントが作成されました。

**👉 [修正と継続の手順を表示 →](/ja/posts/launch-round-1-correction/)**

</div>
