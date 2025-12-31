---
title: "Launch Round 1 - Preparation for Launch Day"
date: 2025-12-30T00:00:00Z
draft: false
tags: ["testing", "gentx", "creative", "chain-launch", "round-1", "preparation", "launch-day"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'Launch Round 1 - Preparation for Launch Day'
---

{{< figure src="cover" caption="alt" >}}

This document contains the **final preparation instructions** and all the information needed for the launch day of Round 1 of the **Infinite Improbability Drive** chain.

> 📖 **Context:** This document is a continuation of the Round 1 process. Make sure you have completed the previous steps:
> - [Original Round 1](/en/posts/launch-round-1/) - Initial process
> - [Round 1 Correction](/en/posts/launch-round-1-correction/) - Submission of the edited Genesis file

---

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px; background-color: rgba(0, 200, 0, 0.1);">

## ✅ Status: Everything Ready for Launch

**All participants have successfully completed the preparation phase:**

- ✅ All participants submitted their node information (IP, subdomain, Node ID)
- ✅ The team configured all secure domains in Cloudflare
- ✅ The team compiled the final Genesis with all valid gentx
- ✅ Seed nodes and persistent peers values are ready
- ✅ The Genesis download script is available

**🎯 The only thing left is to complete the final configuration on your node and be ready to start at the agreed launch synchronized time.**

</div>

---

## 📋 Information Requested from Participants (Completed)

As part of the preparation process, all participants were asked to provide the following information and complete certain configurations:

### Information Submitted by Participants

1. **Server IP** - IP address of the server where the node is hosted
2. **Desired subdomain name** - Format: `server-YOUR-NAME` (example: `server-red`)
3. **Node ID** - Obtained using the command `infinited comet show-node-id`

**✅ All participants successfully completed the submission of this information.**

### Requested Configurations

1. **Firewall configuration** - To protect servers and allow necessary connections
2. **Obtain Node ID** - Unique node identifier for network configuration

**✅ All participants successfully completed these configurations.**

---

## 🔧 Configurations You Must Perform on Your Node

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px; background-color: rgba(255, 193, 7, 0.1);">

### ⚠️ IMPORTANT: Review Your Configuration

**Participants should have these configurations completed before launch day.**

If you haven't done so yet, **make sure you have it done and correct**. Please review and ensure that all configured values are as described in this guide.

</div>

### 🔴 Important Rules for Modifying Files

**⚠️ CRITICAL - Read this before continuing:**

1. **To modify `config.toml`:**
   - ✅ **You do NOT need to stop the service** - You can edit the file while the service is running
   - ✅ The service can continue running during editing

2. **To modify `docker-compose.yml`:**
   - ⛔ **You MUST stop the service BEFORE** editing the file
   - ✅ Edit the file and save the changes
   - ✅ **You MUST start the service AFTER** saving the changes

3. **For operations inside the container (bash):**
   - ✅ **The service MUST be running** - Do not stop it before doing operations inside the container
   - ✅ Use `./drive.sh exec infinite-creative bash` to access the container

---

### Step 1: Firewall Configuration

Before continuing with any other configuration, it is **CRITICAL** to correctly configure the firewall to protect your server and allow necessary connections.

**🔴 IMPORTANT 🔴** 
Make sure to **allow the SSH port (22) first** before enabling the firewall. If you don't, you will lose access to your server console.

Follow these guides in order:

1. **General Firewall Configuration**
   - [Firewall Configuration Guide](https://docs.infinitedrive.xyz/en/drive/services/ports/firewall-configuration/)

2. **Specific Configuration for Infinite Creative Network (node2-infinite-creative)**
   - [Firewall Configuration for node2-infinite-creative](https://docs.infinitedrive.xyz/en/drive/services/catalog/node2-infinite-creative/#firewall-configuration)

---

### Step 2: Modify config.toml

**✅ IMPORTANT:** To modify `config.toml`, **you do NOT need to stop the service**. You can edit the file while the service is running.

**💻 On your Host machine, make sure you are inside the infinite-creative service folder:**
```bash
cd drive/services/node2-infinite-creative
```

**Open the configuration file with Nano to edit it:**
```bash
nano persistent-data/config/config.toml
```

You need to make two important modifications to the configuration file.

#### 2.1: Configure laddr for RPC Access

This configuration will allow you to see your node's RPC data from the web, not just from inside your server. It is very useful for essential validations.

Find the `[rpc]` section and configure `laddr` to use `0.0.0.0` as the address. Port `26657` is fine.

**Edit the value so it looks like this:**
```toml
##########################################
###  RPC Server Configuration Options  ###
##########################################
[rpc]

# TCP or UNIX socket address for the RPC server to listen on
laddr = "tcp://0.0.0.0:26657"
```

#### 2.2: Disable PEX

In the same configuration file, go to the `[p2p]` section and find the `pex` option. It must be configured to `false`.

This avoids a problem with disconnections in small chains (like our case).

**Configure the value like this:**
```toml
###################################
###  P2P Configuration Options  ###
###################################
[p2p]
...
pex = false
```

**To save changes in Nano:**
- Press `Ctrl + O` to save
- Press `Enter` to confirm
- Press `Ctrl + X` to exit

**✅ Once these changes are saved, they will take effect when the node starts (or when it stops and restarts if the node was already running).** It is important to distinguish between the **node** (the binary that runs inside the container) and the **container** (the Docker service). In this case, all modifications we are making to the `config.toml` file are being done with the node stopped, since it will not start until the agreed launch time. However, if in the future you want to modify your `config.toml` file for any reason, **you need to stop and restart the node** for the changes to take effect.

---

## 📋 Ready Configuration Values

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px; background-color: rgba(0, 123, 255, 0.1);">

### 🔗 Seed Nodes and Persistent Peers

The values for `NODE_P2P_SEEDS` and `NODE_PERSISTENT_PEERS` are ready and available in the **official service documentation**. All participants have provided their information and these values are correctly configured.

**📍 Location in the documentation:**

The complete values are available in the **"Network P2P Configuration"** section of the official documentation:

**[Network P2P Configuration - Infinite Creative Network](https://docs.infinitedrive.xyz/en/drive/services/catalog/node2-infinite-creative/#network-p2p-configuration)**

In this section you will find:
- A complete table with all trusted nodes (Node ID, address, port)
- Complete `NODE_P2P_SEEDS` values ready to copy
- Complete `NODE_PERSISTENT_PEERS` values ready to copy
- Detailed information about each configuration variable

**💡 How to get the values:**

1. Access the [official documentation](https://docs.infinitedrive.xyz/en/drive/services/catalog/node2-infinite-creative/#network-p2p-configuration)
2. Find the **"Network P2P Configuration"** section
3. Expand the **"Network P2P Configuration Variables"** section
4. Copy the complete values of `NODE_P2P_SEEDS` and `NODE_PERSISTENT_PEERS`
5. Paste these values in your `docker-compose.yml` file

**⚠️ NOTE:** The values for `NODE_P2P_SEEDS` and `NODE_PERSISTENT_PEERS` are identical. Copy the same value for both variables.

### 📥 Final Genesis URL

The final Genesis is available at the following URL:

```
https://assets.infinitedrive.xyz/tests-round1/genesis-final.json
```

**Expected Chain ID:** `infinite_421018002-1`

</div>

---

### Step 3: Configure docker-compose.yml

**⛔ IMPORTANT:** To modify `docker-compose.yml`, **you MUST stop the service BEFORE** editing the file.

#### 3.1: Stop the Service

**First, stop the infinite-creative service:**

```bash
cd drive/services/node2-infinite-creative
./drive.sh down
```

#### 3.2: Open the docker-compose.yml File

**Open the docker-compose file with Nano to edit it:**
```bash
nano docker-compose.yml
```

#### 3.3: Configure NODE_P2P_EXTERNAL_ADDRESS

This is the address that was previously delivered to each participant based on the subdomain they chose. The port is the one corresponding to this creative network (26676).

Find the `NODE_P2P_EXTERNAL_ADDRESS` variable in the P2P network configuration section and configure the value with your subdomain and port:

**Must have the format:**
```
YOUR-SUBDOMAIN.infinitedrive.xyz:26676
```

**⚠️ IMPORTANT:** Make sure that the `NODE_P2P_EXTERNAL_ADDRESS` line is **NOT commented** in your docker-compose.yml file. If the line is commented (with `#` at the beginning), uncomment it before modifying the value. If you only modify the value but the line remains commented, it will have no effect.

**Example of how it should look in your docker-compose.yml:**
```yaml
###############################
#  Network P2P Configuration  #
###############################

NODE_P2P_EXTERNAL_ADDRESS: "server-red.infinitedrive.xyz:26676"
# NODE_P2P_SEEDS: ""
# NODE_PERSISTENT_PEERS: ""
```

#### 3.4: Configure NODE_P2P_SEEDS and NODE_PERSISTENT_PEERS

Both variables (`NODE_P2P_SEEDS` and `NODE_PERSISTENT_PEERS`) are found in the same location in the official documentation. In that location you can copy and paste each of the fields to replace them in your docker-compose.yml file.

**First, get the values from the documentation:**

1. Access the [official documentation - Network P2P Configuration](https://docs.infinitedrive.xyz/en/drive/services/catalog/node2-infinite-creative/#network-p2p-configuration)
2. Expand the **"Network P2P Configuration Variables"** section
3. Copy the complete value of `NODE_P2P_SEEDS`
4. Copy the complete value of `NODE_PERSISTENT_PEERS`

**⚠️ IMPORTANT:** Although both variables have the same values, **you must copy and paste each one individually** so that each one has its correct key-value in the docker-compose.yml file.

**Then, in your docker-compose.yml file:**

1. Find the `NODE_P2P_SEEDS` variable and paste the value copied from the documentation
2. Find the `NODE_PERSISTENT_PEERS` variable and paste the value copied from the documentation

**⚠️ IMPORTANT:** Make sure that the `NODE_P2P_SEEDS` and `NODE_PERSISTENT_PEERS` lines are **NOT commented** in your docker-compose.yml file. If the lines are commented (with `#` at the beginning), uncomment them before modifying the values. If you only modify the values but the lines remain commented, it will have no effect.

**Example of how it should look in your docker-compose.yml:**
```yaml
###############################
#  Network P2P Configuration  #
###############################

NODE_P2P_EXTERNAL_ADDRESS: "server-red.infinitedrive.xyz:26676"
NODE_P2P_SEEDS: "dd5689375610aaa35b69ed311d69e51ea5557474@server-red.infinitedrive.xyz:26676,e5c1b7423d098c660bb82b7f44f86e333cb6af9e@server-farmer.infinitedrive.xyz:26676,..."
NODE_PERSISTENT_PEERS: "dd5689375610aaa35b69ed311d69e51ea5557474@server-red.infinitedrive.xyz:26676,e5c1b7423d098c660bb82b7f44f86e333cb6af9e@server-farmer.infinitedrive.xyz:26676,..."
```

**Note:** The values in the example are truncated with `...` to show the format. You will find the complete real values in the official documentation.

**To save changes in Nano:**
- Press `Ctrl + O` to save
- Press `Enter` to confirm
- Press `Ctrl + X` to exit

#### 3.6: Start the Service

**After saving the changes, start the service again:**

```bash
./drive.sh up -d
```

**✅ The service is now configured with the correct seed nodes and persistent peers values.**

---

### Step 4: Download and Replace the Final Genesis

The final Genesis is ready and available. You must download it and replace your current Genesis file before launch.

**✅ IMPORTANT:** The service must be running to access the container's bash. You do not need to stop the service.

#### 4.1: Access the Container Bash

Access the container bash:

```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative bash
```

#### 4.2: Download the Final Genesis

Inside the container, download the final Genesis:

```bash
# Download the final Genesis (use the URL from the "Ready Configuration Values" section)
curl -o ~/.infinited/config/genesis.json https://assets.infinitedrive.xyz/tests-round1/genesis-final.json
```

#### 4.3: Verify the Downloaded Genesis

Inside the container, verify that the Genesis was downloaded correctly:

```bash
# Verify the Chain ID
cat ~/.infinited/config/genesis.json | jq -r '.chain_id'

# Expected Chain ID for Creative: infinite_421018002-1

# Validate the Genesis
infinited genesis validate-genesis --home ~/.infinited
```

If validation is successful, the Genesis is ready.

**Exit the container:**
```bash
exit
```

**✅ Your node is now configured with the official final Genesis.**

---

## 📝 Configuration Steps Summary

```
FINAL CONFIGURATION FOR LAUNCH:
1. Verify firewall configuration ✅
   ↓
2. Modify config.toml ⬅️ Does NOT require stopping service
   (RPC laddr and pex = false)
   ↓
3. Stop service ⛔
   ↓
4. Modify docker-compose.yml ⬅️ REQUIRES service stopped
   (P2P External Address, Seed Nodes, Persistent Peers)
   ↓
5. Start service ✅
   ↓
6. Access container bash ✅
   (service running)
   ↓
7. Download final Genesis inside container ⬅️ REQUIRES service running
   ↓
8. Verify downloaded Genesis inside container
   ↓
9. Exit container
   ↓
10. Wait for the agreed time to start the node! 🚀
```

---

## ⏰ Launch Synchronization

### 🎉 Special Round 1 Launch

For this initial launch of Round 1, which is a test chain, we have decided to do it in a special way to commemorate this historic moment.

**📅 Launch Date and Time:**

**January 1, 2026 at 0:00 UTC-0** - The first minute of the new year, the first Thursday of the year.

**⏰ Important Information about the Launch:**

- **It is not critical or mandatory** that 100% of participants start their node exactly at the same time
- Participants who have the opportunity to do so at that time should do so
- Participants who cannot do so at that exact time can do so at any time after that hour
- Your node will synchronize with the network and begin validating normally

**🚀 To start your node:**

You have two options to start your node:

**Option 1: Using the Graphical Interface (Recommended)**

1. Open the graphical interface (see [Graphical Interface](https://docs.infinitedrive.xyz/en/drive/guides/blockchain-nodes/graphical-interface/))
2. Navigate: Main Menu → **"Node Operations"** → **"Start Node"**
3. The interface will show the startup process and confirm when the node is running

![Start Node](https://docs.infinitedrive.xyz/images/node-ui-operations-op1-start.png)

**Option 2: Using Command Line**

```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-start
```

**📖 For more information on how to start and stop nodes, consult the [complete Start/Stop Node guide](https://docs.infinitedrive.xyz/en/drive/guides/blockchain-nodes/start-stop-node/).**

---

### 📊 View Node Logs

After starting your node, it is important to monitor the logs to verify that everything is working correctly and observe the synchronization progress.

**You have two ways to view logs:**

1. **View logs in real-time** - To monitor node activity while it is running
2. **View the last N lines of logs** - Useful when there was an error and you want to see what happened from the start of the node

**Access to log options via Graphical Interface:**

1. Open the graphical interface (see [Graphical Interface](https://docs.infinitedrive.xyz/en/drive/guides/blockchain-nodes/graphical-interface/))
2. Navigate: Main Menu → **"Node Monitoring"**

   ![Node Monitoring Menu](https://docs.infinitedrive.xyz/images/node-ui-monitoring.png)

3. In the **"Node Monitoring"** submenu, you will find the options to view logs

---

#### View Logs in Real-Time

**Option 1: Using the Graphical Interface (Recommended)**

In the **"Node Monitoring"** submenu, select **"Follow Logs"**. Logs will begin to display in real-time. To stop following, press `Ctrl+C`.

**Option 2: Using Command Line**

```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-logs -f
```

Or using the alternative syntax:
```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-logs --follow
```

**What it does:** Streams log entries in real-time as they are written to the log file (similar to `tail -f`).

**Expected output:** Shows a continuous stream of log entries. Press `Ctrl+C` to stop.

**When to use:** Monitor node activity while it is running, observe synchronization progress in real-time, or debug problems as they occur.

---

#### View the Last N Lines of Logs

This option is useful when for some reason there was an error and you want to see what happened or what the node logs were from the start. To see from the start of node operation, especially if there was an error in execution, it is recommended to view the last 200 lines, as in the first 200 lines you could see what happened.

**You have two options to view the last N lines of logs:**

**Option 1: Using the Graphical Interface**

In the **"Node Monitoring"** submenu, select **"View Logs"**. The interface will show the last 50 lines of logs by default.

**Note:** If you need to see a specific number of lines (for example, 100 or 200 lines), you must use the command line (Option 2).

**Option 2: Using Command Line**

To view the last 50 lines (default):
```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-logs
```

To view the last 100 lines:
```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-logs 100
```

To view the last 200 lines (recommended to see from the start of the node):
```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-logs 200
```

**What it does:** Shows the last N lines of the node log file (`/var/log/node/node.log`).

**Expected output:** Recent log entries showing:
- Node startup messages
- Synchronization progress
- Block processing
- Errors or warnings
- Connection status

**When to use:** To review recent node activity, verify errors from the start of execution, or review synchronization progress when you don't need to see logs in real-time.

**📖 For more information on node monitoring, consult the [complete Node Monitoring guide](https://docs.infinitedrive.xyz/en/drive/guides/blockchain-nodes/node-monitoring/).**

### 📅 Future Launches

For future launches (Testnet and Mainnet), the recommended schedule of **4:00 PM - 4:10 PM (UTC-0)** will be used, which is the time of maximum participant availability according to the analysis performed.

**✅ Make sure you have completed all configuration steps before the launch time.**

---

## 🔐 Security: Use of Secure Domains

To ensure this process is secure, **we do not share IP addresses directly**. Instead, we use **secure domains through Cloudflare**.

**Instead of using the IP address `123.456.789`, we use:**
```
server-red.infinitedrive.xyz
```

In this way, when defining the seed nodes for each of us in the configuration files, we use a format like this:

```
node_id@server-red.infinitedrive.xyz:26656
```

This provides:
- ✅ Greater security (we don't expose IPs directly)
- ✅ Flexibility (easy IP change without updating configurations)
- ✅ Better network management

---

## ❓ Frequently Asked Questions

### What happens if I don't complete these steps before launch?

If you don't complete these steps, your node will not be able to connect correctly with other nodes on the network, which will prevent consensus and participation in the launch. It is **critical** to complete the configuration of seed nodes, persistent peers, and the final Genesis before the agreed time.

### Can I start my node before the agreed time?

No. It is essential that all nodes start simultaneously at the agreed time to ensure a successful and synchronized launch.

### Can I start my node after the agreed launch time?

Yes. If you cannot start your node exactly at the agreed time (January 1, 2026 at 0:00 UTC-0), you can start it at any time after that hour. Your node will automatically synchronize with the network and begin validating normally once it completes synchronization.

### Do I need to stop the service to modify config.toml?

No. You can modify `config.toml` while the service is running. However, it is important to distinguish between the **node** (the binary that runs inside the container) and the **container** (the Docker service). Changes to `config.toml` take effect when the node starts or when it stops and restarts if the node was already running. In this case, all modifications are being made with the node stopped, since it will not start until the agreed launch time. If in the future you want to modify your `config.toml` file for any reason, **you need to stop and restart the node** for the changes to take effect.

### Do I need to stop the service to modify docker-compose.yml?

Yes. **You MUST stop the service before** modifying `docker-compose.yml` and **you MUST start it after** saving the changes.

### Can I do operations inside the container with the service stopped?

No. To access the container bash and do operations inside it, **the service MUST be running**. Use `./drive.sh exec infinite-creative bash` when the service is running.

### Can I use my IP directly instead of a subdomain?

Yes, it is technically possible to use an IP address directly. However, **we recommend not showing them** for security and flexibility reasons. For this reason, all participants must use subdomains configured by the team through Cloudflare.

---

## 📚 Technical References

- [Complete Guide: Create Gentx](https://docs.infinitedrive.xyz/en/blockchain/genesis/create-gentx/) - Complete technical documentation
- [Firewall Configuration](https://docs.infinitedrive.xyz/en/drive/services/ports/firewall-configuration/) - General firewall guide
- [Infinite Creative Network - Service Documentation](https://docs.infinitedrive.xyz/en/drive/services/catalog/node2-infinite-creative/) - Complete service documentation
- [Network P2P Configuration](https://docs.infinitedrive.xyz/en/drive/services/catalog/node2-infinite-creative/#network-p2p-configuration) - Seed nodes and persistent peers values (official documentation)

---

## 🔗 Related Documents - Round 1

This section contains all documents related to Round 1, in chronological order:

- **[Testing Phase Guide](/en/posts/testing-phase-guide/)** - General testing phase guide (starting point)
- **[Launch Round 1](/en/posts/launch-round-1/)** - Original Round 1 document with the initial flow
- **[Launch Round 1 - Correction](/en/posts/launch-round-1-correction/)** - Correction process and submission of the edited Genesis

---

**Thank you for your preparation and collaboration!** All participants have successfully completed the preparation phase. Now all that remains is to complete the final configuration (seed nodes, persistent peers, and final Genesis) and be ready to start our nodes at the agreed synchronized launch time.

**🎯 Remember:** The success of the launch depends on all of us starting our nodes simultaneously at the agreed time. Let's all be ready!

