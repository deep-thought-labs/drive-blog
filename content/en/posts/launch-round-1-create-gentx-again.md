---
title: "Launch Round 1 - Create Gentx Again"
date: 2025-12-29T05:00:00Z
draft: false
tags: ["testing", "gentx", "creative", "chain-launch", "round-1", "v2"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'Launch Round 1 - Create Gentx Again'
---

{{< figure src="cover" caption="alt" >}}

This document contains the **instructions to create your gentx again** using the base Genesis v2, as part of the corrected process of Round 1 of the **Infinite Improbability Drive** chain launch.

> 📖 **Context:** This document is a continuation of [Round 1 correction](/en/posts/launch-round-1-correction/). Make sure you have completed Phase 1 (sending your edited Genesis file) before following these instructions.

---

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px;">

## ⚠️ IMPORTANT: Only for Original Round 1 Participants

**This document and the steps described here are ONLY for participants who already participated in the original Round 1 period (December 25-28, 2025) and who have already completed Phase 1 of correction (sending the edited Genesis file).**

- ✅ If you already sent your edited Genesis file in Phase 1, you must follow these steps
- ❌ If you did NOT participate in the original period or did NOT complete Phase 1, this document does not apply to you

</div>

---

## 📋 Create your Gentx Again with Base Genesis v2

The team has compiled all received individual Genesis files and has created the **base Genesis v2** that includes all accounts and balances of all participants from the start.

Now you must follow these steps to create your new gentx based on the base Genesis v2.

### Step 1: Delete your Previous Gentx

Before downloading the new base Genesis v2, you need to delete the previous gentx you created. This previous gentx will not be used because it was created with the previous Genesis, and now we need to create a new gentx using the new corrected Genesis file (base Genesis v2).

To ensure we really delete all related files, **we recommend deleting the entire gentx folder** instead of searching for and deleting individual files. This guarantees a clean state to create your new gentx.

**⚠️ IMPORTANT WARNING:** Be very careful when executing these commands. Make sure to delete **ONLY** the `gentx/` folder and **DO NOT** accidentally delete other important folders such as the complete `config/` folder or other system folders. Carefully verify the path before executing the command.

**Inside the container:**
```bash
# Access the container
cd services/node2-infinite-creative
./drive.sh exec infinite-creative bash

# Delete the entire gentx folder
rm -rf ~/.infinited/config/gentx/
```

**From the host system:**
```bash
# Delete the entire gentx folder
rm -rf services/node2-infinite-creative/persistent-data/config/gentx/
```

### Step 2: Download the Base Genesis v2

The team has compiled the base Genesis v2 that includes all accounts and balances of all participants from the start.

**Inside the container:**
```bash
# Access the container
cd services/node2-infinite-creative
./drive.sh exec infinite-creative bash

# Download the base Genesis v2
curl -o ~/.infinited/config/genesis.json https://assets.infinitedrive.xyz/tests-round1/genesis-base-v2.json
```

**⚠️ Important:**
- This base Genesis v2 includes all accounts and balances of all participants
- It will replace any existing Genesis in your configuration

### Step 3: Verify the Base Genesis v2

Once the base Genesis v2 is downloaded:

1. **Verify the Chain ID:**
   ```bash
   cat ~/.infinited/config/genesis.json | jq -r '.chain_id'
   ```
   
   **Expected Chain ID for Creative:** `infinite_421018002-1`

2. **Verify that your account is included:**
   ```bash
   # List your keys to get your address
   infinited keys list --keyring-backend file --home ~/.infinited
   
   # Verify that your address is in the Genesis (replace YOUR_ADDRESS with your real address)
   cat ~/.infinited/config/genesis.json | jq '.app_state.bank.balances[] | select(.address == "YOUR_ADDRESS")'
   ```

3. **Validate the base Genesis v2:**
   ```bash
   infinited genesis validate-genesis --home ~/.infinited
   ```
   
   If validation is successful, you can proceed with confidence.

### Step 4: Generate your New Gentx

**IMPORTANT:** In this corrected process, **you do NOT need to add your account to the Genesis**, as it is already included in the base Genesis v2.

**💡 Tip:** Before executing the command, you can prepare it in a plain text editor for easier editing. This will allow you to review and edit the complete command (including your key name) before copying and pasting it into the console.

You only need to generate your gentx directly:

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

**⚠️ Important:** Replace `<your-key-name>` with the exact name of your key that you verified earlier. We suggest editing or making the corresponding edit in a plain text file to then simply copy and paste the command into your terminal for an easier process.

**Specific parameters for Creative:**
- **Chain ID:** `infinite_421018002-1` (Creative)
- **Self-delegation:** `10000000000000000000cdrop` (10 tokens)
- **Initial commission rate:** `0.01` (1%)
- **Maximum commission rate:** `0.05` (5%)
- **Maximum rate change:** `0.01` (1%)
- **Minimum self-delegation:** `1000000000000000000` (1 token)

### Step 5: Validate your New Gentx

Before delivering your gentx, validate that it works correctly:

```bash
# Collect gentxs (includes yours)
infinited genesis collect-gentxs --home ~/.infinited

# Validate the resulting genesis
infinited genesis validate-genesis --home ~/.infinited
```

If validation is successful, your gentx is ready to deliver.

### Step 6: Extract and Send your New Gentx

1. **Locate your gentx file:**
   ```bash
   ls -la ~/.infinited/config/gentx/
   ```
   
   You will see a file with format `gentx-<hash>.json`. Note the complete name of this file.

2. **Extract the gentx file from the server:**
   
   The gentx file is stored in the Docker persistent volume, so it is accessible from the host system without needing to copy it manually.
   
   **If you are working directly on the server:**
   ```bash
   cd services/node2-infinite-creative
   ls -la persistent-data/config/gentx/
   
   # Copy maintaining the original name (replace <hash> with the actual hash of your file)
   mkdir -p ~/gentx-round-1-v2
   cp persistent-data/config/gentx/gentx-<hash>.json ~/gentx-round-1-v2/
   ```
   
   **If you are on a remote server**, you need to download the file to your local computer using `scp`:
   ```bash
   # From your local computer (replace <user>, <server>, <hash> and the path according to your configuration)
   mkdir -p ~/gentx-round-1-v2
   scp <user>@<server>:/path/to/drive/services/node2-infinite-creative/persistent-data/config/gentx/gentx-<hash>.json ~/gentx-round-1-v2/
   ```
   
   **Explanation of the `scp` command:**
   - `<user>`: This is the username you use to log in to your server
   - `<server>`: Refers to the IP address or domain name of your server (for example: `192.168.1.100` or `my-server.com`)
   - The path after the colon (`:`) is the complete path to the file on the server
   - `~/gentx-round-1-v2/` is the destination directory on your local computer
   
   **⚠️ Important:** When executing this command, it is very likely that the system will ask you for credentials or authorization to perform the transfer. These are the same credentials you use when logging into your server (password or SSH key).
   
   **Complete example:** If your user is `ubuntu`, your server has the IP `192.168.1.100`, your working directory is `/home/ubuntu/drive`, and your file is named `gentx-adba573456c82908c3221163185703c421a2dd1f.json`:
   ```bash
   mkdir -p ~/gentx-round-1-v2
   scp ubuntu@192.168.1.100:/home/ubuntu/drive/services/node2-infinite-creative/persistent-data/config/gentx/gentx-adba573456c82908c3221163185703c421a2dd1f.json ~/gentx-round-1-v2/
   ```

3. **Compress the gentx file:**
   Once you have the gentx file on your local computer, compress it with a name that includes your moniker to facilitate identification:
   
   ```bash
   cd ~/gentx-round-1-v2
   tar -czf gentx-<your-moniker>-round-1-v2.tar.gz gentx-<hash>.json
   
   # Or using zip
   zip gentx-<your-moniker>-round-1-v2.zip gentx-<hash>.json
   ```
   
   **Example:** If your moniker is `my-validator` and your file is named `gentx-adba573456c82908c3221163185703c421a2dd1f.json`:
   ```bash
   cd ~/gentx-round-1-v2
   tar -czf gentx-my-validator-round-1-v2.tar.gz gentx-adba573456c82908c3221163185703c421a2dd1f.json
   ```
   
   **⚠️ Important:**
   - The compressed file must include your moniker in the name to facilitate identification
   - The JSON gentx file inside the compressed file must maintain its original name (with the hash, do not rename it)

4. **Send via Telegram:**
   - Send the compressed file via Telegram to **Cypher Xenia** with username **@XeniaCypher88**
   - **Include in the message:** "Gentx v2 for Round 1 correction - Moniker: [your-moniker]"

---

## 📝 Process Summary

```
1. Delete previous gentx
   ↓
2. Download base Genesis v2
   ↓
3. Verify base Genesis v2 (Chain ID and your account)
   ↓
4. Validate base Genesis v2
   ↓
5. Generate new gentx (without adding account)
   ↓
6. Validate new gentx
   ↓
7. Extract new gentx from server
   ↓
8. Compress gentx with moniker
   ↓
9. Send compressed gentx via Telegram
```

---

## ❓ Frequently Asked Questions

### What happens if I don't delete the previous gentx folder?

If you don't delete the gentx folder before downloading the base Genesis v2, there may be conflicts. It is important to delete the entire gentx folder to start with a clean state.

### Can I use the same gentx I created previously?

No, you must create a new gentx based on the base Genesis v2. The previous gentx was created with a different Genesis and will not be valid with the new base Genesis v2.

### What happens if my account is not in the base Genesis v2?

If your account is not in the base Genesis v2, it means you did not send your edited Genesis file in Phase 1, or there was a problem processing it. Contact the development team to resolve this issue.

### When should I send my new gentx?

You should send your new gentx as soon as possible now that the base Genesis v2 is available. The team will compile all received gentxs to create the final Genesis.

---

## 📚 References

- [Original Round 1 - First Flow](/en/posts/launch-round-1/) - Original Round 1 document
- [Round 1 Correction](/en/posts/launch-round-1-correction/) - Phase 1: Sending the edited Genesis file
- [Complete Guide: Create Gentx](https://docs.infinitedrive.xyz/en/blockchain/genesis/create-gentx/) - Complete technical documentation

---

**Thank you for your patience and collaboration in this correction process.** Your participation is essential for the success of the Creative chain launch.

