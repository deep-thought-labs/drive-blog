---
title: "バリデーターの準備"
date: 2025-12-25T11:00:00Z
draft: false
tags: ["validator", "keys", "security", "guide"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'バリデーターの準備 - Infinite Improbability Drive'
---

{{< figure src="cover" caption="alt" >}}

ブロックチェーンのバリデーターになる予定の場合、準備手順を理解し、正しく実行することが重要です。暗号鍵（キー）とシードフレーズの安全な管理は、バリデーターのセキュリティにとって**極めて重要**です。

## 開始前に

バリデーターとして準備する前に、以下を確認してください。

- ✅ **システムにDriveがインストールされている**
- ✅ **Driveの動作に関する基本的な理解**
- ✅ **ブロックチェーンノードを管理するための環境が準備されている**

> 📖 **Driveのインストール**: まだDriveがインストールされていない場合は、[Driveのインストール](/ja/posts/drive-installation/)ガイドを参照してください。

## 鍵管理：重要な側面

### ⚠️ シードフレーズの重要性

適切な鍵管理は、バリデーターのセキュリティにとって**極めて重要**です。シードフレーズは、鍵を回復する唯一の方法です。失うと、鍵へのアクセスを永続的に失い、したがってバリデーターも失います。

### 正しい準備プロセス

バリデーターとして正しく準備するには、次の手順を順番に実行してください。

1. Driveとキー生成用のグラフィカルインターフェースユーティリティを使用して**キーを作成**
2. **キーを正しく保存する方法を学ぶ**
3. **シードフレーズをオフラインで保存** — できれば紙に
4. 既存のキーを使用して**ノードの初期化を練習**

> 📖 **鍵管理**: 利用可能なすべての操作の詳細については、[鍵管理に関する完全なドキュメント](https://docs.infinitedrive.xyz/en/drive/guides/blockchain-nodes/keys/)を参照してください。

### セキュリティのベストプラクティス

**⚠️ 重要**: バリデーターを作成する前に、次のプラクティスに従うことを確認してください。

- **複数のコピー**: シードフレーズのコピーを少なくとも2〜3つ作成
- **別々の場所**: コピーを異なる物理的な場所に保存
- **耐性のある素材**: シードフレーズを保存するために高品質の紙または金属を使用
- **共有しない**: シードフレーズを誰とも共有しない
- **デジタルに保存しない**: シードフレーズをコンピューターにプレーンテキストで保存しない

> 📖 **セキュリティ**: 詳細な推奨事項については、[セキュリティのベストプラクティスの完全なガイド](https://docs.infinitedrive.xyz/en/drive/guides/blockchain-nodes/keys/security/)を参照してください。

## 重要な検証: priv_validator_key

検証する最も重要な値は、ノードを初期化した後に構成フォルダーに生成される `priv_validator_key` ファイルが、リカバリーキーを使用してノードを初期化するときに**常に同じ値**であることです。

**これが最も重要なことです**: 同じリカバリーキーを使用するたびに、常に同じ値であることを確認してください。

### 推奨される検証プロセス

「create-validator」トランザクションを実行する前に、次の操作を実行する必要があります。

1. 同じシードフレーズを使用して、`--recover` オプションで**ノードを2〜3回初期化**
2. **常に同じ `priv_validator_key` を生成できることを確認**
3. **正確で正しい `priv_validator_key` を確認**し、構成ファイルに存在することを確認
4. **その後**「create-validator」トランザクションを実行

> 📖 **検証**: 完全な検証プロセスについては、[初期化後の検証ガイド](https://docs.infinitedrive.xyz/en/drive/guides/blockchain-nodes/initialization/verification/)を参照してください。

### ⚠️ 重要な警告

**バリデーターを作成したが、ノードのInit（`--recover` を使用）でシードフレーズを使用しなかった場合、正確な `priv_validator_key` を再度生成する方法はありません。**

これにはリスクがあります: **`priv_validator_key` ファイルを失うと、バリデーターも失います。**

### priv_validator_keyを理解する

`priv_validator_key` の目的を理解することが重要です。

- バリデーターは**ブロックに署名**するためにこれを使用
- トークン保有者に属する操作を移動、委任、または実行する**権限はありません**
- これらの操作は、別のプロセスでキーリングに追加されたキーによって実行されます

> 📖 **完全な概念**: その目的と重要性を完全に理解するには、[Private Validator Keyに関するドキュメント](https://docs.infinitedrive.xyz/en/concepts/private-validator-key/)を参照してください。

### ベストプラクティス

**理想的には**、キーリングで使用されるキーと、INITプロセス中に生成される `priv_validator_key` には、**同じシードフレーズ**を使用する必要があります。

> 📖 **違い**: キーリングとPrivate Validator Keyの違いを理解するには、[キーリング vs Private Validator Key](https://docs.infinitedrive.xyz/en/concepts/keyring-vs-validator-key/)を参照してください。

## バリデーターの推奨ワークフロー

バリデーターとして完全に準備するには、次のワークフローに従うことをお勧めします。

1. ✅ **キーを作成** - Driveを使用してキーを生成
2. ✅ **シードフレーズをバックアップ** - シードフレーズを安全に保存
3. ✅ **リカバリーでノードを初期化** - シードフレーズを使用してノードを初期化
4. ✅ **キーをキーリングに追加** - キーがキーリングにあることを確認
5. ✅ **Private Validator Keyを検証** - キーが正しく生成されていることを確認
6. ✅ **バリデーターを作成** - ブロックチェーンで「create-validator」トランザクションを実行

> 📖 **完全なワークフロー**: 詳細なステップバイステップガイドについては、[バリデーターの完全なワークフロー](https://docs.infinitedrive.xyz/en/drive/guides/blockchain-nodes/keys/validator-workflow/)を参照してください。

## 推奨される方法

Driveとキー生成用のグラフィカルインターフェースユーティリティを使用して、ノードで練習することをお勧めします。

- キーを作成
- キーの保存方法を学ぶ
- シードフレーズをオフラインで紙に保存
- そのキーを使用してノードを初期化する練習
- 同じ `priv_validator_key` を複数回回復できることを確認

## 追加ドキュメント

バリデーターの準備の詳細については、以下を参照してください。

- **[鍵管理](https://docs.infinitedrive.xyz/en/drive/guides/blockchain-nodes/keys/)** - 鍵管理の完全なガイド
- **[ノードの初期化](https://docs.infinitedrive.xyz/en/drive/guides/blockchain-nodes/initialization/)** - ノードを正しく初期化する方法
- **[基本概念](https://docs.infinitedrive.xyz/en/concepts/)** - キー、キーリング、バリデーターに関する基本概念
- **[トラブルシューティング](https://docs.infinitedrive.xyz/en/drive/troubleshooting/key-management-issues/)** - 一般的な問題の解決策

---

**覚えておいてください**: バリデーターのセキュリティは、鍵の適切な管理に完全に依存します。本番環境でバリデーターを作成する前に、時間をかけて練習し、すべてが正しく機能することを確認してください。

