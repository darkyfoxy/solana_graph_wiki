# BatchedRangeProofU64Data

BatchedRangeProofU64Data is a zero-knowledge proof structure used to demonstrate that multiple committed values (Pedersen commitments) are within a valid range — for example, non-negative and less than a certain upper bound. This is critical in confidential token transfers, where values are encrypted but must still comply with system rules.

## CiphertextCommitmentEqualityProofData Structure

**Encoding:** `POD`

**Size:** = bytes

| Field | Type | Description |
| ----- | ---- | ----------- |
| `context` | `BatchedRangeProofContext` | Contains commitment data and associated bit lengths for each value. |
| `proof` | `RangeProofU64` | Serialized zero-knowledge range proof (based on Bulletproofs). |

## BatchedRangeProofContext

| Field | Type | Description |
| ----- | ---- | ----------- |
| `commitments` | `[PedersenCommitment; 8]` | A fixed-size array of Pedersen commitments representing encrypted values. Each commitment corresponds to a value whose range is being proven. |
| `bit_lengths` | `[u8; 8]` | The bit-lengths of the valid range for each commitment. Typically, values are 64 (for `u64` range proofs). |


## RangeProofU64

`RangeProofU64 = [u8; 672]`