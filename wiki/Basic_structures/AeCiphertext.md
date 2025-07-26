# AeCiphertext

The `AeCiphertext` is a packed, deterministic authenticated encryption (AE) format used to securely encode a token amount. It provides both confidentiality and integrity using a symmetric encryption scheme.

## AeCiphertext Structure

Internally, a `AeCiphertext` is represented as:

| Field | Type | Description |
|-------|------|-------------|
| `AeCiphertext` | `[u8; 36]` | Raw bytes of the AeCiphertext |