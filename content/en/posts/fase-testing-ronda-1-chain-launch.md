---
title: "Testing Phase - Round 1: Chain Launch"
date: 2025-12-04T10:00:00Z
tags: ["testing", "genesis"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'Testing Phase - Round 1: Chain Launch'
---

# Testing Phase - Round 1: Chain Launch

{{< figure src="cover" caption="alt" >}}

The first round of testing focuses on the most critical process in a chain's lifecycle: **the Chain Launch**.

## Importance of Chain Launch

This process is **essential and very important** in the supply chain's life cycle because:

- **It's only done once** in the chain's lifecycle
- It has to be **very well defined and standardized**
- During the testing phase, we'll perform it several times on our test chains
- **For the Mainnet, it will only be done once**
- That's why we must practice and have the procedure very clear so that when we do it on the Mainnet, everything works perfectly, like a Swiss watch

## Required Preparation

Before starting, each participant must have:

- ✅ Your node is initialized
- ✅ Your validation key is correctly created in the correct location
- ✅ You have practiced key management and verified the `priv_validator_key`

## Proceso del Chain Launch

### Step 1: Genesis Base

A base Genesis file will be provided that **does not yet contain any validation accounts**.

### Step 2: Creating Transactions

Each participant will use this Genesis file to create the transactions necessary to create their validator within it.

### Step 3: File Generation

Each participant will provide a file generated from their node's data.

### Step 4: Compiling the Final Genesis

All files from each participant will be collected and combined into a single Genesis.

### Step 5: Launching the Chain

This new Genesis file, created by compiling all the transactions performed separately, will be the Genesis of the chain.

When we all start our nodes using this new Genesis file, **we will all be validators of the newly created chain, starting from block 1**.

## Process Flow

```
1. Base Genesis (without validators)

↓
2. Each participant creates transactions for their validator

↓
3. Each participant generates a file with their node's data

↓
4. Compilation of all files into a single Genesis

↓
5. All participants start nodes with the final Genesis

↓
6. Chain launched with all participants acting as validators from block 1
```

## Specific Instructions

Specific instructions and detailed steps will be provided later. This post serves as preparation and context for the process.

## Objectives of this Round

- Validate the entire Chain Launch process
- Ensure all participants can correctly create validators
- Verify that the compiled Genesis works correctly
- Document any issues or necessary improvements in the process

Next Rounds

After this first round, we will continue with:

- Cyclical testing rounds (every Thursday)
- Testing of different functionalities
- Token movement
- Voting in the DAO
- Simulation of various scenarios

---

**This is the most important round** because it establishes the standard procedure we will use for the official Mainnet launch. Accuracy and attention to detail are crucial.

