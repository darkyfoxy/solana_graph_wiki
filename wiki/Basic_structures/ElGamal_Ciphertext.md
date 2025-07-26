# ElGamalCiphertext

The `ElGamalCiphertext` is a structure that represents an ElGamal-encrypted value, commonly used in confidential token operations and zero-knowledge proof systems.

## ElGamalCiphertext Structure

Internally, a `ElGamalPubkey` is represented as:

| Field | Type | Description |
|-------|------|-------------|
| `ElGamalCiphertext` | `[u8; 64]` | Raw bytes of the ElGamalCiphertext |
