# Where Data Is(n't) Stored

## Credential storage

Credentials are stored by the Human Wallet self-custodial wallet so that other than the user who owns them, nobody can access them.

When the user scans government IDs, the data is not stored in Holonym servers. For modern NFC-based ID and Aadhaar verification, a ZKP is created without Holonym ever seeing the data. For older types of government ID verification that do not involve ePassport NFC reading, the user data briefly passes through Holonym servers so that Holonym can sign the credentials, attesting to their authenticity, to return to the user. Data is immediately deleted from Holonym servers and from the KYC provider's servers after the signature is generated. Even if the user does legacy credential verification and does not trust a KYC provider, the protocol gives additional privacy guaranteed: Each user's ID is anonymized in what we call the **Privacy Pool**, i.e. an anonymity set. The Privacy Pool exists to break the link between a user's identity and a user's crypto address. This creates a level of privacy where even if Holonym, an issuer, or anyone tries to surreptitiously collect user data, they still cannot identify user wallets.

## Proof storage

Proofs can be submitted on-chain or off-chain. Proofs, such as "I am a unique person" should not contain sensitive information. The Holonym website and app only permit proofs that do not give sensitive data, regardless of whether they're on- or off-chain.
