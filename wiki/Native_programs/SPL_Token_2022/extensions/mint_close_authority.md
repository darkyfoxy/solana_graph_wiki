# Mint Close Authority Extension

The MintCloseAuthority structure is a mint-level extension in the Token-2022 program that defines an optional authority which can close the mint account and reclaim its rent.

This extension must be initialized before it can be used. If close_authority is not set (i.e. all bytes are zero), then the mint has no close authority and cannot be closed.

## Structure

**Encoding:** `POD`

**Size:** = 32 bytes

| Field | Type | Description |
| ----- | ---- | ----------- |
| `close_authority` | `NonZeroPubkey` | The optional authority allowed to close the mint. |
