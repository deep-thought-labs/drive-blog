---
title: "Launch Round 1 - Gentxを再度作成"
date: 2025-12-29T05:00:00Z
draft: false
tags: ["testing", "gentx", "creative", "chain-launch", "round-1", "v2"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'Launch Round 1 - Gentxを再度作成'
---

{{< figure src="cover" caption="alt" >}}

このドキュメントには、ベースジェネシスv2を使用して**gentxを再度作成する手順**が含まれています。これは、**Infinite Improbability Drive**チェーンのローンチに向けたラウンド1の修正されたプロセスの一部です。

> 📖 **コンテキスト:** このドキュメントは[ラウンド1の修正](/ja/posts/launch-round-1-correction/)の継続です。これらの手順に従う前に、フェーズ1（編集したジェネシスファイルの送信）を完了したことを確認してください。

---

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px;">

## ⚠️ 重要: 元のラウンド1参加者のみ

**このドキュメントとここで説明する手順は、元のラウンド1期間（2025年12月25日〜28日）に既に参加し、修正のフェーズ1（編集したジェネシスファイルの送信）を既に完了した参加者のみを対象としています。**

- ✅ フェーズ1で編集したジェネシスファイルを既に送信した場合は、これらの手順に従う必要があります
- ❌ 元の期間に参加していない、またはフェーズ1を完了していない場合、このドキュメントは適用されません

</div>

---

## 📋 ベースジェネシスv2でGentxを再度作成

チームは受信したすべての個別ジェネシスファイルをコンパイルし、最初からすべての参加者のアカウントと残高を含む**ベースジェネシスv2**を作成しました。

ベースジェネシスv2に基づいて新しいgentxを作成するには、これらの手順に従う必要があります。

### ステップ1: 以前のGentxを削除

新しいベースジェネシスv2をダウンロードする前に、以前に作成したgentxを削除する必要があります。この以前のgentxは使用されません。以前のジェネシスで作成されたため、新しい修正されたジェネシスファイル（ベースジェネシスv2）を使用して新しいgentxを作成する必要があります。

関連するすべてのファイルを確実に削除するために、個別のファイルを検索して削除するのではなく、**gentxフォルダ全体を削除することをお勧めします**。これにより、新しいgentxを作成するためのクリーンな状態が保証されます。

**⚠️ 重要な警告:** これらのコマンドを実行する際は、十分に注意してください。`gentx/`フォルダ**のみ**を削除し、完全な`config/`フォルダやその他のシステムフォルダなどの他の重要なフォルダを誤って削除しないようにしてください。コマンドを実行する前に、パスを慎重に確認してください。

**コンテナ内:**
```bash
# コンテナにアクセス
cd services/node2-infinite-creative
./drive.sh exec infinite-creative bash

# gentxフォルダ全体を削除
rm -rf ~/.infinited/config/gentx/
```

**ホストシステムから:**
```bash
# gentxフォルダ全体を削除
rm -rf services/node2-infinite-creative/persistent-data/config/gentx/
```

### ステップ2: ベースジェネシスv2をダウンロード

チームは、最初からすべての参加者のアカウントと残高を含むベースジェネシスv2をコンパイルしました。

**コンテナ内:**
```bash
# コンテナにアクセス
cd services/node2-infinite-creative
./drive.sh exec infinite-creative bash

# ベースジェネシスv2をダウンロード
curl -o ~/.infinited/config/genesis.json https://assets.infinitedrive.xyz/tests-round1/genesis-base-v2.json
```

**⚠️ 重要:**
- このベースジェネシスv2には、すべての参加者のアカウントと残高が含まれます
- 設定内の既存のジェネシスを置き換えます

### ステップ3: ベースジェネシスv2を確認

ベースジェネシスv2をダウンロードしたら：

1. **チェーンIDを確認:**
   ```bash
   cat ~/.infinited/config/genesis.json | jq -r '.chain_id'
   ```
   
   **Creativeの期待されるチェーンID:** `infinite_421018002-1`

2. **アカウントが含まれていることを確認:**
   ```bash
   # キーをリストしてアドレスを取得
   infinited keys list --keyring-backend file --home ~/.infinited
   
   # アドレスがジェネシスに含まれていることを確認（YOUR_ADDRESSを実際のアドレスに置き換える）
   cat ~/.infinited/config/genesis.json | jq '.app_state.bank.balances[] | select(.address == "YOUR_ADDRESS")'
   ```

3. **ベースジェネシスv2を検証:**
   ```bash
   infinited genesis validate-genesis --home ~/.infinited
   ```
   
   検証が成功した場合、自信を持って続行できます。

### ステップ4: 新しいGentxを生成

**重要:** この修正されたプロセスでは、**ジェネシスにアカウントを追加する必要はありません**。ベースジェネシスv2に既に含まれています。

**💡 ヒント:** コマンドを実行する前に、プレーンテキストエディタで準備すると、編集が簡単になります。これにより、コンソールにコピー＆ペーストする前に、コマンド全体（キー名を含む）を確認および編集できます。

gentxを直接生成するだけです：

```bash
infinited genesis gentx <your-key-name> 10000000000000000000cdrop \
  --chain-id infinite_421018002-1 \
  --commission-rate "0.01" \
  --commission-max-rate "0.05" \
  --commission-max-change-rate "0.01" \
  --min-self-delegation "1000000000000000000" \
  --keyring-backend file \
  --home ~/.infinited
```

**⚠️ 重要:** `<your-key-name>`を、以前に確認したキーの正確な名前に置き換えてください。より簡単なプロセスのために、プレーンテキストファイルで編集または対応する編集を行ってから、コマンドをターミナルにコピー＆ペーストすることをお勧めします。

**Creative向けの特定パラメータ:**
- **チェーンID:** `infinite_421018002-1`（Creative）
- **自己委任:** `10000000000000000000cdrop`（10トークン）
- **初期手数料率:** `0.01`（1%）
- **最大手数料率:** `0.05`（5%）
- **最大レート変更:** `0.01`（1%）
- **最小自己委任:** `1000000000000000000`（1トークン）

### ステップ5: 新しいGentxを検証

gentxを提出する前に、正しく機能することを検証します：

```bash
# gentxを収集（あなたのものを含む）
infinited genesis collect-gentxs --home ~/.infinited

# 結果のジェネシスを検証
infinited genesis validate-genesis --home ~/.infinited
```

検証が成功した場合、gentxは提出の準備ができています。

### ステップ6: 新しいGentxを抽出して送信

1. **gentxファイルの場所を特定:**
   ```bash
   ls -la ~/.infinited/config/gentx/
   ```
   
   `gentx-<hash>.json`形式のファイルが表示されます。このファイルの完全な名前を記録してください。

2. **サーバーからgentxファイルを抽出:**
   
   gentxファイルはDockerの永続ボリュームに保存されているため、手動でコピーする必要なく、ホストシステムからアクセスできます。
   
   **サーバーで直接作業している場合:**
   ```bash
   cd services/node2-infinite-creative
   ls -la persistent-data/config/gentx/
   
   # 元の名前を維持してコピー（<hash>をファイルの実際のハッシュに置き換える）
   mkdir -p ~/gentx-round-1-v2
   cp persistent-data/config/gentx/gentx-<hash>.json ~/gentx-round-1-v2/
   ```
   
   **リモートサーバーにいる場合**、`scp`を使用してファイルをローカルコンピューターにダウンロードする必要があります：
   ```bash
   # ローカルコンピューターから（<user>、<server>、<hash>、パスを設定に応じて置き換える）
   mkdir -p ~/gentx-round-1-v2
   scp <user>@<server>:/path/to/drive/services/node2-infinite-creative/persistent-data/config/gentx/gentx-<hash>.json ~/gentx-round-1-v2/
   ```
   
   **`scp`コマンドの説明:**
   - `<user>`: サーバーにログインするために使用するユーザー名
   - `<server>`: サーバーのIPアドレスまたはドメイン名を指します（例：`192.168.1.100`または`my-server.com`）
   - コロン（`:`）の後のパスは、サーバー上のファイルへの完全なパスです
   - `~/gentx-round-1-v2/`は、ローカルコンピューター上の宛先ディレクトリです
   
   **⚠️ 重要:** このコマンドを実行すると、転送を実行するための認証情報または承認をシステムが要求する可能性が非常に高くなります。これらは、サーバーにログインするときに使用するのと同じ認証情報（パスワードまたはSSHキー）です。
   
   **完全な例:** ユーザーが`ubuntu`で、サーバーのIPが`192.168.1.100`で、作業ディレクトリが`/home/ubuntu/drive`で、ファイル名が`gentx-adba573456c82908c3221163185703c421a2dd1f.json`の場合：
   ```bash
   mkdir -p ~/gentx-round-1-v2
   scp ubuntu@192.168.1.100:/home/ubuntu/drive/services/node2-infinite-creative/persistent-data/config/gentx/gentx-adba573456c82908c3221163185703c421a2dd1f.json ~/gentx-round-1-v2/
   ```

3. **gentxファイルを圧縮:**
   ローカルコンピューターにgentxファイルを取得したら、識別を容易にするためにモニカーを含む名前で圧縮します：
   
   ```bash
   cd ~/gentx-round-1-v2
   tar -czf gentx-<your-moniker>-round-1-v2.tar.gz gentx-<hash>.json
   
   # またはzipを使用
   zip gentx-<your-moniker>-round-1-v2.zip gentx-<hash>.json
   ```
   
   **例:** モニカーが`my-validator`で、ファイル名が`gentx-adba573456c82908c3221163185703c421a2dd1f.json`の場合：
   ```bash
   cd ~/gentx-round-1-v2
   tar -czf gentx-my-validator-round-1-v2.tar.gz gentx-adba573456c82908c3221163185703c421a2dd1f.json
   ```
   
   **⚠️ 重要:**
   - 圧縮ファイルには識別を容易にするためにモニカーを含める必要があります
   - 圧縮ファイル内のJSON gentxファイルは元の名前を維持する必要があります（ハッシュを含む、名前を変更しない）

4. **Telegramで送信:**
   - 圧縮ファイルをTelegramで**Cypher Xenia**（ユーザー名**@XeniaCypher88**）に送信します
   - **メッセージに含める:** "ラウンド1修正用のGentx v2 - モニカー: [your-moniker]"

---

## 📝 プロセスの概要

```
1. 以前のgentxを削除
   ↓
2. ベースジェネシスv2をダウンロード
   ↓
3. ベースジェネシスv2を確認（チェーンIDとアカウント）
   ↓
4. ベースジェネシスv2を検証
   ↓
5. 新しいgentxを生成（アカウントを追加しない）
   ↓
6. 新しいgentxを検証
   ↓
7. サーバーから新しいgentxを抽出
   ↓
8. モニカーでgentxを圧縮
   ↓
9. Telegramで圧縮されたgentxを送信
```

---

## ❓ よくある質問

### 以前のgentxフォルダを削除しないとどうなりますか？

ベースジェネシスv2をダウンロードする前にgentxフォルダを削除しない場合、競合が発生する可能性があります。クリーンな状態で開始するために、gentxフォルダ全体を削除することが重要です。

### 以前に作成した同じgentxを使用できますか？

いいえ、ベースジェネシスv2に基づいて新しいgentxを作成する必要があります。以前のgentxは異なるジェネシスで作成されたため、新しいベースジェネシスv2では無効になります。

### アカウントがベースジェネシスv2にない場合はどうなりますか？

アカウントがベースジェネシスv2にない場合、フェーズ1で編集したジェネシスファイルを送信しなかったか、処理中に問題が発生したことを意味します。この問題を解決するには、開発チームに連絡してください。

### 新しいgentxをいつ送信すべきですか？

ベースジェネシスv2が利用可能になったので、できるだけ早く新しいgentxを送信する必要があります。チームは受信したすべてのgentxをコンパイルして、最終ジェネシスを作成します。

---

## 📚 参照

- [元のラウンド1 - 最初のフロー](/ja/posts/launch-round-1/) - 元のラウンド1ドキュメント
- [ラウンド1の修正](/ja/posts/launch-round-1-correction/) - フェーズ1: 編集したジェネシスファイルの送信
- [完全ガイド：Gentxを作成](https://docs.infinitedrive.xyz/ja/blockchain/genesis/create-gentx/) - 完全な技術ドキュメント

---

**この修正プロセスにおけるあなたの忍耐と協力に感謝します。** あなたの参加はCreativeチェーンのローンチの成功にとって不可欠です。

