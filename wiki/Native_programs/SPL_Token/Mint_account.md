# Mint Account

The **Mint Account** is a special on-chain account used in the [SPL Token Program](https://spl.solana.com/token) to define the **properties of a token** — like its total supply, decimal precision, and authorities that control minting or freezing.

It is the central source of truth for any SPL token.

---

## Structure

**Encoding:** `POD`

**Size:** 82 bytes

Here is the layout of a Mint account as defined in the SPL Token Program:

| Name              | Type                                                                                                                                           | Description                                                                              |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- |
| `mint_authority`   | [`COption`](https://wiki.solanagraph.com/Basic_structures/COption.md)<[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)> | Who can mint new tokens. If `None`, the token supply is fixed.                           |
| `supply`           | `u64`                                                                                                                                          | Total number of tokens minted (in base units).                                           |
| `decimals`         | `u8`                                                                                                                                           | Number of decimal places (e.g. `6` means 1 token = `1_000_000` base units).              |
| `is_initialized`   | `bool`                                                                                                                                         | Indicates if this mint account has been initialized properly.                            |
| `freeze_authority` | [`COption`](https://wiki.solanagraph.com/Basic_structures/COption.md)<[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)> | Optional authority that can freeze token accounts (typically for compliance or control). |


