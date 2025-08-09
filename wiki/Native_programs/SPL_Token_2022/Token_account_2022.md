# Token Account 2022

A Token Account is a user-owned account that holds balances of a specific SPL token.
In Token-2022, token accounts support extensions, enabling features like transfer fees, confidential transfers, or permanent delegation.

Each account is linked to a particular Mint, and an owner who controls it.
---

## Structure

**Encoding:** `POD` + `TLV`

**Size:** >= 166 bytes

| Name | Type | Description |
| ---- | ---- | ----------- |
| `mint` | `Pubkey` | The SPL Token mint this account is associated with. |
| `owner` | `Pubkey` | The account's owner (who controls its tokens). |
| `amount` | `u64` | Token balance in base units. |
| `delegate` | `COption<Pubkey>` | Optional delegate allowed to transfer tokens on behalf of the owner. |
| `state` | `AccountState` | The status of the token account (e.g., active or frozen). |
| `is_native` | `COption`<`u64`> | Present if this account wraps native SOL. Holds rent-exempt reserve value. |
| `delegated_amount` | `u64` | The amount the delegate is allowed to transfer. |
| `close_authority` | `COption<Pubkey>` | Optional authority allowed to close the account and withdraw rent. |
| `account_type` | `AccountType` | Stored at byte offset `165`. Distinguishes between Mint and Token accounts. |
| `extensions` | TLV entries | Optional extensions activated for this account. |



## AccountState

The state field is represented as an enum with a 1-byte discriminant in on-chain data:

| Value | Variant | Description |
| ----- | ------- | ----------- |
| `0` | `Uninitialized` | The account has not yet been initialized (data is not valid yet). |
| `1` | `Initialized` | The account is active and can be used by the owner or delegate. |
| `2` | `Frozen` | The account has been frozen by the mint’s freeze authority. No transfers or operations are allowed. |

## AccountType

The `account_type` field is located at byte offset 165 for both Mint and Token accounts, ensuring unified parsing logic for extensions.

| Value | Variant | Description |
| ----- | ------- | ----------- |
| `0` | `Uninitialized` | The account has not been initialized. |
| `1` | `Mint` | A mint account with optional extensions. |
| `2` | `Account` | A token holding account with optional extensions. |