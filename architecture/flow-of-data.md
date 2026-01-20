# Flow of Data

## Architecture

The Human ID protocol consists of the following components:

* Client (website or mobile app)
* Issuers (either using the Human ID credential format or custom formats such as ICAO9303 for NFC passports issued by the government)
* Hub smart contracts (1 per chain)
* Relayer

The flow of data is outlined in the action sequence described in the [#actions](overview.md#actions "mention") section. The following describes the flow of data in more detail. The diagram illustrates the _issuance+storage_ (designated by 1.\*) and _proving_ (designated by 2.\*) steps. An organization can grant access to users based on the results stored in the proof contract.

<figure><img src="../.gitbook/assets/Minting and Proving Diagram(2).png" alt=""><figcaption><p>Previous V2 architecture. In V3 storage is done in the Human Wallet, there is no Roots smart contract, and the Proof smart contract is called the Hub.</p></figcaption></figure>

1.1. User verifies themself to an issuer

1.2. The issuer signs credentials

1.3. The user generates a proof that credentials were sigend with particular attributes

1.5. User encrypts their credentials client-side. The encryption key is generated from the user's signature.

1.6. User stores their encrypted credentials in the Human Wallet

2.1. User generates a proof and (e.g., a proof that they are a US resident) and submits it to the verifier

2.2. The verifier attests to it and returns it to either the user or the relayer, depending on how the attestation is to be consumed

2.3. The attestation is given to the recipient: either the Hub smart contract or an offchain consumer
