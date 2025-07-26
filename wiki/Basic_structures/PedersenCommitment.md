# PedersenCommitment

The `PedersenCommitment` is a packed, plain-old-data (POD) representation of a Pedersen commitment, a fundamental cryptographic primitive used to hide a value while still allowing it to be used in zero-knowledge proofs (e.g. range proofs or sum checks).

`PedersenCommitment = [u8; 32]`