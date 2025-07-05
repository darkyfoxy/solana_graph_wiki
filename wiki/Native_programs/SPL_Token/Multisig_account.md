# Multisig Account

The **Multisig Account** is used in the [SPL Token Program](https://spl.solana.com/token) to represent an account controlled by multiple signers.

It allows a group of public keys to act collectively as an authority for token operations, such as minting, transferring, or setting authorities — depending on the configuration.

---

## Structure

**Encoding:** `POD`
**Size:** 355 bytes (1 + 1 + 1 + 11×32)

| Name | Type | Description |
|------|------|-------------|
| `m` | `u8` | Number of required signatures |
| `n` | `u8` | Number of total valid signers |
| `is_initialized` | `bool` | Indicates if the account is usable |
| `signers` | `[`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`; 11]` | Array of signer public keys |
