# SPL Token-2022

The **Token-2022 Program** is an extended version of the [SPL Token Program](https://wiki.solanagraph.com/Native_programs/SPL_Token/SPL_Token.md) that introduces new functionality via extensions.


## Instruction list

| ID | Instruction | Description |
| -- | ----------- | ----------- |
| 0 | [`InitializeMint`](#initializemint) | Creates a new mint with optional freeze authority. |
| 1 | [`InitializeAccount`](#initializeaccount) | Initializes a new token account. |
| 2 | [`InitializeMultisig`](#initializemultisig) | Initializes a multisig account with N signers. |
| 3 | [`Transfer`](#transfer) | Transfers tokens from one account to another. NOTE This instruction is deprecated in favor of `TransferChecked` or `TransferCheckedWithFee` |
| 4 | [`Approve`](#approve) | Approves a delegate to transfer tokens. |
| 5 | [`Revoke`](#revoke) | Revokes a delegate’s authority. |
| 6 | [`SetAuthority`](#setauthority) | Changes mint or account authority. |
| 7 | [`MintTo`](#mintto) | Mints new tokens to an account. |
| 8 | [`Burn`](#burn) | Burns tokens from an account. |
| 9 | [`CloseAccount`](#closeaccount) | Closes a token account and reclaims SOL. |
| 10 | [`FreezeAccount`](#freezeaccount) | Freezes a token account. |
| 11 | [`ThawAccount`](#thawaccount) | Thaws a frozen account. |
| 12 | [`TransferChecked`](#transferchecked) | Same as `Transfer`, with decimals check. |
| 13 | [`ApproveChecked`](#approvechecked) | Same as `Approve`, with decimals check. |
| 14 | [`MintToChecked`](#minttochecked) | Same as `MintTo`, with decimals check. |
| 15 | [`BurnChecked`](#burnchecked) | Same as `Burn`, with decimals check. |
| 16 | [`InitializeAccount2`](#initializeaccount2) | Initializes a token account (owner in data). |
| 17 | [`SyncNative`](#syncnative) | Syncs wrapped SOL token account with lamports. |
| 18 | [`InitializeAccount3`](#initializeaccount3) | Like `InitializeAccount2`, without Rent sysvar. |
| 19 | [`InitializeMultisig2`](#initializemultisig2) | Like `InitializeMultisig`, without Rent sysvar. |
| 20 | [`InitializeMint2`](#initializemint2) | Like `InitializeMint`, without Rent sysvar. |
| 21 | [`GetAccountDataSize`](#getaccountdatasize) | Returns the required size of a token account. |
| 22 | [`InitializeImmutableOwner`](#initializeimmutableowner) | Declares the account’s owner as immutable. |
| 23 | [`AmountToUiAmount`](#amounttouiamount) | Converts raw amount to UI string. |
| 24 | [`UiAmountToAmount`](#uiamounttoamount) | Converts UI string amount to raw amount. |
| 25 | [`InitializeMintCloseAuthority`](#) | Set close authority for a mint. |
| 26 | [`TransferFeeExtension`](#) | Entry point for Transfer Fee extension. |
| 27 | [`ConfidentialTransferExtension`](#) | Entry point for Confidential Transfer extension. |
| 28 | [`DefaultAccountStateExtension`](#) | Entry point for Default Account State extension. |
| 29 | [`Reallocate`](#) | Reallocate account for additional extensions. |
| 30 | [`MemoTransferExtension`](#) | Entry point for Memo Transfer extension. |
| 31 | [`CreateNativeMint`](#) | Create the wrapped SOL native mint. |
| 32 | [`InitializeNonTransferableMint`](#) | Make mint non-transferable. |
| 33 | [`InterestBearingMintExtension`](#) | Entry point for interest-bearing mint extension. |
| 34 | [`CpiGuardExtension`](#) | Entry point for CPI guard extension. |
| 35 | [`InitializePermanentDelegate`](#initializepermanentdelegate) | Set permanent delegate for a mint. |
| 36 | [`TransferHookExtension`](#) | Entry point for transfer hook extension. |
| 37 | [`ConfidentialTransferFeeExtension`](#) | Entry point for confidential fee extension. |
| 38 | [`WithdrawExcessLamports`](#) | Rescue SOL from token-owned account. |
| 39 | [`MetadataPointerExtension`](#) | Entry point for metadata pointer extension. |
| 40 | [`GroupPointerExtension`](#) | Entry point for group pointer extension. |
| 41 | [`GroupMemberPointerExtension`](#) | Entry point for group member pointer extension. |
| 42 | [`ConfidentialMintBurnExtension`](#) | Entry point for confidential mint/burn extension. |
| 43 | [`ScaledUiAmountExtension`](#) | Entry point for scaled UI amount extension. |
| 44 | [`PausableExtension`](#) | Entry point for pausable token extension. |

### AuthorityType

This enum is used by the `SetAuthority` instruction to specify which authority is being updated:


| Value | Variant | Description |
| ----- | ------- | ----------- |
| `0` | `MintTokens` | Authority to mint new tokens |
| `1` | `FreezeAccount` | Authority to freeze token accounts linked to the mint |
| `2` | `AccountOwner` | Authority that owns a token account |
| `3` | `CloseAccount` | Authority that can close a token account

## Instructions

### InitializeMint

The `InitializeMint` instruction is used to **create a new token mint** with a specified decimal precision and authorities. This is typically the first step when creating a new SPL token.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_authority_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `freeze_authority_pubkey` | `Option<`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`>` |
| `decimals` | `u8` |

### InitializeAccount

The `InitializeAccount` instruction is used to **initialize a token account** so it can hold tokens of a specific mint.
This is one of the most common instructions in the SPL Token program and must be run after creating the account with the system program.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `account_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `owner_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |


### InitializeMultisig

The `InitializeMultisig` instruction is used to **create a multisignature account** that can act as an authority in other token instructions.
This account requires M out of N signatures to authorize operations.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `multisig_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `m` | `u8` |

### Transfer

The `Transfer` instruction is used to move tokens from one account to another.
It supports both single-signer and multisig authorities.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `source_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `destination_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `amount` | `u64`

### Approve

The `Approve` instruction allows a delegate to transfer up to a specified amount of tokens on behalf of the token account owner.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `source_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `delegate_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `owner_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `amount` | `u64` |
### Revoke

The `Revoke` instruction removes a previously approved delegate from a token account.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `source_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `owner_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |


### SetAuthority

The `SetAuthority` instruction is used to change the authority of a token account or mint.
It can update the mint authority, freeze authority, account owner, or close authority.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `owned_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `new_authority_pubkey`| `Option<`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`>` |
| `authority_type` | [`AuthorityType`](#authoritytype) |
| `owner_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |


### MintTo

The `MintTo` instruction mints new tokens from a mint to a destination token account.
This instruction increases the total supply of the token.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `account_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `owner_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `amount` | `u64`

### Burn

The `Burn` instruction removes tokens from a token account, reducing the total supply of the mint.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `account_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `amount` | `u64` |


### CloseAccount

The `CloseAccount` instruction transfers all remaining SOL from a token account to a destination account and deallocates the token account.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `account_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `destination_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `owner_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |


### FreezeAccount

The `FreezeAccount` instruction marks a token account as frozen, disabling all operations on it.
Requires the mint's freeze authority.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `account_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `owner_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |


### ThawAccount

The `ThawAccount` instruction unfreezes a previously frozen token account.
Requires the mint’s freeze authority.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `account_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `owner_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |


### TransferChecked

The `TransferChecked` instruction is similar to `Transfer`, but includes a **decimals check** to ensure the token amount conforms to the mint's precision.
This is useful when validating transfers in offline signing or hardware wallets.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `source_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `destination_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `amount` | `u64` |
| `decimals` | `u8` |


### ApproveChecked

The `ApproveChecked` instruction approves a delegate to transfer up to a specified amount of tokens, **verifying the mint's decimal precision** during approval.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `source_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `delegate_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `owner_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `amount` | `u64` |
| `decimals` | `u8` |


### MintToChecked

The `MintToChecked` instruction mints new tokens to an account, **validating the mint's decimal precision** to ensure safe transfers in hardware wallets or offline tools.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `account_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `owner_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `amount` | `u64` |
| `decimals` | `u8` |

### BurnChecked

The `BurnChecked` instruction removes tokens from a token account,
**checking the mint’s decimal precision** before reducing the total supply.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `account_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `amount` | `u64` |
| `decimals` | `u8` |


### InitializeAccount2

The `InitializeAccount2` instruction creates a new token account,
similar to `InitializeAccount`, but the owner is passed via instruction data instead of the account list.
Useful for CPI (Cross Program Invocation) scenarios.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `account_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `owner_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |


### SyncNative

The `SyncNative` instruction updates the amount field of a wrapped SOL token account
to match the account’s actual lamport balance.

This is useful when native SOL is transferred directly to the token account using system instructions.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `account_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |


### InitializeAccount3

The `InitializeAccount3` instruction initializes a token account just like `InitializeAccount2`, but it does **not require** the Rent sysvar account to be passed in.

Useful for programs that know rent-exemption will already be handled externally.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `account_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `owner_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |


### InitializeMultisig2

The `InitializeMultisig2` instruction sets up a new multisignature account
**without requiring** the Rent sysvar account. It defines how many signers (M) are required
to authorize instructions where the multisig account is used.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `multisig_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `m` | `u8` |


### InitializeMint2

The `InitializeMint2` instruction creates a new mint, just like `InitializeMint`, but does **not require** the Rent sysvar account to be passed.
Used when the account is already rent-exempt.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_authority_pubkey`| [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `freeze_authority_pubkey` | `Option<`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`>` |
| `decimals` | `u8` |


### GetAccountDataSize

The `GetAccountDataSize` instruction calculates the required size for a token account based on the provided mint. It returns the result as a little-endian `u64` using Solana’s return data mechanism.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |

### InitializeImmutableOwner

The `InitializeImmutableOwner` instruction marks a token account's owner as **immutable**, meaning the `SetAuthority` instruction can no longer be used to change ownership.

This must be called **before** `InitializeAccount`, and is mainly used for compatibility with the Associated Token Account program.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `account_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |


### AmountToUiAmount

The `AmountToUiAmount` instruction converts a raw token amount (`u64`) into a human-readable UI string based on the mint’s decimal precision.
The result is returned using Solana’s return data mechanism.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `amount` | `u64` |


### UiAmountToAmount

The `UiAmountToAmount` instruction converts a human-readable token amount (as a string) into the raw `u64` integer format based on the mint’s decimal precision.

The result is returned using Solana’s return data mechanism.

**Parameters:**
| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `ui_amount` | `&str` |


### InitializeMintCloseAuthority

The `InitializeMintCloseAuthority` instruction sets an optional close authority on a mint. 
This must be done **before** the mint is initialized and only works if the mint has the correct size for extensions.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id`| [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `close_authority` | `Option<`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`>` |


### TransferFeeExtension

See [Transfer Fee Config Extension](https://wiki.solanagraph.com/Native_programs/SPL_Token_2022/extensions/transfer_fee_config.md)

### ConfidentialTransferExtension

See [Confidential Transfer Mint Extension](https://wiki.solanagraph.com/Native_programs/SPL_Token_2022/extensions/confidential_transfer_mint.md)

### DefaultAccountStateExtension

### Reallocate

The `Reallocate` instruction increases the size of an existing token account to support additional extensions. 
This is useful when enabling optional features after account creation.


**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `account_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `payer` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `owner_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signer_pubkeys` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `extension_types` | `&[ExtensionType]` |

### MemoTransferExtension

### CreateNativeMint

The `CreateNativeMint` instruction creates the **wrapped SOL mint** (also known as the **native mint**). 
This instruction is permissionless and only needs to be invoked once on-chain after the token-2022 program is deployed.


**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `payer` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |


### InitializeNonTransferableMint

The `InitializeNonTransferableMint` instruction enables the **non-transferable extension** for a given mint account. 
Once initialized, **tokens minted from this mint cannot be transferred** between accounts.


**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |

### InterestBearingMintExtension

### CpiGuardExtension

### InitializePermanentDelegate

The `InitializePermanentDelegate` instruction sets a **permanent delegate** for a mint account. 
This delegate will have ongoing authority to perform actions like `Transfer` or `Burn` on *any* token account associated with the mint, **even without explicit approval**.


**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint_pubkey` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `delegate` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |


### TransferHookExtension

### ConfidentialTransferFeeExtension

### WithdrawExcessLamports

The `WithdrawExcessLamports` instruction is used to **recover SOL** that was accidentally sent to a token account (an account owned by the Token program). It transfers excess lamports to a destination account, leaving behind just enough for rent exemption.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `source_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `destination_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signers` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |

### MetadataPointerExtension

### GroupPointerExtension

### GroupMemberPointerExtension

### ConfidentialMintBurnExtension

### ScaledUiAmountExtension

### PausableExtension