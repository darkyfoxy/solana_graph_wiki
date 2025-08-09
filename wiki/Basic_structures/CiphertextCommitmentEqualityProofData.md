# CiphertextCommitmentEqualityProofData

The `CiphertextCommitmentEqualityProofData` structure provides both the cryptographic proof and the necessary contextual data to verify that an ElGamal-encrypted ciphertext corresponds to a given Pedersen commitment. This is used to ensure consistency between encrypted values and committed values in zero-knowledge confidential transfer workflows.

## CiphertextCommitmentEqualityProofData Structure

**Encoding:** `POD`

**Size:** =  bytes

| Field | Type | Description |
| ----- | ---- | ----------- |
| `context` | `CiphertextCommitmentEqualityProofContext` | Includes all the cryptographic inputs (ciphertext, commitment, pubkey). |
| `proof` | `CiphertextCommitmentEqualityProof` | The zero-knowledge proof that both represent the same value. |


## CiphertextCommitmentEqualityProofContext

| Field | Type | Description |
| ----- | ---- | ----------- |
| `pubkey` | `ElGamalPubkey` | The ElGamal public key used to encrypt the value. |
| `ciphertext` | `ElGamalCiphertext` | The ElGamal-encrypted value. |
| `commitment` | `PedersenCommitment` | A Pedersen commitment to the same value. |


## CiphertextCommitmentEqualityProof

`CiphertextCommitmentEqualityProof = [u8; 192]`
