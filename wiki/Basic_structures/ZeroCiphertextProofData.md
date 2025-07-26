# ZeroCiphertextProofData

The `ZeroCiphertextProofData` structure is used to prove in zero knowledge that an encrypted value (ciphertext) represents the value zero — without revealing any underlying information. This is especially useful in confidential token systems where encrypted balances must be verified for emptiness before allowing operations like account closure.

## Structure

**Encoding:** `POD`

**Size:** = 192 bytes

| Field | Type | Description |
| ----- | ---- | ----------- |
| `context` | [`ZeroCiphertextProofContext`](#zerociphertextproofcontext) | Contains the ElGamal ciphertext that is claimed to be zero. |
| `proof` | [`ZeroCiphertextProof`](#zerociphertextproof) | A zero-knowledge proof that the ciphertext indeed encrypts the value zero. |

## ZeroCiphertextProofContext

The ZeroCiphertextProofContext structure provides the contextual data required to verify a zero-ciphertext proof in a confidential transfer system.

**Encoding:** `POD`

**Size:** = 96 bytes

| Field | Type | Description |
| ----- | ---- | ----------- |
| `pubkey` | [`ElGamalPubkey`](https://wiki.solanagraph.com/Basic_structures/ElGamal_Pubkey.md) | The ElGamal public key used to encrypt the ciphertext. |
| `ciphertext` | [`ElGamalCiphertext`](https://wiki.solanagraph.com/Basic_structures/ElGamal_Ciphertext.md) | The ElGamal ciphertext being proved to encrypt the value zero. |


## ZeroCiphertextProof

`ZeroCiphertextProof = [u8; 96]`