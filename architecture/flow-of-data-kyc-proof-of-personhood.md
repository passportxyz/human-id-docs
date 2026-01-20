# Flow of Data: KYC Proof of Personhood

Human ID's Proof of Personhood via KYC consists of the following components:

* User agent (UI)
* Human ID server
* ID verification provider
* Verifier

The flow of data is outlined in the following sequence diagram. Please refer to notes for detailed explanations for relevant parts.

```mermaid
sequenceDiagram
    participant U as User / User agent
    participant Z as Human  ID server
    participant IDV as IDV Provider
    participant V as Verifier

    Note over U, Z: 1. Initiate ID verification
    Note left of U: User visits /gov-id
    U->>Z: Initiate ID verification process
    Z->>Z: Create session
    Z->>U: Request payment
    U->>Z: Make payment
    Z->>Z: Verify payment and save TX hash
    Note over U, IDV: 2. Complete ID verification
    Z->>IDV: Request IDV session
    Note right of IDV: IDV Providers: Veriff, Onfido, Facetec
    IDV->>Z: Return IDV session
    U->>U: Render IDV session on UI
    U->>IDV: User submits selfie and document to complete verification
    Note right of IDV: Refer to notes on handling of user data by IDV Providers
    IDV->>Z: Return IDV session result
    Z->>U: Return signed IDV result
    U->>U: IDV session result is encrypted on client-isde
    Note right of U: Refer to notes on client-side encryption
    U->>Z: Ciphertext stored
    Note right of Z: Refer to notes on ciphertext storage
    Note over U, V: 3. Proof of ID uniqueness
    U->>U: Generate ZKP for uniqueness
    U->>V: Send ZKP to verifier
    V->>V: Verifier verifies the ZKP
    V->>V: Issue SBT with embedded metadata
    Note right of V: Refer to notes on SBT issuance
    V->>U: Soul-bound token is sent to the specified address on the specified chain
```

#### Issuance and Proving

Sections 1 and 2 in the sequence diagram constitute _issuance_. This is where the user's private credentials are issued.

Section 3 is proving, where the user proves facts about their issued credentials.

#### Notes on handling of user data by IDV Providers

Following data are requested by IDV providers as photo or/and video stream during the verification process.

* Selfie (photo, video stream)
* One of the following documents
  * Passport
  * Driver License
  * Identity Card

Currently, following IDV providers are supported.

* [Veriff](https://trust.veriff.com/)
* [Onfido](https://onfido.com/privacy/)
* [Facetec](https://dev.facetec.com/privacy-site)

**Veriff** has clearly outlined in its [trust center](https://trust.veriff.com/)

* a list of compliances (i.e: GDPR)
* regarding data collection, retention and deletion [controls](https://trust.veriff.com/controls#data-and-privacy)
  * a list of [subprocessors](https://trust.veriff.com/controls#documentation)

**Onfido** has its [privacy policy](https://onfido.com/privacy/)

* a list of [compliances](https://onfido.com/company/certifications/)
* regarding data
  * [collection](https://onfido.com/privacy/#toc-2-the-information-we-collect-and-how-we-use-it-on-behalf-of-our-clients-3)
  * [processing](https://onfido.com/privacy/#toc-3-using-information-as-controller-4)
  * [security](https://onfido.com/privacy/#toc-5-information-security-6)
  * [storage](https://onfido.com/privacy/#toc-6-data-storage-7)
* ControlCase has issued compliance certificate for ISO 27001

**Facetec** has two privacy policies ([site](https://dev.facetec.com/privacy-site) and [sdk](https://dev.facetec.com/privacy-sdk))

> _SDK privacy policy seems more relevant for usage for ID verification. Its documentation on privacy is sparse compared to the other 2 providers._

* In article #2, it mentions that any data sent to its server is encrypted, siloed and is never stored with any additional personally identifiable information (PII).
* In article #6, it provides detailed info on its compliance to GDPR for EU residents.

#### Notes on client-side encryption of IDV session result

IDV provider returns the session result to user.

With Human Wallet:

The result is encrypted on client-side using a derivative of the PRF.

With other wallets:

The result is encrypted with key derived with `hash(userSignature(aConstantMessage))` to generate ciphertext.

#### Notes on ciphertext and storage of userCredentials

Only the encrypted ciphertext which is non PII is stored in Human ID database as below.

```json
// userCredentialsv2
{
  "_id": {
    "$oid": "676d..."
  },
  "holoUserId": "f111...",
  "encryptedGovIdCreds": {
    "ciphertext": "0x...",
    "iv": "0x...",
    "_id": {
      "$oid": "676d..."
    }
  },
  "__v": {
    "$numberInt": "0"
  }
}

```

View [#government-id-issuer](../how-it-works/credentials.md#government-id-issuer "mention")to see the data included in user credentials.

#### Notes on verifier and SBT issuance

The user submits a zero knowledge proof of uniqueness ([see the circuit here](https://github.com/holonym-foundation/id-hub-contracts/blob/main/zk/circuits/circom/V3SybilResistance.circom)) to the verifier server. The verifier verifies the ZKP, and upon verification, issues a soulbound token to the user. The circuit ID, issuer address, expiry, and actionNullifier, the ZK proof are embedded in the Soul-bound token.
