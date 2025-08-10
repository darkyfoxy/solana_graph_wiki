# ElGamalPubkey

`ElGamalPubkey` is a 32-byte public key used for confidential data encryption on-chain. It enables privacy-preserving features such as encrypted balances or encrypted transfer amounts.

Internally, it is represented as a fixed-size byte array and typically displayed or serialized in Base58 format, making it human-readable and compact.

## What is a ElGamalPubkey

Internally, a `ElGamalPubkey` is represented as:

| Field | Type | Description |
|-------|------|-------------|
| `ElGamalPubkey` | `[u8; 32]` | Raw bytes of the ElGamal public key |

This is simply an array of 32 bytes.

## NonZeroElGamalPubkey

In many contexts, a key value of all zeroes is treated as "no key" or an unset value.

NonZeroElGamalPubkey enforces that the value is not equal to [0u8; 32], ensuring the presence of a valid ElGamal public key. This helps distinguish between:

- A valid encryption key (non-zero)
- The absence of a key ([0; 32])
