# Mint Account 2022

The Mint Account in the **token-2022** program defines the core properties of an SPL token, including supply, decimal precision, and authorities (e.g. minting, freezing). It also supports TLV-based extensions, allowing custom behaviors such as transfer fees, close authority, metadata pointers, and more.

This account must be properly sized to include any desired extensions before initialization.

---

## Structure

**Encoding:** `POD` + `TLV`

**Size:** >= 166 bytes

This is the structure of a Mint Account in the Token-2022 program. It consists of a base POD layout followed by a TLV section for extensions.

| Name | Type | Description |
| ---- | ---- | ----------- |
| `mint_authority` | `COption<Pubkey> `| Who can mint new tokens. If `None`, the token supply is fixed. |
| `supply` | `u64` | Total number of tokens minted (in base units). |
| `decimals` | `u8` | Number of decimal places (e.g. `6` means 1 token = 1,000,000 base units). |
| `is_initialized` | `bool` | Indicates if this mint account has been initialized properly. |
| `freeze_authority` | `COption<Pubkey> `| Optional authority that can freeze token accounts (typically for compliance or control). |
| `padding` | `[u8; 83]` | Reserved padding bytes to ensure the account is 165 bytes — aligning `AccountType` offset across types. |
| `account_type` | `AccountType` | Enum that defines whether this account is a `Mint`, `TokenAccount`, or `Uninitialized`. |
| `extensions` | TLV entries | Optional data entries representing enabled extensions for this account. |


## AccountType

The AccountType enum is used to identify the kind of Token-2022 account, and is stored at offset 165, right after the fixed POD layout.

| Value | Variant | Description |
| ----- | ------- | ----------- |
| `0` | `Uninitialized` | Account is not initialized. Should not contain usable data. |
| `1` | `Mint` | Account represents a token mint with optional extensions. |
| `2` | `Account` | Account represents a token holding account (`TokenAccount`) with extensions. |

**Why offset 165?**

- The base size of a TokenAccount is 165 bytes
- The base size of a Mint is 82 bytes, but in Token-2022 it is padded with 83 extra bytes, making it also 165 bytes
- This design ensures that both TokenAccount and Mint accounts use the same offset to store the AccountType and start the TLV section

This consistent offset simplifies parsing and avoids ambiguity between account types.

