---
title: "Launch Round 1"
date: 2025-12-25T14:00:00Z
draft: false
tags: ["testing", "gentx", "creative", "chain-launch", "round-1"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'Launch Round 1 - Creative Network'
---

{{< figure src="cover" caption="alt" >}}

This is the **first testing round** for the launch of the **Infinite Improbability Drive** chain. This round starts today, **December 25, 2025, Thursday** — a Christmas on Thursday, because it's Christmas and because Thursdays are weird days.

In this round, we will work with the **Creative network** of our chain and guide you step by step to create your gentx (genesis transaction) that will be included in the final genesis of the chain.

> 📖 **Testing Phase Context:** This round is part of the [Testing Phase: General Guide](/en/posts/testing-phase-guide/) that was announced on December 20. If you haven't read the general guide yet, we recommend reviewing it to understand the complete context of the testing rounds.

**About Infinite Improbability Drive networks:**
- **Creative**: Network used for testing and development (this first round)
- **Testnet**: Broader testing network
- **Mainnet**: Main production network

The Creative network will be used for our first chain launch testing round. If you can't participate in this round, don't worry: we will perform this same iteration round on other chains repeatedly to ensure we always have stable results and that everyone has opportunities to participate.

## 📅 Participation Period

**Publication date:** December 25, 2025 (Thursday)  
**Delivery period:** December 25 to 28, 2025 (4 days)

During these four days, participants will have the opportunity to:

1. Download the base genesis provided by the team
2. Create their gentx following the specific instructions
3. Deliver their gentx file to the development team

**After December 28**, the development team will compile all received gentxs into a final genesis that will be redistributed to all participants to proceed with the chain launch.

---

## 🎯 Objective of this Round

The objective of this first round is:

- **Validate the complete process** of gentx creation in a real environment
- **Test the infrastructure** of Creative Network before the official launch
- **Ensure all participants** can create and deliver their gentxs correctly
- **Compile a final genesis** with all participating validators

---

## 📋 Prerequisites

Before starting, make sure you have:

- ✅ **Drive installed and configured** (see [Drive Installation](/en/posts/drive-installation/))
- ✅ **Creative service configured** (`node2-infinite-creative`)
- ✅ **Node initialized** using the recovery process with your validator seed phrase
- ✅ **Key added to the keyring** using the same validator seed phrase you used to initialize the node
- ✅ **Know the name of the key** you added to the keyring (this is the name you chose when adding the key)
- ✅ **Access to the server** where Drive is running

**⚠️ Important about the key:**
- You must have initialized your node using the recovery process with your validator seed phrase
- You must have added that same seed phrase as a key to the keyring with a specific name (for example: `validator`, `my-validator`, etc.)
- **You must remember and be clear about what the name of that key is**, as you will need it in all commands in this process
- This key name is what you will use in the `add-genesis-account` and `genesis gentx` commands

> 📖 **Complete documentation:** To understand the complete gentx creation process, see the [complete guide in the documentation](https://docs.infinitedrive.xyz/en/blockchain/genesis/create-gentx/).

---

## 🚀 Specific Instructions for Creative

### Step 1: Access the Creative Container

Navigate to the Creative service directory and access the container's bash:

```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative bash
```

Once inside the container, the `infinited` binary will be available directly.

---

### Step 2: Download the Base Genesis

Download the base genesis for Creative by running the following command inside the container:

```bash
curl -o ~/.infinited/config/genesis.json https://assets.infinitedrive.xyz/tests-round1/genesis-base.json
```

**⚠️ Important:**
- This command will download the base genesis directly to the correct location (`~/.infinited/config/genesis.json`)
- It will replace the existing genesis that was generated during initialization
- Make sure you are inside the container before executing the command

### Verify the Downloaded Genesis

Verify that the genesis downloaded correctly and that the Chain ID is correct:

```bash
cat ~/.infinited/config/genesis.json | jq -r '.chain_id'
```

**Expected Chain ID for Creative:** `infinite_421018002-1`

**Note:** If the download command changes, the development team will notify you through the official communication channel.

### Validate the Base Genesis

Before continuing, validate that the base genesis is correct:

```bash
infinited genesis validate-genesis --home ~/.infinited
```

If validation is successful, you can proceed with confidence.

---

### Step 3: Verify your Key in the Keyring

Before continuing, verify that your key exists in the keyring and remember its name:

```bash
infinited keys list --keyring-backend file --home ~/.infinited
```

This command will show all the keys you have in the keyring. **Identify and note the name of the key** that corresponds to your validator (the one you added using your validator seed phrase).

**Example output:**
```
- name: validator
  type: local
  address: infinite1abc123...
  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"..."}'
```

In this example, the key name is `validator`. **Use this same name** in the following steps, replacing `validator` with your actual key name.

---

### Step 4: Add Funds to the Account in Genesis

**💡 Tip:** Before executing the command, you can prepare it in a plain text editor for easier editing. This will allow you to review and edit the complete command (including your key name) before copying and pasting it into the console.

Add your account to the genesis with the initial balance. **For this first Creative round**, use the following amount:

```bash
infinited genesis add-genesis-account <your-key-name> 1000000000000000000000cdrop \
  --keyring-backend file \
  --home ~/.infinited
```

**⚠️ Important:** Replace `<your-key-name>` with the exact name of your key that you verified in Step 3 (for example: `validator`, `my-validator`, etc.).

**Specific parameters for Creative:**
- Denomination: `cdrop` (Creative drop)
- Amount: `1000000000000000000000cdrop` (1000 tokens in atomic units)

### Verify that the Account was Added Correctly

Before generating the gentx, it's recommended to verify that your account was added correctly to the genesis. You can do this by checking the genesis content:

```bash
cat ~/.infinited/config/genesis.json | jq '.app_state.bank.balances'
```

This command will show all balances in the genesis. Look for your public address (the same one you saw when listing your keys) and verify that it has the correct amount.

**Example expected output for Creative:**
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

You can also verify the account information in the accounts section:

```bash
cat ~/.infinited/config/genesis.json | jq '.app_state.auth.accounts'
```

**Example expected output:**
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

**Note:** The values shown are examples. For Mainnet the denomination will be `drop`, for Testnet it will be `tdrop`, and for Creative it will be `cdrop`.

If you see your address with the correct amount, you can proceed with confidence to generate your gentx.

---

### Step 5: Generate the Gentx

**💡 Tip:** Before executing the command, you can prepare it in a plain text editor for easier editing. This will allow you to review and edit the complete command before copying and pasting it into the console.

Generate your gentx with the specific parameters for Creative:

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

**⚠️ Important:** Replace `<your-key-name>` with the exact name of your key that you verified in Step 3.

**Specific parameters for this round:**
- **Chain ID:** `infinite_421018002-1` (Creative)
- **Self-delegation:** `10000000000000000000cdrop` (10 tokens)
- **Initial commission rate:** `0.01` (1%)
- **Maximum commission rate:** `0.05` (5%)
- **Maximum rate change:** `0.01` (1%)
- **Minimum self-delegation:** `1000000000000000000` (1 token)

---

### Step 6: Validate your Gentx

Before delivering your gentx, validate that it works correctly:

```bash
# Collect gentxs (includes yours)
infinited genesis collect-gentxs --home ~/.infinited

# Validate the resulting genesis
infinited genesis validate-genesis --home ~/.infinited
```

If validation is successful, your gentx is ready to deliver.

---

### Step 7: Locate your Gentx File

Your gentx was generated in the following directory inside the container:

```bash
~/.infinited/config/gentx/
```

The gentx file has a format with a unique hash, similar to: `gentx-adba573456c82908c3221163185703c421a2dd1f.json`

**⚠️ Important:** The file name does NOT include your moniker, but a unique hash generated automatically. **You should NOT rename this JSON file**.

To see the exact name of your file:

```bash
ls -la ~/.infinited/config/gentx/
```

You will see a file with format `gentx-<hash>.json`. Note the complete name of this file for the following steps.

---

### Step 8: Extract the Gentx File from the Server

The gentx file is stored in the Docker persistent volume, so it's accessible from the host system without needing to copy it manually.

**From the host system (outside the container):**

1. **Navigate to the Creative service directory:**
   ```bash
   cd services/node2-infinite-creative
   ```

2. **Locate your gentx file:**
   ```bash
   ls -la persistent-data/.infinited/config/gentx/
   ```

3. **Copy the gentx file maintaining its original name:**
   
   **⚠️ Important:** The gentx file has a name with hash format (example: `gentx-adba573456c82908c3221163185703c421a2dd1f.json`). **You should NOT rename this JSON file**. Keep its original name as it was generated.
   
   ```bash
   # Create a specific directory for the gentx
   mkdir -p ~/gentx-round-1
   
   # Copy maintaining the original name (replace <hash> with the actual hash of your file)
   cp persistent-data/.infinited/config/gentx/gentx-<hash>.json ~/gentx-round-1/
   ```
   
   **Example:** If your file is named `gentx-adba573456c82908c3221163185703c421a2dd1f.json`:
   ```bash
   mkdir -p ~/gentx-round-1
   cp persistent-data/.infinited/config/gentx/gentx-adba573456c82908c3221163185703c421a2dd1f.json ~/gentx-round-1/
   ```

**If you're on a remote server**, you can use `scp` to download it to your local computer maintaining the original name:

```bash
# From your local computer (replace <hash> with the actual hash of your file)
scp user@server:/path/to/drive/services/node2-infinite-creative/persistent-data/.infinited/config/gentx/gentx-<hash>.json ~/
```

**Explanation of the `scp` command:**
- `user`: This is the username you use to log in to your server
- `@server`: Refers to the IP address or domain name of your server (for example: `@192.168.1.100` or `@my-server.com`)
- The path after the colon (`:`) is the complete path to the file on the server
- `~/` is the destination directory on your local computer (your home directory)

**⚠️ Important:** When executing this command, it's very likely that the system will ask you for credentials or authorization to perform the transfer. These are the same credentials you use when logging into your server (password or SSH key).

**Complete example:** If your user is `ubuntu`, your server has the IP `192.168.1.100`, and your file is named `gentx-adba573456c82908c3221163185703c421a2dd1f.json`:
```bash
mkdir -p ~/gentx-round-1
scp ubuntu@192.168.1.100:/home/ubuntu/drive/services/node2-infinite-creative/persistent-data/.infinited/config/gentx/gentx-adba573456c82908c3221163185703c421a2dd1f.json ~/gentx-round-1/
```

---

### Step 9: Compress and Send your Gentx

**Compress the file:**

**⚠️ Important:** 
- The JSON gentx file must maintain its original name (with the hash, don't rename it)
- The compressed file SHOULD include your moniker in its name to facilitate identification

```bash
# Create a compressed file with your moniker (replace <moniker> with your actual moniker and <hash> with your file hash)
tar -czf gentx-<moniker>-round-1.tar.gz gentx-<hash>.json

# Or using zip
zip gentx-<moniker>-round-1.zip gentx-<hash>.json
```

**Example:** If your moniker is `my-validator` and your file is named `gentx-adba573456c82908c3221163185703c421a2dd1f.json`:
```bash
cd ~/gentx-round-1
tar -czf gentx-my-validator-round-1.tar.gz gentx-adba573456c82908c3221163185703c421a2dd1f.json
```

**Compressed file structure:**
- **Compressed file name:** `gentx-<your-moniker>-round-1.tar.gz` (includes your moniker for identification)
- **Content of compressed file:** `gentx-<hash>.json` (the original JSON file with its original name)

**Send via Telegram:**

Send the compressed file directly via Telegram to **Cypher Xenia** with username **@XeniaCypher88**.

1. Open Telegram
2. Search for user **@XeniaCypher88** (Cypher Xenia)
3. Send the compressed file directly

**⚠️ Important:**
- Only send the gentx file, **NOT** the complete genesis
- Verify that you are sending the correct file before sending it
- Keep a backup copy of your gentx
- Include your moniker or identifier in the message if the team requests it

---

## 📝 Additional Information

### Verify your Gentx Before Sending

You can verify the content of your gentx to make sure it's correct:

```bash
# From the container (replace <hash> with your file hash)
cat ~/.infinited/config/gentx/gentx-<hash>.json | jq .
```

Or from the host:

```bash
# Replace <hash> with your file hash
cat services/node2-infinite-creative/persistent-data/.infinited/config/gentx/gentx-<hash>.json | jq .
```

Verify that:
- The `chain_id` is `infinite_421018002-1`
- The `moniker` inside the JSON content is correct
- The amounts and parameters are as specified

---

## 🔄 Post-Delivery Process

Once the development team receives all gentxs (until December 28):

1. **Final Genesis Compilation:**
   - The team will compile all received gentxs into a final genesis
   - The final genesis will be validated to be correct and complete

2. **Final Genesis Redistribution:**
   - All participants will receive the compiled final genesis
   - Instructions will be provided to replace the local genesis

3. **Chain Launch:**
   - Once everyone has the final genesis, the launch will proceed
   - Your validator will be active from block 1

---

## ❓ Frequently Asked Questions

### What happens if I don't deliver my gentx on time?

If you don't deliver your gentx before December 28, you won't be able to participate as a validator in the initial Creative launch. However, we will perform this same iteration round on other chains repeatedly, so you will have future opportunities to participate.

### Can I modify my gentx after sending it?

No, once you send your gentx, you won't be able to modify it. Make sure to validate it correctly before sending it.

### What information should I include when sending the gentx?

You only need to send the compressed gentx file. The development team may request additional information if necessary.

### How do I know my gentx was received correctly?

The development team will confirm receipt of your gentx. If you don't receive confirmation, contact the team.

---

## 📚 References

- [Complete Guide: Create Gentx](https://docs.infinitedrive.xyz/en/blockchain/genesis/create-gentx/) - Complete technical documentation
- [Drive Installation](/en/posts/drive-installation/) - How to install Drive
- [Validator Preparation](/en/posts/validator-preparation/) - Key management and security

---

## 🎯 Process Summary

```
1. Access the Creative container
   ↓
2. Download base genesis
   ↓
3. Validate base genesis
   ↓
4. Verify that your key exists in the keyring and remember its name
   ↓
5. Add account with funds to genesis using your key name (specified amounts)
   ↓
6. Generate gentx using your key name with Creative-specific parameters
   ↓
7. Validate gentx and genesis
   ↓
8. Extract gentx from server to your computer (maintaining original name)
   ↓
9. Compress maintaining original name and send via Telegram
   ↓
10. Wait for confirmation and final genesis
```

---

**We're excited to start this first testing round with you!** If you have any questions or encounter any problems during the process, don't hesitate to contact the development team.

**Delivery period:** December 25 - 28, 2025  
**Service:** Creative (node2-infinite-creative)  
**Chain ID:** `infinite_421018002-1`

---

<div style="border: 2px solid currentColor; border-left: 6px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px;">

## 🔄 UPDATE: Final Genesis and Synchronized Launch

> **📅 This is an update after the gentx delivery period.**

</div>

---

The deadline for gentx file delivery has ended and all files have been successfully collected. The final Genesis file is now ready to be distributed to all new validators participating in this phase.

### Download the Final Genesis

The development team will provide a script to download and replace the final Genesis file. This script will download the Genesis compiled with all received gentxs and place it in the correct location within the container.

> **📝 Note:** The download command and the URL of the final Genesis file are yet to be defined and will be updated in this document when available. **DO NOT attempt to use the example command shown below, as it is not yet available.**

**For participants in this phase:**

1. **Access the Creative container:**
   - Navigate to the Creative service directory and access the container's bash (as done in Step 1):
   ```bash
   cd services/node2-infinite-creative
   ./drive.sh exec infinite-creative bash
   ```

2. **Download the final Genesis:**
   - Once inside the container, execute the command that will be provided by the development team (example of expected format):
   ```bash
   curl -o ~/.infinited/config/genesis.json [URL_TO_BE_DEFINED]
   ```
   
   **⚠️ Important:**
   - **This command is only an example of the format.** The actual URL will be provided when available
   - **DO NOT execute this command yet**, as the URL is not available
   - Once the command is available, it will download the final Genesis directly to the correct location (`~/.infinited/config/genesis.json`)
   - It will replace the existing genesis with the compiled final Genesis
   - Make sure you are inside the container before executing the command when it's available

3. **Verify the downloaded Genesis:**
   - Verify that the Genesis downloaded correctly and that the Chain ID is correct:
   ```bash
   cat ~/.infinited/config/genesis.json | jq -r '.chain_id'
   ```
   
   **Expected Chain ID for Creative:** `infinite_421018002-1`

4. **Validate the final Genesis:**
   - Before continuing, validate that the final Genesis is correct:
   ```bash
   infinited genesis validate-genesis --home ~/.infinited
   ```
   
   If validation is successful, you can proceed with confidence.

5. **Preparation for launch:**
   - Once you have the final Genesis downloaded and validated, you will be ready to start your validator node
   - The chain will be born when all validators start their nodes in a synchronized manner

### ⏰ Synchronized Launch

**⚠️ Important:** The chain launch must be performed in a **synchronized** manner. All participants must start their validator nodes at the same time for the chain to be born correctly.

> **📝 Note:** The specific times in the schedule are yet to be defined and will be updated in this document when confirmed.

**Launch schedule:**

1. **Script availability time:**
   - **Time:** [To be defined] UTC
   - **At that moment** is when all participants will be able to proceed to execute the script to download and replace the final Genesis file
   - Make sure you have access to the container and are prepared to execute the command at that time

2. **Node start time:**
   - **Time:** [To be defined] UTC (five minutes after the script availability time)
   - This will be the moment when **all participants must start their validator nodes simultaneously**
   - This synchronization is critical for the successful birth of the chain

**Schedule example (times to be confirmed):**
- If the script is available at **[HH:MM] UTC**, at that moment all participants will be able to proceed to execute the script to download the final Genesis file
- At **[HH:MM] UTC** (five minutes later), all participants must start their validator nodes at the same time

> **📢 Note:** The exact times will be updated in this document and communicated by the development team through official communication channels. Make sure to stay alert to these announcements and be prepared for the synchronized launch.

---

**We're about to bring the Creative chain to life!** Make sure you have everything prepared and are ready for the synchronized launch.

