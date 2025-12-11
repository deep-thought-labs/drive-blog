---
title: "Testing Phase: How to Create a Gentx (Genesis Transaction)"
date: 2024-02-15T10:00:00Z
tags: ["testing", "guide", "genesis", "project-42", "gentx"]
---

# How to Create a Gentx (Genesis Transaction)

## STEP 0. Prerequisites

### 0-1. Verify `Go` and `jq` Installation

```bash
# Verify Go (required: 1.21+)
go version

# Verify jq (required for scripts)
jq --version
```

If these are not installed, please install them first:
- **Go**: [Go Installation Guide](https://go.dev/doc/install)
- **jq**:
  - macOS: `brew install jq`
  - Linux: `sudo apt-get install jq` or `sudo yum install jq`

---

### 0-2. Clone the Infinite Repository

```bash
git clone https://github.com/deep-thought-labs/infinite.git

cd infinite

git switch migration
```

---

## STEP 1. Compile the Binary

### 1-1. Build and Install the Binary

```bash
make install
```

**Purpose:** Compiles and installs the `infinited` binary.

**Estimated time:** 2–5 minutes (depending on dependencies)

If an error occurs during compilation, install the required packages and rebuild:

```bash
sudo apt update
sudo apt install -y build-essential pkg-config libssl-dev

cd ~/infinite
make clean
make install
```

---

### Binary Location

The binary will be installed at:  
`$HOME/go/bin/infinited`

Make sure `$HOME/go/bin` is included in your system `PATH`.

```bash
# Add to PATH (add this to ~/.bashrc or ~/.zshrc)
export PATH=$PATH:$HOME/go/bin

# Verify
which infinited
infinited version

# Expected output:
# infinite
# infinited version 0.1.9-...
```

---

## STEP 2. Create the Genesis File

### 2-1. Initialize the Chain and Restore Your Account

For **Mainnet**:
```bash
infinited init my-moniker --recover --chain-id infinite_421018-1 --home ~/.infinited
```

> Note: The chain ID may be automatically updated by `customize_genesis.sh`,  
> but you should still use the correct initial chain ID for consistency.

Replace the chain ID below if you are setting up for Testnet or Creative:

```bash
# Testnet
--chain-id infinite_421018001-1 

# Creative
--chain-id infinite_421018002-1
```

This command creates a base genesis file using Cosmos SDK defaults (later customized in the next step).

---

### 2-2. Apply Infinite Drive Customization

**⚠️ Required:** The generated genesis uses Cosmos SDK default values.  
You must apply Infinite Drive-specific customization.

**Mainnet:**
```bash
# Run from the infinite root directory
./scripts/customize_genesis.sh ~/.infinited/config/genesis.json --network mainnet
```

For Testnet or Creative, replace `--network` accordingly:

```bash
# Testnet
--network testnet

# Creative
--network creative
```

**This script performs:**
- ✅ Sets denom for all modules to the correct network denomination  
- ✅ Adds full token metadata  
- ✅ Configures EVM precompiles and ERC20 pairs  
- ✅ Sets consensus, staking, mint, governance, slashing, fee market, and distribution parameters  
- ✅ Automatically backs up genesis

**Configuration file paths:**
- Mainnet: `scripts/genesis-configs/mainnet.json`
- Testnet: `scripts/genesis-configs/testnet.json`
- Creative: `scripts/genesis-configs/creative.json`

These files define all network parameters such as denominations, token metadata, staking, minting, governance, slashing, and consensus settings.

---

### 2-3. Create or Recover Your Account and Add Funds

⚠️ Proceed only if you already have a mnemonic securely stored.

#### 2-3-1. Recover your account from mnemonic

```bash
infinited keys add validator --recover --keyring-backend file --home ~/.infinited
```

You may replace `validator` with any preferred account name.

---

#### 2-3-2. Add funds to the account

```bash
# Add the account to genesis with initial balance (example: 100 tokens)
infinited genesis add-genesis-account validator 100000000000000000000drop \
  --keyring-backend file \
  --home ~/.infinited
```

**Amount:** `100000000000000000000drop` = 100 Improbability (100 × 10¹⁸)

> Always use atomic units (10¹⁸) and include the correct denomination suffix (drop, tdrop, or cdrop).

```bash
# Testnet
tdrop

# Creative
cdrop
```

---

### 2-4. Create the Validator

#### 2-4-1. Generate the Validator Gentx (Genesis Transaction)

**Mainnet:**
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

For Testnet and Creative, replace denominations and chain IDs as follows:

```bash
# Testnet
1000000000000000000tdrop
--chain-id infinite_421018001-1

# Creative
1000000000000000000cdrop
--chain-id infinite_421018002-1
--commission-rate "0.01" \
--commission-max-rate "0.05" \
```

**Parameters:**
- `validator`: Account name (must exist in keyring and have funds in genesis)
- **Amount:** Self-delegation amount  
  - Mainnet: `1000000000000000000drop`  
  - Testnet: `1000000000000000000tdrop`  
  - Creative: `1000000000000000000cdrop`
- `--chain-id`: Must match the chain ID used during initialization
- `--commission-rate`: Initial commission rate (e.g., 10% = "0.10")
- `--commission-max-rate`: Maximum commission rate (e.g., 20% = "0.20")
- `--commission-max-change-rate`: Max rate change per update (e.g., 1% = "0.01")
- `--min-self-delegation`: Minimum self-delegation (atomic units)

---

#### 2-4-2. Collect Genesis Transactions

```bash
infinited genesis collect-gentxs --home ~/.infinited
```

**⚠️ Important:**
- The chain **requires at least one validator** in genesis to start producing blocks.  
- If you skip this, the chain will start but not produce blocks.  
- All validators must use the same `--chain-id`.

---

### 2-5. Validate the Genesis File

```bash
infinited genesis validate-genesis --home ~/.infinited
```

**This checks:**
- ✅ Consistency of denominations  
- ✅ Total supply matches sum of all balances  
- ✅ Validator configuration is valid  
- ✅ JSON structure is correct

---

### 2-6. Distribute the Genesis File

The finalized `genesis.json` must be distributed to **all nodes** before network launch.

```bash
scp ~/.infinited/config/genesis.json user@validator1:/path/to/.infinited/config/
scp ~/.infinited/config/genesis.json user@validator2:/path/to/.infinited/config/
```

**⚠️ Important:**  
All nodes must use **exactly the same** `genesis.json` file.  
Even a minor difference can cause a network fork.

---

## STEP 3. Start the Network

**Mainnet**
```bash
infinited start \
  --chain-id infinite_421018-1 \
  --evm.evm-chain-id 421018 \
  --home ~/.infinited
```

**Testnet**
```bash
infinited start \
  --chain-id infinite_421018001-1 \
  --evm.evm-chain-id 421018001 \
  --home ~/.infinited
```

**Creative**
```bash
infinited start \
  --chain-id infinite_421018002-1 \
  --evm.evm-chain-id 421018002 \
  --home ~/.infinited
```

Always specify **both** IDs:
- `--chain-id`: Cosmos chain ID (format: `{name}_{number}-{version}`)
- `--evm.evm-chain-id`: EVM chain ID (integer, EIP-155 standard)
