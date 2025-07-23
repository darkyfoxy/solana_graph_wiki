## Transfer Fee Amount Extension

The TransferFeeAmount structure is an account-level extension in the Token-2022 program used to track withheld fees from token transfers.

When a transfer involving a fee-configured mint occurs, part of the amount may be withheld from the sender and stored locally in the account's withheld_amount field instead of being immediately transferred to the mint.

## Structure

**Encoding:** `POD`

**Size:** = 18 bytes

| Field | Type | Description |
| ----- | ---- | ----------- |
| `withheld_amount` | `u64` | The amount of tokens (in base units) withheld during transfers. |