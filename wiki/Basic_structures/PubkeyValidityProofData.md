# PubkeyValidityProofData

The `PubkeyValidityProofData` structure is used in zero-knowledge (ZK) systems to prove the validity of an ElGamal public key without revealing the private key. This is a fundamental step in establishing trust in cryptographic systems involving confidential transfers, such as ElGamal-based encryption used in confidential SPL token accounts.

## PubkeyValidityProofData Structure

**Encoding:** `POD`

**Size:** = 96 bytes

| Field | Type | Description |
| ----- | ---- | ----------- |
| `context` | [`PubkeyValidityProofContext`](#pubkeyvalidityproofcontext) | Contains the ElGamal public key being validated. |
| `proof` | `PubkeyValidityProof` | A cryptographic proof that the public key is well-formed. |


## PubkeyValidityProofContext

This struct holds the ElGamal public key that is being validated.

`PubkeyValidityProofContext = `[`ElGamalPubkey`](https://wiki.solanagraph.com/Basic_structures/ElGamal_Pubkey.md)

## PubkeyValidityProof

`PubkeyValidityProof = [u8; 64]`