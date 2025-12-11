---
title: "Testing Phase: Preparation with Drive and Key Management"
date: 2024-01-25T10:00:00Z
tags: ["testing", "drive", "keys", "security", "preparation"]
---

# Testing Phase: Preparation with Drive and Key Management

Before starting the testing rounds, it is essential to properly prepare our environment and understand the critical importance of secure management of keys and seed phrases.

## Installing Drive

**Drive** is the client tool developed by the Lab that allows the management of multiple nodes and services in a unified way.

Drive Resources

- **Repository**: [github.com/deep-thought-labs/drive](https://github.com/deep-thought-labs/drive)
- **Documentation**: Technical documentation is available in the repository and is being continuously refined to make it more accessible.

Drive Features

Drive allows you to:
- Manage multiple nodes
- Manage services
- Coming soon: support for QL1 nodes, allowing you to run both validators on the same server

**Note**: Currently, the operational demand for both networks is very low, so running them on the same server is perfectly feasible, avoiding duplicate costs.

## Key Management: Critical Aspects

### ⚠️ Importance of Seed Phrases

Proper key management is **absolutely critical** for the security of your validator. Pay special attention to these points:

### Correct Initialization Process

1. **Create your keys** using Drive and the key generation utility.
2. **Learn how to store them** correctly.
3. **Save your seed phrase offline** — preferably on paper.
4. **Practice initializing the node** using the key you already have.

### Critical Verification: priv_validator_key

The most important value to verify is that the `priv_validator_key` file generated in your configuration folder after initializing the node **is always the same value** when you initialize your node with your recovery key.

**This is the most important thing**: Make sure it's always the same value every time you use the same recovery key.

### Recommended Verification Process

Before executing the "create-validator" transaction, you must:

1. Initialize a node two or three times with `--recover`, using the same seed phrase.
2. Ensure you can always generate the same `priv_validator_key`.
3. Know your exact and correct `priv_validator_key` and verify that it is present in your configuration file.
4. Only then execute the "create-validator" transaction.

### ⚠️ Important Warning

**If you create a validator but did NOT use a seed phrase in the node's Init (with `--recover`), there is NO way to regenerate the exact `priv_validator_key` again.**

This poses a risk: **If you lose the `priv_validator_key` file, you also lose your validator.**

### Understanding priv_validator_key

It is important to understand the purpose of `priv_validator_key`:

- The validator uses it to **sign its blocks**
- It does NOT have the power to move, delegate, or perform any other operation that belongs to the token holder
- Those operations are the responsibility of the keys that are added to the Key Ring in a separate process

### Best Practices

**Ideally**, the keys used in the Key Ring and the `priv_validator_key` generated during the INIT process should use **the same seed phrase**.

## Recommended Practice

We recommend that everyone practice with their node using Drive and the graphical interface utility for key generation:

- Create your keys
- Learn how to store them
- Save your seed phrase offline on paper
- Practice initializing the node using that key

## Next Steps

Once you have Drive installed and have practiced key management, we'll be ready for the first round of testing: **the Chain Launch**.

---

**Remember**: The security of your validator depends entirely on the correct management of your keys. Take the time to practice and verify that everything works correctly before proceeding.

