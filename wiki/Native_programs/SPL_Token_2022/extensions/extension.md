# Token-2022 Extensions

The Token-2022 Program introduces a powerful extension framework that allows mint and token accounts to be extended with optional, modular functionality — without changing the core account layout.

## TLV Format

Each extension is stored in the account data as a TLV entry:

| Name | Type | Description |
| ---- | ---- | ----------- |
| `type` | `u16` | Extension type, identified by `ExtensionType`. |
| `length` | `u16` | Length of the extension data in bytes. |
| `value` | `[...]` | Extension-specific data structure. |


## ExtensionType

This enum defines all supported extensions and maps directly to the type field in TLV entries.
Mint extensions can only be used on mint accounts; account extensions only on token holding accounts.

| Value | Variant | Description |
| ----- | ------- | ----------- |
| 0 | `Uninitialized` | Placeholder or padding entry. |
| 1 | `TransferFeeConfig` | Transfer fee rate settings and authorities. |
| 2 | `TransferFeeAmount` | Withheld fees on an account. |
| 3 | `MintCloseAuthority` | Optional authority to allow mint closure. |
| 4 | `ConfidentialTransferMint` | Auditor settings for confidential transfers. |
| 5 | `ConfidentialTransferAccount` | Confidential transfer state and balances. |
| 6 | `DefaultAccountState` | Default state for new token accounts. |
| 7 | `ImmutableOwner` | Prevents changing the account's owner. |
| 8 | `MemoTransfer` | Requires memo in inbound transfers. |
| 9 | `NonTransferable` | Marks mint as non-transferable. |
| 10 | `InterestBearingConfig` | Enables interest accrual on tokens. |
| 11 | `CpiGuard` | Blocks privileged token operations over CPI. |
| 12 | `PermanentDelegate` | Designates a permanent delegate with authority. |
| 13 | `NonTransferableAccount` | Marks account as belonging to a non-transferable mint. |
| 14 | `TransferHook` | Requires CPI to a hook program before transfer. |
| 15 | `TransferHookAccount` | Indicates account is under transfer hook rules. |
| 16 | `ConfidentialTransferFeeConfig` | Encrypted fee config and public key. |
| 17 | `ConfidentialTransferFeeAmount` | Encrypted withheld fee data. |
| 18 | `MetadataPointer` | Pointer to external metadata account. |
| 19 | `TokenMetadata` | Token metadata embedded in the mint. |
| 20 | `GroupPointer` | Pointer to token group config. |
| 21 | `TokenGroup` | Token group configuration. |
| 22 | `GroupMemberPointer` | Pointer to group member configuration. |
| 23 | `TokenGroupMember` | Group member configuration. |
| 24 | `ConfidentialMintBurn` | Enables confidential minting and burning. |
| 25 | `ScaledUiAmount` | UI amount scaled by a fixed ratio. |
| 26 | `Pausable` | Allows pausing minting, burning, and transfers. |
| 27 | `PausableAccount` | Indicates the account is pausable. |
