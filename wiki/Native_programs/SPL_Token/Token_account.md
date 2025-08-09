# Token Account

A **Token Account** is a user-facing account used to hold balances of an SPL token.
Each Token Account is linked to a specific [**Mint**](https://wiki.solanagraph.com/Native_programs/SPL_Token/Mint_account.md) and an **owner**, and keeps track of the number of tokens held, delegated, or locked.

Token Accounts are created for each user (or contract) who wants to hold tokens, and they are **separate from the** [**Mint**](https://wiki.solanagraph.com/Native_programs/SPL_Token/Mint_account.md) itself.

---

## Structure

**Encoding:** `POD`

**Size:** 165 bytes

| Name | Type | Description |
| ---- | ---- | ----------- |
| `mint` | `Pubkey` | The SPL Token mint this account is associated with. |
| `owner` | `Pubkey` | The owner of the token account. |
| `amount` | `u64` | The number of tokens held in this account (in base units). |
| `delegate` | `COption<Pubkey>` | An optional delegate that can transfer tokens on behalf of the owner. |
| `state` | `AccountState` | The current state of the account (uninitialized, initialized, frozen). |
| `is_native` | `COption<u64>` | Used for wrapped SOL accounts. Contains rent-exempt reserve if present. |
| `delegated_amount` | `u64` | The amount of tokens the delegate is authorized to transfer. |
| `close_authority` | `COption<Pubkey>` | Optional authority allowed to close the account and reclaim rent. |


## AccountState

The state field is represented as an enum with a 1-byte discriminant in on-chain data:
| Value | Variant | Description |
| ----- | ------- | ----------- |
| `0` | `Uninitialized` | The account has not yet been initialized (data is not valid yet). |
| `1` | `Initialized` | The account is active and can be used by the owner or delegate. |
| `2` | `Frozen` | The account has been frozen by the mint’s freeze authority. No transfers or operations are allowed. |
