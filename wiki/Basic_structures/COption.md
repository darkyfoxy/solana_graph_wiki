# COption

`COption<T>` is Solana's compact and serialization-friendly version of Rust's `Option<T>`.
It is used to represent values that may or may not be present in account data — such as optional authorities, mint addresses, delegates, etc.

---

## 📦 Structure

Here is the definition of `COption`:
| Name | Type | Description |
| ---- | ---- | ----------- |
| `is_some` | `u32` | A flag indicating whether `value` is present: `0 = None`, `1 = Some` |
| `value` | `T` | The actual value (only meaningful if `is_some == 1`) |

⚠️ Even if is_some == 0, the value field still occupies memory in the account layout.
It is ignored during deserialization but must be included when calculating byte size or packing data.

---

## Why is is_some a u32?

Although a boolean (bool) would require only 1 byte, is_some is defined as a u32 (4 bytes) for alignment and compatibility reasons