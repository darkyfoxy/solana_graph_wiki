# Confidential Transfer Account Extension

The `ConfidentialTransferAccount` extension adds support for confidential balance tracking and privacy-preserving transfers to a token account. It encrypts all sensitive balance information and allows owners to control whether the account accepts confidential or non-confidential transfers.

## Structure

**Encoding:** `POD`

**Size:** = 288 bytes

| Field | Type | Description |
| ----- | ---- | ----------- |
| `approved` | `bool` | Indicates whether the account is approved for use. All confidential operations fail if not approved. |
| `elgamal_pubkey` | [`PodElGamalPubkey`](https://wiki.solanagraph.com/Basic_structures/ElGamal_Pubkey.md) | The public key used to encrypt the account’s balances. |
| `pending_balance_lo` | [`EncryptedBalance`](#encryptedbalance) | Encrypted low 16 bits of the pending balance. |
| `pending_balance_hi` | [`EncryptedBalance`](#encryptedbalance) | Encrypted high 32 bits of the pending balance. |
| `available_balance` | [`EncryptedBalance`](#encryptedbalance) | Encrypted available balance (used for withdrawals and transfers). |
| `decryptable_available_balance` | [`DecryptableBalance`](#decryptablebalance) | A version of the available balance decryptable by the owner. |
| `allow_confidential_credits` | `bool` | If `false`, confidential transfers to this account will be rejected. |
| `allow_non_confidential_credits` | `bool` | If `false`, non-confidential (plain) transfers will be rejected. |
| `pending_balance_credit_counter` | `u64` | Number of `Deposit` or `Transfer` instructions that added to the pending balance. |
| `maximum_pending_balance_credit_counter` | `u64` | Maximum allowed value for the `pending_balance_credit_counter` before applying it to available balance. |
| `expected_pending_balance_credit_counter` | `u64` | Snapshot of `pending_balance_credit_counter` expected during the last `ApplyPendingBalance` instruction. |
| `actual_pending_balance_credit_counter` | `u64` | The true counter value when `ApplyPendingBalance` was last called. |

### EncryptedBalance

The EncryptedBalance represents a token amount that is encrypted using ElGamal public-key encryption. This encryption scheme enables confidential transfers in SPL Token extensions by hiding token balances from public view while preserving the ability to verify and compute on them using zero-knowledge proofs.

> `EncryptedBalance = `[`ElGamalCiphertext`](https://wiki.solanagraph.com/Basic_structures/ElGamal_Ciphertext.md)

### DecryptableBalance

The `DecryptableBalance` represents an account balance encrypted using authenticated encryption (AE). It allows the recipient to decrypt the balance using a symmetric key while providing confidentiality and integrity guarantees.

> `DecryptableBalance = `[`AeCiphertext`](https://wiki.solanagraph.com/Basic_structures/AeCiphertext.md)