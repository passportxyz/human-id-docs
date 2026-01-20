---
description: >-
  Credentials are signed attestations by an issuer. While it supports legacy
  credential types, Human  ID has a standard format for such credentials
  designed for simplicity and efficiency in a ZKP.
---

# Credentials

## Human ID Standard Credential Format <a href="#what-is-a-leaf" id="what-is-a-leaf"></a>

A leaf is an element of a Merkle tree. Merkle trees allow for efficient and anonymous proofs of set membership in zero-knowledge. Leaves are Poseidon hashes of 6 254-bit field elements:

<table><thead><tr><th width="122">Index</th><th width="302">Name</th><th>Description</th></tr></thead><tbody><tr><td>0</td><td>Issuer Address</td><td>The Ethereum address of the credential issuer, i.e. who provided this attestation</td></tr><tr><td>1</td><td>Nullifier Secret</td><td>A secret used to create nullifiers</td></tr><tr><td>2</td><td>Issuer-defined field</td><td>Can be used for anything the issuer wants</td></tr><tr><td>3</td><td>Issuer-defined field</td><td>Can be used for anything the issuer wants</td></tr><tr><td>4</td><td>iat</td><td>Unix timestamp in seconds when the credential was issued at</td></tr><tr><td>5</td><td>Scope</td><td>Default to 0 (wildcard) indicating it can be used anywhere. This field is mostly deprecated and is not used anywhere.</td></tr></tbody></table>

Note that while credentials can only have 2 issuer- fields, nothing prevents these fields from representing more: they can be set to the hash of unlimited custom fields

#### Nullifier Secret

The _nullifier secret_ is a value that only the user knows. Because it is pseudorandom, the preimage of the leaf cannot be guessed without at least brute-forcing the secret, which is infeasible at 16 bytes. Thus, even if the other fields are predictable, the credential will still be anonymous without knowing the secret. In this way, it acts as a pepper, obscuring the preimage from the publicly known digest.

It also has a particular property allowing for sybil resistance: credentials can be spent for a specific use case by publishing a salted hash of the secret pepper. We refer to this as a **hashbrown** because it is a mixture of salt & pepper\_.\_ Accurate computation of the hashbrown can be verified in a ZKP without revealing the pepper. Some use cases of involve anonymous reputation, anonymous voting, bot prevention, and fair airdrops. For example

> Whenever a user votes in an election with actionID `abc123`, they must publish `hash(abc123, secret)` . And they generate a proof that the result came from `abc123` and `secret` . This hash then gets added on-chain, so the user can never vote again without people seeing they're trying to "double-spend" their secret.

## Leaf Formats of Holonym Foundation Issuers

Holonym Foundation provides two issuers, though others are encouraged to create more issuers.

### Government ID Issuer

<table><thead><tr><th width="122">Index</th><th width="302">Name</th><th>Description</th></tr></thead><tbody><tr><td>0</td><td>Issuer Address</td><td>The Ethereum address of the credential issuer, i.e. who provided this attestation</td></tr><tr><td>1</td><td>Nullifier Secret</td><td>A secret used to create nullifiers</td></tr><tr><td>2</td><td>Country</td><td>This is a number corresponding to the user's country. Countries are numbered based on a custom accumulator scheme. This allows them to be used in efficient ZK arguments of presence in allowlists of countries.</td></tr><tr><td>3</td><td>Additional Info</td><td>Poseidon hash of: Name, city, state/province, street, zipcode</td></tr><tr><td>4</td><td>Time of credential issuance</td><td>Represented as seconds since 1900 (unix timestamp with -70yr offset)</td></tr><tr><td>5</td><td>Scope</td><td>0</td></tr></tbody></table>

### Phone Issuer

<table><thead><tr><th width="122">Index</th><th width="302">Name</th><th>Description</th></tr></thead><tbody><tr><td>0</td><td>Issuer Address</td><td>The Ethereum address of the credential issuer, i.e. who provided this attestation</td></tr><tr><td>1</td><td>Nullifier Secret</td><td>A secret used to create nullifiers</td></tr><tr><td>2</td><td>Phone Number</td><td>Phone number in E.164 but without the leading + so it can be converted to a field element. E.g., +15555555555 becomes 15555555555</td></tr><tr><td>3</td><td>Empty</td><td>0</td></tr><tr><td>4</td><td>iat</td><td>Day the credential was issued at, in days since 1900/01/01</td></tr><tr><td>5</td><td>Scope</td><td>0</td></tr></tbody></table>

### Biometrics Issuer

<table><thead><tr><th width="122">Index</th><th width="302">Name</th><th>Description</th></tr></thead><tbody><tr><td>0</td><td>Issuer Address</td><td>The Ethereum address of the credential issuer, i.e. who provided this attestation</td></tr><tr><td>1</td><td>Nullifier Secret</td><td>A secret used to create nullifiers</td></tr><tr><td>2</td><td>Group Name</td><td>1 (this maybe incremented)</td></tr><tr><td>3</td><td>Reference Hash</td><td>Hash of the reference id</td></tr><tr><td>4</td><td>iat</td><td>Day the credential was issued at, in days since 1900/01/01</td></tr><tr><td>5</td><td>Scope</td><td>0</td></tr></tbody></table>
