# Solana account

# Solana Accounts

In Solana, **accounts** are the fundamental unit of storage. Everything — tokens, smart contracts, metadata, even executable programs — lives in accounts.

---

## Where Accounts Are Stored

At runtime, Solana stores account data in memory, but persistent storage happens in a structure called the **AccountDB**. This database is responsible for saving all accounts to disk.

The layout and format of account files depends on the specific Solana validator implementation.  
According to the [Agave source code](https://github.com/anza-xyz/agave/blob/528d984671c18e870ee94461a400751c8feddfbd/accounts-db/src/accounts_file.rs#L70), account data can be stored using either:

- `AppendVec`
- `TieredStorage`

---

## Typical Account Structure

When developers or smart contracts interact with accounts, they usually see something similar to the following structure:

| Name         | Type           | Description                                                     |
| ------------ | -------------- | --------------------------------------------------------------- |
| `lamports`   | `u64`          | Number of lamports held by the account                          |
| `data`       | `Vec<u8>`      | Arbitrary binary data stored in the account                     |
| `owner`      | `Pubkey`       | Public key of the program that owns this account                |
| `executable` | `bool`         | If `true`, the account contains an executable program           |
| `rent_epoch` | `u64`          | The next epoch when this account must pay rent                  |

This is the logical structure returned by Solana RPC nodes ([Solana RPC Methods & Documentation](https://solana.com/docs/rpc/http/getaccountinfo)).

## Account Types

The field executable: bool plays a key role in defining what the account actually represents.

- If executable == false — this is a data account, with data struture
- If executable == true — this is a program accoun, with compiled bytecode

## Storage on Disk

Under the hood, account data stored on disk — especially inside AppendVec files — follows a more low-level structure.

Below is an approximate layout of how an account may be stored inside a file. This does not represent the exact memory layout, but it helps illustrate the components involved ([Agave source code](https://github.com/anza-xyz/agave/blob/master/accounts-db/src/append_vec/meta.rs)):

| Name                          | Type     | Description                                    |
| ----------------------------- | -------- | ---------------------------------------------- |
| `write_version_obsolete`      | `u64`    | Legacy field, no longer actively used          |
| `data_len`                    | `u64`    | Length of the account's data segment           |
| `pubkey`                      | `Pubkey` | Public key of the account                      |
| `lamports`                    | `u64`    | Lamports held in this account                  |
| `rent_epoch`                  | `u64`    | Next rent epoch                                |
| `owner`                       | `Pubkey` | Owning program                                 |
| `executable`                  | `bool`   | If `true`, this is a loaded executable account |
| `data`                        | `[u8]`   | Binary data payload                            |

> ⚠️ Note: This structure is for illustrative purposes only. It does not reflect exact memory layout and may vary between Solana client versions and storage types.
