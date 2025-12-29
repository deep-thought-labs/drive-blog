---
title: "Launch Round 1 - Correction"
date: 2025-12-29T00:00:00Z
draft: false
tags: ["testing", "gentx", "creative", "chain-launch", "round-1", "correction"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'Launch Round 1 - Correction'
---

{{< figure src="cover" caption="alt" >}}

This document describes the **flow correction** and **updated steps** to continue with Round 1 of the **Infinite Improbability Drive** chain launch.

> 📖 **Context:** This is a continuation document of the [original Round 1](/en/posts/launch-round-1/). If you did not participate in the original Round 1 period (December 25-28, 2025), this document does not apply to you, as the original participation period has ended.

---

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px;">

## ⚠️ IMPORTANT: Only for Original Round 1 Participants

**This document and the steps described here are ONLY for participants who already participated in the original Round 1 period (December 25-28, 2025).**

- ✅ If you already sent your gentx during the original period, you must follow these steps
- ❌ If you did NOT participate in the original period, this document does not apply to you
- ⏰ There is no strict time limit to complete these steps, but it is important to do so **as soon as possible** so the team can proceed with the correction

</div>

---

## 🔍 Error Detected in Original Flow

During the Round 1 process, a **fundamental error** was identified in the initial flow that requires correction before we can continue with the chain launch.

### What happened?

The base Genesis file provided initially had a critical problem: **it did not contain the pre-added accounts and balances** that it should have included from the beginning.

**Flow that was followed (incorrect):**
1. An empty base Genesis was provided (without accounts)
2. Each participant added their own account individually in their local Genesis file
3. Each participant created their gentx based on their edited Genesis
4. When trying to compile all gentxs, the process would fail because the original base Genesis did not have the accounts from the start

**Correct flow that should have been followed:**
1. Participants provide their public accounts to the team
2. The team pre-fills the base Genesis with all accounts and balances
3. The complete base Genesis is provided to all participants
4. Participants only create their gentx based on the complete base Genesis
5. The team successfully compiles all gentxs

### Correction Plan

To correct this error, the process will be divided into two phases:

**Phase 1: Information Collection**
- Participants send their edited Genesis files (which contain their added accounts)
- The team compiles all individual Genesis files to extract all accounts and balances
- The team creates a new base Genesis v2 that includes all accounts from the start

**Phase 2: Corrected Process**
- The new base Genesis v2 is provided to all participants
- Participants download the base Genesis v2 (which already includes all accounts)
- Participants create new gentx based on the base Genesis v2
- Participants send their new gentx
- The team compiles all gentxs into the final Genesis

---

## 📋 Phase 1: Send your Edited Genesis File

In this phase, we need you to send the Genesis file you edited during the original process (the one containing your added account).

### Step 1: Locate your Edited Genesis File

Your edited Genesis file is located in the same place where you worked during the original process:

**Inside the container:**
```bash
~/.infinited/config/genesis.json
```

**From the host system (outside the container):**
```bash
services/node2-infinite-creative/persistent-data/config/genesis.json
```

Or from the service directory:
```bash
cd services/node2-infinite-creative
ls -la persistent-data/config/genesis.json
```

### Step 2: Verify it is the Correct File

Before extracting and sending, verify that the file contains your account:

**From the container:**
```bash
# Access the container
cd services/node2-infinite-creative
./drive.sh exec infinite-creative bash

# List your keys to get your address
infinited keys list --keyring-backend file --home ~/.infinited

# Verify that your address is in the Genesis (replace YOUR_ADDRESS with your real address)
cat ~/.infinited/config/genesis.json | jq '.app_state.bank.balances[] | select(.address == "YOUR_ADDRESS")'
```

**From the host:**
```bash
# Replace YOUR_ADDRESS with your real address
cat services/node2-infinite-creative/persistent-data/config/genesis.json | jq '.app_state.bank.balances[] | select(.address == "YOUR_ADDRESS")'
```

If you see your address with the correct balance, that is the file you need to send.

### Step 3: Extract the Genesis File from the Server

The Genesis file is stored in the Docker persistent volume, so it is accessible from the host system without needing to copy it manually.

**If you are working directly on the server:**

1. **Navigate to the Creative service directory:**
   ```bash
   cd services/node2-infinite-creative
   ```

2. **Create a directory for the file:**
   ```bash
   mkdir -p ~/genesis-correction
   ```

3. **Copy the Genesis file:**
   ```bash
   cp persistent-data/config/genesis.json ~/genesis-correction/
   ```

**If you are on a remote server**, you need to download the file to your local computer using `scp`:

```bash
# From your local computer
# Replace <user>, <server> and the path according to your configuration
scp <user>@<server>:/path/to/drive/services/node2-infinite-creative/persistent-data/config/genesis.json ~/genesis-correction/
```

**Explanation of the `scp` command:**
- `<user>`: This is the username you use to log in to your server
- `<server>`: Refers to the IP address or domain name of your server (for example: `192.168.1.100` or `my-server.com`)
- The path after the colon (`:`) is the complete path to the file on the server
- `~/genesis-correction/` is the destination directory on your local computer

**⚠️ Important:** When executing this command, it is very likely that the system will ask you for credentials or authorization to perform the transfer. These are the same credentials you use when logging into your server (password or SSH key).

**Complete example:** If your user is `ubuntu`, your server has the IP `192.168.1.100`, and your working directory is `/home/ubuntu/drive`:
```bash
# From your local computer
mkdir -p ~/genesis-correction
scp ubuntu@192.168.1.100:/home/ubuntu/drive/services/node2-infinite-creative/persistent-data/config/genesis.json ~/genesis-correction/
```

### Step 4: Compress your Genesis File

Once you have the Genesis file on your local computer (either copied directly on the server or downloaded with `scp`), compress it with a name that includes your moniker to facilitate identification:

1. **Navigate to the directory where the file is:**
   ```bash
   cd ~/genesis-correction
   ```

2. **Compress the file with your moniker:**
   ```bash
   tar -czf genesis-<your-moniker>-round-1-correction.tar.gz genesis.json
   
   # Or using zip
   zip genesis-<your-moniker>-round-1-correction.zip genesis.json
   ```

   **Example:** If your moniker is `my-validator`:
   ```bash
   tar -czf genesis-my-validator-round-1-correction.tar.gz genesis.json
   ```

   **⚠️ Important:**
   - The compressed file must include your moniker in the name to facilitate identification
   - The original `genesis.json` file inside the compressed file will maintain its original name

### Step 5: Send your Genesis File

Send the compressed file via Telegram to **Cypher Xenia** with username **@XeniaCypher88**.

1. Open Telegram
2. Search for user **@XeniaCypher88** (Cypher Xenia)
3. Send the compressed file (for example: `genesis-<your-moniker>-round-1-correction.tar.gz`)
4. **Include in the message:** "Edited Genesis file for Round 1 correction - Moniker: [your-moniker]"

**⚠️ Important:**
- Only send the compressed file that contains `genesis.json`, **NOT** the complete genesis with gentx
- Verify that you are sending the correct file before sending it
- Keep a backup copy of your Genesis file

### Step 6: Confirmation

Once you send your Genesis file, the team will receive and process it. The team will compile all received Genesis files to create the base Genesis v2.

**There is no strict time limit**, but it is important to send your file **as soon as possible** so the team can proceed with the correction.

---

## ⏳ Waiting for Base Genesis v2

Once the team has received and processed all individual Genesis files:

1. **The team will extract** all accounts and balances from all received Genesis files
2. **The team will create** a new base Genesis v2 that will include all accounts from the start
3. **The team will publish** the new base Genesis v2 and notify all participants

> **📢 Note:** The team will notify when the base Genesis v2 is available through official communication channels. Make sure to stay alert to these announcements.

**👉 Once the base Genesis v2 is available, consult the instructions in the following document:**
**→ [Launch Round 1 - Create Gentx Again](/en/posts/launch-round-1-create-gentx-again/)**

---

## 📝 Process Summary (Phase 1)

```
PHASE 1: Correction
1. Locate edited Genesis file
   ↓
2. Verify that it contains your account
   ↓
3. Extract/download Genesis file from server
   ↓
4. Compress Genesis file with moniker
   ↓
5. Send compressed Genesis file via Telegram
   ↓
6. Wait for confirmation and base Genesis v2
```

---

## ❓ Frequently Asked Questions

### What happens if I don't send my edited Genesis file?

If you don't send your edited Genesis file, the team will not be able to include your account in the base Genesis v2, and you will not be able to participate in the chain launch.

### Is there a time limit to send my Genesis file?

There is no strict time limit, but it is important to send it **as soon as possible** so the team can proceed with the correction and create the base Genesis v2.

### Can I modify my Genesis file before sending it?

No, you must send exactly the same Genesis file you used to create your original gentx. Do not make additional modifications.

### What happens if I no longer have my edited Genesis file?

If you no longer have your edited Genesis file, contact the development team to find an alternative solution.

### When will the base Genesis v2 be available?

The base Genesis v2 will be available once the team has received and processed all individual Genesis files from participants. The team will notify when it is ready.

**👉 Once the base Genesis v2 is available, consult the instructions in:**
**→ [Launch Round 1 - Create Gentx Again](/en/posts/launch-round-1-create-gentx-again/)**

---

## 📚 References

- [Original Round 1 - First Flow](/en/posts/launch-round-1/) - Original Round 1 document
- [Complete Guide: Create Gentx](https://docs.infinitedrive.xyz/en/blockchain/genesis/create-gentx/) - Complete technical documentation

---

**Thank you for your patience and collaboration in this correction process.** Your participation is essential for the success of the Creative chain launch.

