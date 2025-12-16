---
title: テストフェーズ：Gentx（Genesisトランザクション）の作成方法
date: 2025-12-11T10:00:00Z
tags: ["guide", "genesis"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'ガイド - Gentxの作成方法'
---

# テストフェーズ：Gentx（Genesisトランザクション）の作成方法

{{< figure src="cover" caption="alt" >}}

## STEP 0. 前提条件

## 0-1. goとjqのインストール確認

```bash
# Verify Go (required: 1.21+)

go version

# Verify jq (required for scripts)
jq --version
```

上記がインストールされていない場合は、インストールして下さい
- **Go** : [Go インストール](https://go.dev/doc/install)

- **jq** :
	- macOS:`brew install jq`
	- Linux:`sudo apt-get install jq`または`sudo yum install jq`

  
  

## 0-2. Infiniteリポジトリをクローンする

```bash
git clone https://github.com/deep-thought-labs/infinite.git

cd infinite

git switch migration
```

  
---

# STEP 1. バイナリのコンパイル

## 1-1. バイナリをコンパイルする
```bash
make install
```

**機能**：`infinited`バイナリをコンパイルしてインストールする
**推定所要時間**: 2～5 分 (初回、依存関係のダウンロード)

コンパイル中にエラーが出た場合は、必要パッケージを導入して再ビルドして下さい

```bash
# 必要パッケージを導入
sudo apt update

sudo apt install -y build-essential pkg-config libssl-dev

# 再ビルド
cd ~/infinite
make clean
make install
```

---
### バイナリロケーション

バイナリは次の場所にインストールされます:`$HOME/go/bin/infinited`

**直接コマンドを使用するには**、`$HOME/go/bin`PATH に含まれていることを確認してください。

```bash
# Add to PATH (add to ~/.bashrc or ~/.zshrc)
export PATH=$PATH:$HOME/go/bin

# Verify
which infinited
infinited version
# You should see something like:
# infinite
# infinited version 0.1.10
```

---
# STEP 2. Genesis ファイルの作成

## 2-1: アカウントを復元してGenesisを初期化する

**メインネット**
```bash
# Initialize the chain with a moniker and chain ID (mainnet)

infinited init Xenia --recover --chain-id infinite_421018-1 --home ~/.infinited

```

>**注**: チェーン ID は`customize_genesis.sh`で自動的に更新されますが、一貫性を保つために正しい初期チェーン ID を使用する必要があります。

テストネットとクリエイティブは、以下の`--chain-id`に置き換えて下さい。

```bash
# テストネット
--chain-id infinite_421018001-1

# クリエイティブ
--chain-id infinite_421018002-1
```

**これが行うこと**: デフォルトの Cosmos SDK 値を使用して基本的なジェネシス ファイルを作成します (「ステーク」額面を使用 - 次の手順でカスタマイズされます)。

---
## 2-2: Infinite Driveのカスタマイズを適用する

**⚠️必須**：生成されたジェネシスは`infinited init`Cosmos SDKのデフォルト値を使用します。Infinite Driveのカスタマイズを適用する必要があります。

**メインネット**
```bash
# infiniteのルートディレクトリから実行

./scripts/customize_genesis.sh ~/.infinited/config/genesis.json --network mainnet
```


テストネットとクリエイティブは、以下の`--network`に置き換えて下さい。
```bash
# テストネット
--network testnet

# クリエイティブ
--network creative
```

**このスクリプトの機能**:
- ✅ すべてのモジュールの額面をネットワーク固有の額面に設定します
- ✅ 完全なトークンメタデータを追加します
- ✅ EVMプリコンパイルとERC20ペアを構成する
- ✅ コンセンサス、ステーキング、ミント、ガバナンス、スラッシュ、手数料市場、分配パラメータを設定します
- ✅ 自動バックアップを作成

**構成ファイルの場所**: スクリプトは、次の場所にある JSON 構成ファイルからすべてのネットワーク固有のパラメータを読み取ります。

- **メインネット**:`scripts/genesis-configs/mainnet.json`

- **テストネット**:`scripts/genesis-configs/testnet.json`

- **クリエイティブ**：`scripts/genesis-configs/creative.json`

これらのファイルには、ネットワーク固有のパラメータ（額面、トークンメタデータ、ステーキング、ミント、ガバナンス、スラッシング、手数料市場、分配、コンセンサスパラメータ）がすべて含まれています。

---
## 2-3：アカウントを作成して資金を入金する

⚠️すでに作成済みのアカウントがあり、ニーモニックを安全に保管している前提で進めます。
### **2-3-1 : ニーモニックからアカウントを回復**:

```shell
infinited keys add validator --recover --keyring-backend file --home ~/.infinited
```

`validator`の部分は自分で管理しやすいアカウント名に変更して下さい

---
### **2-3-2 : アカウントに資金を追加する**

```shell
# Add the account to genesis with initial balance (example: 100 tokens)

infinited genesis add-genesis-account validator 100000000000000000000drop \ --keyring-backend file \ --home ~/.infinited
```

  
>量: `100000000000000000000drop`= 100 Improbability (100 × 10^18)
>
  **金額の形式**：アトミック単位（10の18乗）を使用してください。必ずデノムサフィックス（drop, tdrop, cdrop）を含めてください。

```bash
# テストネット
tdrop

# クリエイティブ
cdrop

```

---
## 2-4: バリデータを作成する

### 2-4-1 : バリデータジェネシストランザクション（gentx）の作成

  **メインネット**
```bash
# Create a gentx for the validator (example: self-delegate 1 token)
infinited genesis gentx validator 1000000000000000000drop \
  --chain-id infinite_421018-1 \
  --commission-rate "0.10" \
  --commission-max-rate "0.20" \
  --commission-max-change-rate "0.01" \
  --min-self-delegation "1000000000000000000" \
  --keyring-backend file \
  --home ~/.infinited
```

  
テストネットとクリエイティブは以下の単位と`--chain-id`に置き換えて下さい

```bash
# テストネット

1000000000000000000tdrop
--chain-id infinite_421018001-1


# クリエイティブ
1000000000000000000cdrop
--chain-id infinite_421018002-1
--commission-rate "0.01" \
--commission-max-rate "0.05" \
```

**パラメータ**:
- `validator`: アカウント名（キーリングに存在し、Genesisに資金がある必要があります）
- 金額: 自己委任する金額（1トークン = 1 × 10^18、アカウント残高以下である必要があります）

	- メインネット:`1000000000000000000drop`
	- テストネット:`1000000000000000000tdrop`
	- クリエイティブ：`1000000000000000000cdrop`

- `--chain-id`: `infinited init`で使用されているチェーンIDと一致する必要があります
	- メインネット:`infinite_421018-1`
	- テストネット:`infinite_421018001-1`
	- クリエイティブ：`infinite_421018002-1`
- `--commission-rate`: 初期手数料（例：10% = "0.10"、クリエイティブ使用1% = "0.01"）
- `--commission-max-rate`: 最大手数料（例：20% = "0.20"、クリエイティブ使用5% = "0.05"）
- `--commission-max-change-rate`: 更新ごとの最大変更 (例: 1% = "0.01")
- `--min-self-delegation`: 自己委任する最小トークン数（アトミック単位、例：1トークン = "10000000000000000000"）

---
### 2-4-2 : Genesisトランザクションの収集

```shell
# Collect all gentx files into genesis

infinited genesis collect-gentxs --home ~/.infinited
```

> **⚠️重要**：
>- チェーンはブロックを生成するためにジェネシスに**少なくとも1つのバリデータを必要とする**
>- `collect-gentxs`を実行しない場合、チェーンは開始されますが、ブロックは生成されません。
>- すべてのバリデータは同じ`--chain-id`を使用する必要があります

---
## 2-5 : Genesisを検証する

```shell
# Validate the genesis file
infinited genesis validate-genesis --home ~/.infinited
```

**これは以下をチェックします**:
- ✅ すべてのデノミネーションの一貫性
- ✅ 総供給量は残高の合計に等しい
- ✅ バリデーターの設定が正しい
- ✅ JSON構造は有効であること

---

## 2-6 : Genesisファイルを配布する

ファイナライズされた`genesis.json`は、起動前にネットワーク内の**すべてのノード**に配布される必要があります。

```shell
# Copy genesis to all validator nodes
scp ~/.infinited/config/genesis.json user@validator1:/path/to/.infinited/config/

scp ~/.infinited/config/genesis.json user@validator2:/path/to/.infinited/config/

```

**⚠️重要**：すべてのノードは**完全に同じ** `genesis.json`ファイルを使用する必要があります。少しでも相違があると、ネットワークがフォークします。  

---
# STEP 3. ネットワークを開始する

**メインネット**
```bash
infinited start \
  --chain-id infinite_421018-1 \
  --evm.evm-chain-id 421018 \
  --home ~/.infinited
```

  

**テストネット：**
```bash
infinited start \
  --chain-id infinite_421018001-1 \
  --evm.evm-chain-id 421018001 \
  --home ~/.infinited
```

  

**クリエイティブ：**
```bash
infinited start \
  --chain-id infinite_421018002-1 \
  --evm.evm-chain-id 421018002 \
  --home ~/.infinited
```


**常に二つのチェーンIDを指定して下さい**:
- `--chain-id`: コスモスチェーンID (形式: `{name}_{number}-{version}`)
- `--evm.evm-chain-id`: EVMチェーンID（整数、EIP-155）

---