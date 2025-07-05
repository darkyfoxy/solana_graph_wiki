# 🔑 Public Key (Pubkey)

In Solana, most things — accounts, programs, authorities — are identified by a **public key**. This is a 32-byte value that uniquely identifies something.

---

## What is a Pubkey

Internally, a `Pubkey` is represented as:

| Field | Type | Description |
|-------|------|-------------|
| `Pubkey` | `[u8; 32]` | A 32-byte public key identifier |

This is simply an array of 32 bytes.

---

## Base58 Representation

When you see a public key in RPC responses, explorer URLs, or logs — it is usually encoded as a **Base58 string**, for example:

`4Nd1mYvFgfqXtXrU7u7A57q1Hgk3uZK9RfxyMvFqWxiT`

Solana uses Base58 encoding widely in RPC, logs, and user interfaces — but internally it's always just 32 bytes in memory.

---

## NonZeroPubkey

In many places, Solana code and programs use NonZeroPubkey to represent optional public keys.

A **NonZeroPubkey** is conceptually the same as a **Pubkey**, but:

- If all bytes are zero (`[0u8; 32]`), it is considered "absent" or "unset"

- This is not the same as the **System Program** address (System Program address: `11111111111111111111111111111111` -> [`0u8; 32`])


