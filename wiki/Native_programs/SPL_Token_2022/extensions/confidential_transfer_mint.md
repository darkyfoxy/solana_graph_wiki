# Confidential Transfer Mint Extension

The `ConfidentialTransferMint` structure defines the configuration for confidential transfers on a given token mint. It enables encrypted transfers and controls whether accounts must be approved before use, as well as designating an optional auditor key to decrypt transfer amounts.

This structure is stored as part of the mint’s extensions and is required to enable confidential transfer functionality.

## Structure

**Encoding:** `POD`

**Size:** = 65 bytes

| Field | Type | Description |
| ----- | ---- | ----------- |
| `authority` | [`NonZeroPubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) | Optional authority allowed to configure the mint and approve accounts. Must not be a multisig. |
| `auto_approve_new_accounts` | `bool` | If `true`, no approval is required and new accounts may be used immediately. If `false`, the authority must approve newly configured accounts. |
| `auditor_elgamal_pubkey` | [`NonZeroElGamalPubkey`](https://wiki.solanagraph.com/Basic_structures/ElGamal_Pubkey.md) | Optional ElGamal public key for an auditor that can decrypt transfer amounts. |

## Instructions

| ID | Instruction | Description |
| -- | ----------- | ----------- |
| 0 | `InitializeMint` | Initializes confidential transfer support for a mint. Must be in same transaction as `InitializeMint` to prevent front-running. |
| 1 | `UpdateMint` | Updates configuration for a confidential transfer mint. Requires authority signature. |
| 2 | `ConfigureAccount` | Configures a token account for confidential transfers. Requires ElGamal key verification. |
| 3 | `ApproveAccount` | Approves a confidential account when auto-approval is disabled on the mint. |
| 4 | `EmptyAccount` | Empties the available balance of a confidential account. Used before closing. |
| 5 | `Deposit` | Deposits SPL tokens into a confidential account's pending balance. |
| 6 | `Withdraw` | Withdraws tokens from a confidential account's available balance. Requires zero-knowledge proofs. |
| 7 | `Transfer` | Confidentially transfers tokens between accounts. Requires ZK proofs. |
| 8 | `ApplyPendingBalance` | Applies pending deposits/transfers to a confidential account's available balance. |
| 9 | `EnableConfidentialCredits` | Allows an account to receive confidential transfers. |
| 10 | `DisableConfidentialCredits` | Disallows receiving confidential transfers. May still allow non-confidential credits. |
| 11 | `EnableNonConfidentialCredits` | Allows an account to receive non-confidential transfers. |
| 12 | `DisableNonConfidentialCredits` | Blocks non-confidential transfers; account only accepts confidential tokens. |
| 13 | `TransferWithFee` | Confidential transfer including fee calculation. Requires ZK proofs. |
| 14 | `ConfigureAccountWithRegistry` | Like `ConfigureAccount`, but uses an `ElGamalRegistry` account instead of proof verification. |


### InitializeMint

The `InitializeMint` instruction sets up confidential transfer capabilities for a newly created token mint.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority` | `Option<`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`>` |
| `auto_approve_new_accounts` | `bool` |
| `auditor_elgamal_pubkey` | `Option<`[`ElGamalPubkey`](https://wiki.solanagraph.com/Basic_structures/ElGamal_Pubkey.md)`>` |

### UpdateMint

The `UpdateMint` instruction modifies the confidential transfer configuration of an existing mint.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `multisig_signers` | `[`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `auto_approve_new_accounts` | `bool` |
| `auditor_elgamal_pubkey` | `Option<`[`ElGamalPubkey`](https://wiki.solanagraph.com/Basic_structures/ElGamal_Pubkey.md)`>` |

### ConfigureAccount

The ConfigureAccount instruction enables confidential transfers for a specific token account.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `token_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `decryptable_zero_balance` | [`DecryptableBalance`](https://wiki.solanagraph.com/Native_programs/SPL_Token_2022/extensions/confidential_transfer_account.md#decryptablebalance)  |
| `maximum_pending_balance_credit_counter` | `u64` – A limit to how many pending balance updates can be applied |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `multisig_signers` | `[`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `proof_data_location` | [`PubkeyValidityProofData`](https://wiki.solanagraph.com/Basic_structures/PubkeyValidityProofData.md)  |

### ApproveAccount

The ApproveAccount instruction authorizes a previously configured token account to use confidential transfers.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `account_to_approve` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `multisig_signers` | `[`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |

### EmptyAccount

The EmptyAccount instruction zeroes out the available confidential balance of a token account.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `token_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `multisig_signers` | `[`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `proof_data_location` | [`ZeroCiphertextProofData`](https://wiki.solanagraph.com/Basic_structures/ZeroCiphertextProofData.md) |

### Deposit

The Deposit instruction moves public (non-confidential) SPL tokens into a confidential token account's pending balance.

This allows a user to convert regular tokens into confidential tokens.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `token_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `amount` | `u64` |
| `decimals` | `u8` |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `multisig_signers` | `[`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |


### Withdraw

The Withdraw instruction allows a user to redeem confidential tokens from their available balance into standard (non-confidential) SPL tokens. This moves tokens out of the confidential system.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `token_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `amount` | `u64` |
| `decimals` | `u8` |
| `new_decryptable_available_balance` | [`DecryptableBalance`](https://wiki.solanagraph.com/Native_programs/SPL_Token_2022/extensions/confidential_transfer_account.md#decryptablebalance) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `multisig_signers` | `[`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `equality_proof_data_location` | [`CiphertextCommitmentEqualityProofData`](https://wiki.solanagraph.com/Basic_structures/CiphertextCommitmentEqualityProofData.md) |
| `range_proof_data_location` | `BatchedRangeProofU64Data` — Proof that the withdrawal amount fits within allowed bounds (e.g. is non-negative) |

### Transfer

The transfer instruction enables a confidential transfer of tokens between two SPL Token accounts. Unlike inner_transfer, which is used in CPI contexts, this instruction is invoked directly by transactions.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `source_token_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `destination_token_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `new_source_decryptable_available_balance` | [`DecryptableBalance`](https://wiki.solanagraph.com/Native_programs/SPL_Token_2022/extensions/confidential_transfer_account.md#decryptablebalance) |
| `transfer_amount_auditor_ciphertext_lo` | [`ElGamalCiphertext`](https://wiki.solanagraph.com/Basic_structures/ElGamal_Ciphertext.md) |
| `transfer_amount_auditor_ciphertext_hi` | [`ElGamalCiphertext`](https://wiki.solanagraph.com/Basic_structures/ElGamal_Ciphertext.md) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `multisig_signers` | `[`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `equality_proof_data_location` | [`CiphertextCommitmentEqualityProofData`](https://wiki.solanagraph.com/Basic_structures/CiphertextCommitmentEqualityProofData.md) |
| `ciphertext_validity_proof_data_location` | `BatchedGroupedCiphertext3HandlesValidityProofData` — Proves ciphertext structure is valid |
| `range_proof_data_location` | `BatchedRangeProofU128Data` — Ensures transferred amount is in a valid range |


### ApplyPendingBalance

The `ApplyPendingBalance` instruction finalizes any pending confidential deposits or transfers by rolling them into the available balance of a token account.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `token_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `pending_balance_instructions` | `u64` — The number of pending deposit/transfer instructions being applied |
| `new_decryptable_available_balance` | [`DecryptableBalance`](https://wiki.solanagraph.com/Native_programs/SPL_Token_2022/extensions/confidential_transfer_account.md#decryptablebalance) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `multisig_signers` | `[`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |

### EnableConfidentialCredits

The `EnableConfidentialCredits` instruction enables a token account to receive confidential (encrypted) transfers. It is used after the account has been configured for confidential transfers and allows the account to accept incoming encrypted tokens.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `token_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `multisig_signers` | `[`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |

### DisableConfidentialCredits

The `DisableConfidentialCredits` instruction disables a token account from receiving confidential (encrypted) transfers. It is used to opt out of receiving encrypted token deposits into an account that has been configured for confidential transfers.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `token_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `multisig_signers` | `[`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |

### EnableNonConfidentialCredits

Enables a token account, configured with the Confidential Transfer extension, to accept non-confidential (transparent) token transfers in addition to confidential ones.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `token_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `multisig_signers` | `[`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |

### DisableNonConfidentialCredits

Disables non-confidential (transparent) transfers to a confidential token account, enforcing that only confidential transfers are allowed.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `token_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `multisig_signers` | `[`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |

### TransferWithFee

Performs a confidential token transfer between two accounts while applying a confidential fee.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `source_token_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `destination_token_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `new_source_decryptable_available_balance` | [`DecryptableBalance`](https://wiki.solanagraph.com/Native_programs/SPL_Token_2022/extensions/confidential_transfer_account.md#decryptablebalance) |
| `transfer_amount_auditor_ciphertext_lo` | [`ElGamalCiphertext`](https://wiki.solanagraph.com/Basic_structures/ElGamal_Ciphertext.md) |
| `transfer_amount_auditor_ciphertext_hi` | [`ElGamalCiphertext`](https://wiki.solanagraph.com/Basic_structures/ElGamal_Ciphertext.md) |
| `authority` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `multisig_signers` | `[`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`]` |
| `equality_proof_data_location` | [`CiphertextCommitmentEqualityProofData`](https://wiki.solanagraph.com/Basic_structures/CiphertextCommitmentEqualityProofData.md) |
| `transfer_amount_ciphertext_validity_proof_data_location` | `BatchedGroupedCiphertext3HandlesValidityProofData` |
| `fee_sigma_proof_data_location` | `PercentageWithCapProofData` |
| `fee_ciphertext_validity_proof_data_location` | `BatchedGroupedCiphertext2HandlesValidityProofData` |
| `range_proof_data_location` | `BatchedRangeProofU256Data` |

### ConfigureAccountWithRegistry

Initializes confidential transfer settings for a token account using a pre-validated ElGamal registry account instead of an in-transaction ZKP proof.

**Parameters:**

| Name | Type |
| ---- | ---- |
| `token_program_id` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `token_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `mint` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `elgamal_registry_account` | [`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md) |
| `payer` | `Option<`[`Pubkey`](https://wiki.solanagraph.com/Basic_structures/Public_key.md)`>` |

