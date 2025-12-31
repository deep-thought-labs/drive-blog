---
title: "Launch Round 1 - ローンチ日の準備"
date: 2025-12-30T00:00:00Z
draft: false
tags: ["testing", "gentx", "creative", "chain-launch", "round-1", "preparation", "launch-day"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'Launch Round 1 - ローンチ日の準備'
---

{{< figure src="cover" caption="alt" >}}

このドキュメントには、**Infinite Improbability Drive**チェーンのラウンド1のローンチ日に必要な**最終準備手順**とすべての情報が含まれています。

> 📖 **コンテキスト:** このドキュメントはラウンド1プロセスの継続です。以前の手順を完了していることを確認してください：
> - [元のラウンド1](/ja/posts/launch-round-1/) - 初期プロセス
> - [ラウンド1の修正](/ja/posts/launch-round-1-correction/) - 編集されたGenesisファイルの送信

---

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px; background-color: rgba(0, 200, 0, 0.1);">

## ✅ ステータス: ローンチの準備完了

**すべての参加者が準備フェーズを正常に完了しました：**

- ✅ すべての参加者がノード情報（IP、サブドメイン、Node ID）を送信
- ✅ チームがCloudflareですべてのセキュアドメインを設定
- ✅ チームがすべての有効なgentxで最終Genesisをコンパイル
- ✅ シードノードと永続ピアの値が準備完了
- ✅ Genesisダウンロードスクリプトが利用可能

**🎯 残っているのは、ノードで最終設定を完了し、合意された同期ローンチの時間に開始する準備をすることだけです。**

</div>

---

## 📋 参加者に要求された情報（完了）

準備プロセスの一環として、すべての参加者に次の情報の提供と特定の設定の完了が求められました：

### 参加者が送信した情報

1. **サーバーIP** - ノードがホストされているサーバーのIPアドレス
2. **希望するサブドメイン名** - 形式: `server-あなたの名前`（例: `server-red`）
3. **Node ID** - コマンド `infinited comet show-node-id` を使用して取得

**✅ すべての参加者がこの情報の送信を正常に完了しました。**

### 要求された設定

1. **ファイアウォール設定** - サーバーを保護し、必要な接続を許可するため
2. **Node IDの取得** - ネットワーク設定のための一意のノード識別子

**✅ すべての参加者がこれらの設定を正常に完了しました。**

---

## 🔧 ノードで実行する必要がある設定

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px; background-color: rgba(255, 193, 7, 0.1);">

### ⚠️ 重要: 設定を確認

**参加者はローンチ日前にこれらの設定を完了している必要があります。**

まだ完了していない場合は、**完了し、正しいことを確認してください**。このガイドで説明されているように、すべての設定値が正しいことを確認してください。

</div>

### 🔴 ファイルを変更するための重要なルール

**⚠️ 重要 - 続行する前にこれを読んでください：**

1. **`config.toml`を変更する場合：**
   - ✅ **サービスを停止する必要はありません** - サービスが実行中でもファイルを編集できます
   - ✅ 編集中もサービスを実行し続けることができます

2. **`docker-compose.yml`を変更する場合：**
   - ⛔ **ファイルを編集する前にサービスを停止する必要があります**
   - ✅ ファイルを編集して変更を保存
   - ✅ 変更を保存した**後**にサービスを起動する必要があります

3. **コンテナ内での操作（bash）の場合：**
   - ✅ **サービスが実行中である必要があります** - コンテナ内で操作を行う前に停止しないでください
   - ✅ `./drive.sh exec infinite-creative bash` を使用してコンテナにアクセス

---

### ステップ1: ファイアウォール設定

他の設定を続行する前に、サーバーを保護し、必要な接続を許可するためにファイアウォールを正しく設定することが**重要**です。

**🔴 重要 🔴** 
ファイアウォールを有効にする前に、まず**SSHポート（22）を許可**してください。そうしないと、サーバーコンソールへのアクセスを失います。

次の順序でこれらのガイドに従ってください：

1. **一般的なファイアウォール設定**
   - [ファイアウォール設定ガイド](https://docs.infinitedrive.xyz/ja/drive/services/ports/firewall-configuration/)

2. **Infinite Creative Network（node2-infinite-creative）の特定設定**
   - [node2-infinite-creativeのファイアウォール設定](https://docs.infinitedrive.xyz/ja/drive/services/catalog/node2-infinite-creative/#firewall-configuration)

---

### ステップ2: config.tomlを変更

**✅ 重要:** `config.toml`を変更する場合、**サービスを停止する必要はありません**。サービスが実行中でもファイルを編集できます。

**💻 ホストマシンで、infinite-creativeサービスフォルダ内にいることを確認してください：**
```bash
cd drive/services/node2-infinite-creative
```

**Nanoで設定ファイルを開いて編集します：**
```bash
nano persistent-data/config/config.toml
```

設定ファイルに2つの重要な変更を加える必要があります。

#### 2.1: RPCアクセスのためにladdrを設定

この設定により、サーバー内だけでなく、WebからノードのRPCデータを確認できるようになります。基本的な検証に非常に便利です。

`[rpc]`セクションを見つけて、`laddr`を`0.0.0.0`をアドレスとして使用するように設定します。ポート`26657`で問題ありません。

**値を次のように編集します：**
```toml
##########################################
###  RPC Server Configuration Options  ###
##########################################
[rpc]

# TCP or UNIX socket address for the RPC server to listen on
laddr = "tcp://0.0.0.0:26657"
```

#### 2.2: PEXを無効化

同じ設定ファイルで、`[p2p]`セクションに移動し、`pex`オプションを見つけます。`false`に設定する必要があります。

これは、小さなチェーン（私たちの場合など）での切断の問題を回避します。

**値を次のように設定します：**
```toml
###################################
###  P2P Configuration Options  ###
###################################
[p2p]
...
pex = false
```

**Nanoで変更を保存するには：**
- `Ctrl + O`を押して保存
- `Enter`を押して確認
- `Ctrl + X`を押して終了

**✅ これらの変更が保存されると、ノードが起動したとき（またはノードが既に実行中の場合、停止して再起動したとき）に有効になります。** **ノード**（コンテナ内で実行されるバイナリ）と**コンテナ**（Dockerサービス）を区別することが重要です。この場合、`config.toml`ファイルに行っているすべての変更は、ノードが停止している状態で行われています。これは、ノードが合意されたローンチ時間まで起動しないためです。ただし、将来的に何らかの理由で`config.toml`ファイルを変更したい場合は、変更を有効にするために**ノードを停止して再起動する必要があります**。

---

## 📋 準備完了の設定値

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px; background-color: rgba(0, 123, 255, 0.1);">

### 🔗 シードノードと永続ピア

`NODE_P2P_SEEDS`と`NODE_PERSISTENT_PEERS`の値は、**公式サービスドキュメント**で準備完了し、利用可能です。すべての参加者が情報を提供し、これらの値は正しく設定されています。

**📍 ドキュメント内の場所：**

完全な値は、公式ドキュメントの**「Network P2P Configuration」**セクションで利用可能です：

**[ネットワークP2P設定 - Infinite Creative Network](https://docs.infinitedrive.xyz/en/drive/services/catalog/node2-infinite-creative/#network-p2p-configuration)**

このセクションでは以下を見つけることができます：
- すべての信頼できるノードの完全なテーブル（Node ID、アドレス、ポート）
- コピーする準備ができた`NODE_P2P_SEEDS`の完全な値
- コピーする準備ができた`NODE_PERSISTENT_PEERS`の完全な値
- 各設定変数に関する詳細情報

**💡 値を取得する方法：**

1. [公式ドキュメント](https://docs.infinitedrive.xyz/en/drive/services/catalog/node2-infinite-creative/#network-p2p-configuration)にアクセス
2. **「Network P2P Configuration」**セクションを見つける
3. **「Network P2P Configuration Variables」**セクションを展開
4. `NODE_P2P_SEEDS`と`NODE_PERSISTENT_PEERS`の完全な値をコピー
5. これらの値を`docker-compose.yml`ファイルに貼り付け

**⚠️ 注意:** `NODE_P2P_SEEDS`と`NODE_PERSISTENT_PEERS`の値は同じです。両方の変数に同じ値をコピーしてください。

### 📥 最終Genesis URL

最終Genesisは次のURLで利用可能です：

```
https://assets.infinitedrive.xyz/tests-round1/genesis-final.json
```

**期待されるChain ID:** `infinite_421018002-1`

</div>

---

### ステップ3: docker-compose.ymlを設定

**⛔ 重要:** `docker-compose.yml`を変更する場合、**ファイルを編集する前にサービスを停止する必要があります**。

#### 3.1: サービスを停止

**まず、infinite-creativeサービスを停止します：**

```bash
cd drive/services/node2-infinite-creative
./drive.sh down
```

#### 3.2: docker-compose.ymlファイルを開く

**Nanoでdocker-composeファイルを開いて編集します：**
```bash
nano docker-compose.yml
```

#### 3.3: NODE_P2P_EXTERNAL_ADDRESSを設定

これは、各参加者が選択したサブドメインに基づいて以前に各参加者に配信されたアドレスです。ポートは、このクリエイティブネットワークに対応するポート（26676）です。

P2Pネットワーク設定セクションで`NODE_P2P_EXTERNAL_ADDRESS`変数を見つけ、サブドメインとポートで値を設定します：

**次の形式である必要があります：**
```
あなたのサブドメイン.infinitedrive.xyz:26676
```

**⚠️ 重要:** `NODE_P2P_EXTERNAL_ADDRESS`行がdocker-compose.ymlファイルで**コメントアウトされていない**ことを確認してください。行がコメントアウトされている場合（先頭に`#`がある場合）、値を変更する前にコメントを解除してください。値を変更するだけで行がコメントアウトされたままの場合、効果はありません。

**docker-compose.ymlでの表示例：**
```yaml
###############################
#  Network P2P Configuration  #
###############################

NODE_P2P_EXTERNAL_ADDRESS: "server-red.infinitedrive.xyz:26676"
# NODE_P2P_SEEDS: ""
# NODE_PERSISTENT_PEERS: ""
```

#### 3.4: NODE_P2P_SEEDSとNODE_PERSISTENT_PEERSを設定

両方の変数（`NODE_P2P_SEEDS`と`NODE_PERSISTENT_PEERS`）は、公式ドキュメントの同じ場所にあります。その場所で、docker-compose.ymlファイルで置き換えるために各フィールドをコピーして貼り付けることができます。

**まず、ドキュメントから値を取得します：**

1. [公式ドキュメント - Network P2P Configuration](https://docs.infinitedrive.xyz/en/drive/services/catalog/node2-infinite-creative/#network-p2p-configuration)にアクセス
2. **「Network P2P Configuration Variables」**セクションを展開
3. `NODE_P2P_SEEDS`の完全な値をコピー
4. `NODE_PERSISTENT_PEERS`の完全な値をコピー

**⚠️ 重要:** 両方の変数が同じ値を持っていますが、**それぞれを個別にコピーして貼り付ける必要があります**。これにより、docker-compose.ymlファイルでそれぞれが正しいキー値を持つようになります。

**次に、docker-compose.ymlファイルで：**

1. `NODE_P2P_SEEDS`変数を見つけて、ドキュメントからコピーした値を貼り付け
2. `NODE_PERSISTENT_PEERS`変数を見つけて、ドキュメントからコピーした値を貼り付け

**⚠️ 重要:** `NODE_P2P_SEEDS`と`NODE_PERSISTENT_PEERS`行がdocker-compose.ymlファイルで**コメントアウトされていない**ことを確認してください。行がコメントアウトされている場合（先頭に`#`がある場合）、値を変更する前にコメントを解除してください。値を変更するだけで行がコメントアウトされたままの場合、効果はありません。

**docker-compose.ymlでの表示例：**
```yaml
###############################
#  Network P2P Configuration  #
###############################

NODE_P2P_EXTERNAL_ADDRESS: "server-red.infinitedrive.xyz:26676"
NODE_P2P_SEEDS: "dd5689375610aaa35b69ed311d69e51ea5557474@server-red.infinitedrive.xyz:26676,e5c1b7423d098c660bb82b7f44f86e333cb6af9e@server-farmer.infinitedrive.xyz:26676,..."
NODE_PERSISTENT_PEERS: "dd5689375610aaa35b69ed311d69e51ea5557474@server-red.infinitedrive.xyz:26676,e5c1b7423d098c660bb82b7f44f86e333cb6af9e@server-farmer.infinitedrive.xyz:26676,..."
```

**注意:** 例の値は形式を示すために`...`で切り詰められています。完全な実際の値は公式ドキュメントで見つけることができます。

**Nanoで変更を保存するには：**
- `Ctrl + O`を押して保存
- `Enter`を押して確認
- `Ctrl + X`を押して終了

#### 3.6: サービスを起動

**変更を保存した後、サービスを再度起動します：**

```bash
./drive.sh up -d
```

**✅ サービスは、正しいシードノードと永続ピアの値で設定されました。**

---

### ステップ4: 最終Genesisをダウンロードして置き換え

最終Genesisは準備完了し、利用可能です。ローンチ前にダウンロードして、現在のGenesisファイルを置き換える必要があります。

**✅ 重要:** コンテナのbashにアクセスするには、サービスが実行中である必要があります。サービスを停止する必要はありません。

#### 4.1: コンテナBashにアクセス

コンテナbashにアクセスします：

```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative bash
```

#### 4.2: 最終Genesisをダウンロード

コンテナ内で、最終Genesisをダウンロードします：

```bash
# 最終Genesisをダウンロード（「準備完了の設定値」セクションのURLを使用）
curl -o ~/.infinited/config/genesis.json https://assets.infinitedrive.xyz/tests-round1/genesis-final.json
```

#### 4.3: ダウンロードしたGenesisを確認

コンテナ内で、Genesisが正しくダウンロードされたことを確認します：

```bash
# Chain IDを確認
cat ~/.infinited/config/genesis.json | jq -r '.chain_id'

# Creativeの期待されるChain ID: infinite_421018002-1

# Genesisを検証
infinited genesis validate-genesis --home ~/.infinited
```

検証が成功した場合、Genesisは準備完了です。

**コンテナから退出：**
```bash
exit
```

**✅ ノードは公式の最終Genesisで設定されました。**

---

## 📝 設定手順の要約

```
ローンチの最終設定:
1. ファイアウォール設定を確認 ✅
   ↓
2. config.tomlを変更 ⬅️ サービスを停止する必要はありません
   (RPC laddrとpex = false)
   ↓
3. サービスを停止 ⛔
   ↓
4. docker-compose.ymlを変更 ⬅️ サービスを停止する必要があります
   (P2P外部アドレス、シードノード、永続ピア)
   ↓
5. サービスを起動 ✅
   ↓
6. コンテナbashにアクセス ✅
   (サービス実行中)
   ↓
7. コンテナ内で最終Genesisをダウンロード ⬅️ サービス実行中である必要があります
   ↓
8. コンテナ内でダウンロードしたGenesisを確認
   ↓
9. コンテナから退出
   ↓
10. ノードを開始する合意された時間を待つ！ 🚀
```

---

## ⏰ ローンチの同期

### 🎉 特別なラウンド1ローンチ

このラウンド1の初期ローンチは、テストチェーンであるため、この歴史的な瞬間を記念するために特別な方法で行うことにしました。

**📅 ローンチ日時：**

**2026年1月1日 0:00 UTC-0** - 新年の最初の分、年の最初の木曜日。

**⏰ ローンチに関する重要な情報：**

- 参加者の100%が正確に同時にノードを開始することは**重要でも必須でもありません**
- その時点で機会がある参加者はそうすべきです
- その正確な時点でできない参加者は、その時間後のいつでも行うことができます
- ノードはネットワークと同期し、正常に検証を開始します

**🚀 ノードを開始するには：**

ノードを開始するには2つのオプションがあります：

**オプション1: グラフィカルインターフェースを使用（推奨）**

1. グラフィカルインターフェースを開く（[グラフィカルインターフェース](https://docs.infinitedrive.xyz/ja/drive/guides/blockchain-nodes/graphical-interface/)を参照）
2. ナビゲート: メインメニュー → **「Node Operations」** → **「Start Node」**
3. インターフェースは起動プロセスを表示し、ノードが実行されているときに確認します

![ノードを開始](https://docs.infinitedrive.xyz/images/node-ui-operations-op1-start.png)

**オプション2: コマンドラインを使用**

```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-start
```

**📖 ノードの開始と停止の方法の詳細については、[完全なノードの開始/停止ガイド](https://docs.infinitedrive.xyz/ja/drive/guides/blockchain-nodes/start-stop-node/)を参照してください。**

---

### 📊 ノードログを表示

ノードを開始した後、すべてが正しく機能していることを確認し、同期の進行を観察するためにログを監視することが重要です。

**ログを表示するには2つの方法があります：**

1. **リアルタイムでログを表示** - ノードが実行中にノードのアクティビティを監視するため
2. **最後のN行のログを表示** - エラーが発生し、ノードの開始から何が起こったかを確認したい場合に便利

**グラフィカルインターフェース経由でログオプションにアクセス：**

1. グラフィカルインターフェースを開く（[グラフィカルインターフェース](https://docs.infinitedrive.xyz/ja/drive/guides/blockchain-nodes/graphical-interface/)を参照）
2. ナビゲート: メインメニュー → **「Node Monitoring」**

   ![ノード監視メニュー](https://docs.infinitedrive.xyz/images/node-ui-monitoring.png)

3. **「Node Monitoring」**サブメニューで、ログを表示するオプションを見つけることができます

---

#### リアルタイムでログを表示

**オプション1: グラフィカルインターフェースを使用（推奨）**

**「Node Monitoring」**サブメニューで、**「Follow Logs」**を選択します。ログはリアルタイムで表示を開始します。フォローを停止するには、`Ctrl+C`を押します。

**オプション2: コマンドラインを使用**

```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-logs -f
```

または代替構文を使用：
```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-logs --follow
```

**機能:** ログファイルに書き込まれると同時にログエントリをリアルタイムでストリームします（`tail -f`と同様）。

**期待される出力:** ログエントリの連続ストリームを表示します。停止するには`Ctrl+C`を押します。

**使用する場合:** ノードが実行中にノードのアクティビティを監視し、リアルタイムで同期の進行を観察するか、問題が発生したときにデバッグする場合。

---

#### 最後のN行のログを表示

このオプションは、何らかの理由でエラーが発生し、何が起こったか、またはノードの開始からのログを確認したい場合に便利です。ノード操作の開始から確認するには、特に実行中にエラーがあった場合、最後の200行を表示することをお勧めします。最初の200行で何が起こったかを確認できるためです。

**最後のN行のログを表示するには2つのオプションがあります：**

**オプション1: グラフィカルインターフェースを使用**

**「Node Monitoring」**サブメニューで、**「View Logs」**を選択します。インターフェースはデフォルトで最後の50行のログを表示します。

**注意:** 特定の行数（たとえば、100行または200行）を表示する必要がある場合は、コマンドライン（オプション2）を使用する必要があります。

**オプション2: コマンドラインを使用**

最後の50行を表示するには（デフォルト）：
```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-logs
```

最後の100行を表示するには：
```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-logs 100
```

最後の200行を表示するには（ノードの開始から確認するために推奨）：
```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-logs 200
```

**機能:** ノードログファイル（`/var/log/node/node.log`）の最後のN行を表示します。

**期待される出力:** 最近のログエントリを表示：
- ノードの起動メッセージ
- 同期の進行
- ブロックの処理
- エラーまたは警告
- 接続ステータス

**使用する場合:** 最近のノードアクティビティを確認し、実行の開始からエラーを確認するか、リアルタイムでログを表示する必要がない場合に同期の進行を確認する場合。

**📖 ノード監視の詳細については、[完全なノード監視ガイド](https://docs.infinitedrive.xyz/ja/drive/guides/blockchain-nodes/node-monitoring/)を参照してください。**

### 📅 今後のローンチ

今後のローンチ（TestnetとMainnet）では、実行された分析に従って、参加者の最大可用性の時間である**午後4:00 - 午後4:10（UTC-0）**の推奨スケジュールが使用されます。

**✅ ローンチ時間前にすべての設定手順を完了していることを確認してください。**

---

## 🔐 セキュリティ: セキュアドメインの使用

このプロセスを安全にするために、**IPアドレスを直接共有しません**。代わりに、**Cloudflare経由のセキュアドメイン**を使用します。

**IPアドレス`123.456.789`を使用する代わりに、次を使用します：**
```
server-red.infinitedrive.xyz
```

このように、設定ファイルでそれぞれのシードノードを定義する際に、次のような形式を使用します：

```
node_id@server-red.infinitedrive.xyz:26656
```

これにより以下が提供されます：
- ✅ より高いセキュリティ（IPを直接公開しない）
- ✅ 柔軟性（設定を更新せずにIPを簡単に変更）
- ✅ より良いネットワーク管理

---

## ❓ よくある質問

### ローンチ前にこれらの手順を完了しないとどうなりますか？

これらの手順を完了しない場合、ノードはネットワーク上の他のノードと正しく接続できず、コンセンサスとローンチへの参加が妨げられます。合意された時間前にシードノード、永続ピア、および最終Genesisの設定を完了することが**重要**です。

### 合意された時間より前にノードを開始できますか？

いいえ。成功した同期ローンチを確保するために、すべてのノードが合意された時間に同時に開始することが重要です。

### 合意されたローンチ時間の後にノードを開始できますか？

はい。合意された時間（2026年1月1日 0:00 UTC-0）に正確にノードを開始できない場合、その時間後のいつでも開始できます。ノードは自動的にネットワークと同期し、同期が完了すると正常に検証を開始します。

### config.tomlを変更するためにサービスを停止する必要がありますか？

いいえ。サービスが実行中でも`config.toml`を変更できます。ただし、**ノード**（コンテナ内で実行されるバイナリ）と**コンテナ**（Dockerサービス）を区別することが重要です。`config.toml`への変更は、ノードが起動したとき、またはノードが既に実行中の場合に停止して再起動したときに有効になります。この場合、すべての変更はノードが停止している状態で行われています。これは、ノードが合意されたローンチ時間まで起動しないためです。将来的に何らかの理由で`config.toml`ファイルを変更したい場合は、変更を有効にするために**ノードを停止して再起動する必要があります**。

### docker-compose.ymlを変更するためにサービスを停止する必要がありますか？

はい。`docker-compose.yml`を変更する**前にサービスを停止する必要があります**。変更を保存した**後にサービスを起動する必要があります**。

### サービスが停止している状態でコンテナ内で操作できますか？

いいえ。コンテナbashにアクセスしてコンテナ内で操作を行うには、**サービスが実行中である必要があります**。サービスが実行中の場合、`./drive.sh exec infinite-creative bash`を使用してください。

### サブドメインの代わりにIPを直接使用できますか？

はい、技術的にはIPアドレスを直接使用することは可能です。ただし、セキュリティと柔軟性の理由から、**表示しないことを推奨します**。このため、すべての参加者は、チームがCloudflare経由で設定したサブドメインを使用する必要があります。

---

## 📚 技術参照

- [完全ガイド: Gentxを作成](https://docs.infinitedrive.xyz/ja/blockchain/genesis/create-gentx/) - 完全な技術ドキュメント
- [ファイアウォール設定](https://docs.infinitedrive.xyz/ja/drive/services/ports/firewall-configuration/) - 一般的なファイアウォールガイド
- [Infinite Creative Network - サービスドキュメント](https://docs.infinitedrive.xyz/ja/drive/services/catalog/node2-infinite-creative/) - 完全なサービスドキュメント
- [Network P2P Configuration](https://docs.infinitedrive.xyz/en/drive/services/catalog/node2-infinite-creative/#network-p2p-configuration) - シードノードと永続ピアの値（公式ドキュメント）

---

## 🔗 関連ドキュメント - ラウンド1

このセクションには、ラウンド1に関連するすべてのドキュメントが時系列順に含まれています：

- **[テストフェーズガイド](/ja/posts/testing-phase-guide/)** - テストフェーズの一般的なガイド（開始点）
- **[Launch Round 1](/ja/posts/launch-round-1/)** - 初期フローを含む元のラウンド1ドキュメント
- **[Launch Round 1 - 修正](/ja/posts/launch-round-1-correction/)** - 修正プロセスと編集されたGenesisの送信

---

**準備と協力に感謝します！** すべての参加者が準備フェーズを正常に完了しました。残っているのは、最終設定（シードノード、永続ピア、および最終Genesis）を完了し、合意された同期ローンチ時間にノードを開始する準備をすることだけです。

**🎯 覚えておいてください：** ローンチの成功は、すべてのノードが合意された時間に同時に開始することに依存します。すべて準備を整えましょう！

