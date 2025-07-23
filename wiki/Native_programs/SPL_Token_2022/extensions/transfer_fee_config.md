# Transfer Fee Config Extension

The TransferFeeConfig extension enables SPL Token mints to impose transfer fees on token movements. These fees are collected automatically on each transfer and may be withdrawn by a designated authority.

## Structure

**Encoding:** `POD`

**Size:** = 108 bytes

| Name | Type | Description |
| ---- | ---- | ----------- |
| `transfer_fee_config_authority` | [`NonZeroPubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) | Optional authority that may update the fee configuration. |
| `withdraw_withheld_authority` | [`NonZeroPubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) | Optional authority allowed to withdraw withheld fees from mint. |
| `withheld_amount` | `u64` | Accumulated withheld fees collected from transfers. |
| `older_transfer_fee` | [`TransferFee`](#transferfee-structure) | The previous fee configuration, used when the current epoch is before `newer_transfer_fee.epoch`. |
| `newer_transfer_fee` | [`TransferFee`](#transferfee-structure) | The current or upcoming fee configuration, effective when the epoch is reached. |


## TransferFee Structure

**Encoding:** `POD`

**Size:** = 18 bytes

| Name | Type | Description |
| ---- | ---- | ----------- |
| `epoch` | `u64` | First epoch when this fee takes effect. |
| `maximum_fee` | `u64` | Maximum fee (in tokens) that can be charged for a transfer. |
| `transfer_fee_basis_points` | `u16` | Fee rate in basis points (1 basis point = 0.01%). |


## Instructions

| ID | Instruction | Description |
| -- | ----------- | ----------- |
| 0 | `InitializeTransferFeeConfig` | Initializes the transfer fee extension on a new mint. Must be called before `InitializeMint`. |
| 1 | `TransferCheckedWithFee` | Transfers tokens and validates the provided fee. Works even if the mint has no fee configured. |
| 2 | `WithdrawWithheldTokensFromMint` | Withdraws all withheld fees from the mint to a destination account. Requires `withdraw_withheld_authority`. |
| 3 | `WithdrawWithheldTokensFromAccounts` | Withdraws withheld fees from multiple token accounts. |
| 4 | `HarvestWithheldTokensToMint` | Permissionless: collects withheld tokens from token accounts into the mint. |
| 5 | `SetTransferFee` | Updates transfer fee settings. Must be signed by fee authority. |



### InitializeTransferFeeConfig

Initializes the Transfer Fee Config extension for a mint account.
This extension enables the mint to collect a percentage-based fee on token transfers. It sets up both the configuration authority and the authority allowed to withdraw withheld fees.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `transfer_fee_config_authority` | `Option<`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`>` |
| `withdraw_withheld_authority` | `Option<`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`>` |
| `transfer_fee_basis_points` | `u16` |
| `maximum_fee` | `u64` |


### TransferCheckedWithFee

The TransferCheckedWithFee instruction transfers tokens from one account to another while enforcing transfer fee validation.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `source` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `destination` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signers` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `amount` | `u64` |
| `decimals` | `u8` |
| `fee` | `u64` |

### WithdrawWithheldTokensFromMint

The WithdrawWithheldTokensFromMint instruction allows an authorized entity to withdraw all withheld transfer fees that have been accumulated in the mint and send them to a designated destination account.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `destination` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signers` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |

### WithdrawWithheldTokensFromAccounts

The WithdrawWithheldTokensFromAccounts instruction allows the mint’s authorized authority to withdraw withheld transfer fees from multiple token accounts and send them to a single destination account.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `destination` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signers` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `sources` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |

### HarvestWithheldTokensToMint

The HarvestWithheldTokensToMint instruction allows anyone to collect withheld transfer fees from a list of token accounts and move them into the mint's internal withheld amount.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `sources` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |

### SetTransferFee

The SetTransferFee instruction updates the transfer fee configuration on a mint. It sets the new basis point rate and the maximum fee to apply on transfers.


| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `signers` | `&[&`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `transfer_fee_basis_points` | `u16` |
| `maximum_fee` | `u64` |
