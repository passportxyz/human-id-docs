# Run an Observer

If you want to run an observer, please reach out to the Human ID team.

## Overview

The observer is a component of the Clean Hands Stack for programmable privacy, used for GDPR-compliant storage of encrypted user data with the DecryptBabyJubJub method for KYC.

The observer is a primary component in the Clean Hands stack with [**Human ID**](https://docs.holonym.id). To interact with the observer, a user generates a ZKP they have passed sanctions checks, and this ZKP outputs the ciphertext of the user's personal identifiable information (PII) and the user's associated blockchain address. The Observer's role in this system is to verify ZKPs, issue attestations to users with valid ZKPs, and to store the public outputs of these ZKPs so that the ciphertext can be decrypted if Human Network permits.

## Endpoints

### POST /observations

This endpoint does the following.

* Verify the Clean Hands ZKP. Uses [this circuit](https://github.com/holonym-foundation/id-hub-contracts/blob/main/zk/circuits/circom/V3CleanHands.circom) to verify a proof which should have been generated using [this package](https://www.npmjs.com/package/wasm-vole-zk-adapter).
* Make sure the encryption key output by the circuit is Human Network’s public key.
* Make sure the issuer address output by the circuit is the configured clean hands issuer.
* Make sure the conditions contract signed by the user is on our whitelist.
* Verify the user’s signature of the conditions contract.
* Store the ZKP’s public values, user's address, user's signature, and signed access contract in the `observations` collection.
* Issue an attestation on Sign Protocol.

### POST /observations/sui

Identical to `POST /observations` but mints an SBT on Sui instead of issuing an attestation on Sign Protocol.

### GET /observations?user\_address=\<address>

This endpoint queries the database for an observation for the provided user address and returns the result.

## Schemas

```rust
pub struct ObservationSchema {
    /// Distinct from _id. This is a hash of the fields of the observation. Allows for 
    /// more efficient lookups to make sure we don't store the same observation twice.
    pub id: String,
    pub user_address: String,
    pub signature: String,
    pub access_contract: String,
    pub zkp_public_values: Vec<String>,
}
```

## Environment variables

Create a .env file with the following variables. All are necessary.

```bash
# You might want to modify the following
MONGODB_URI=mongodb://localhost:27017
CLEAN_HANDS_ISSUER_ADDRESS=3953516660401541564649985379958697237340496801951929947163239598560489169274

# The following variables MUST be changed
ATTESTOR_PRIVATE_KEY=123 
OP_RPC_URL=abc
SUI_PRIVATE_KEY=123
SUI_PRIVATE_KEY_SCHEME=ed25519
```

`MONGODB_URI` - URI for MongoDB. The observer stores ZKP outputs, the user's blockchain address, the user's signature, and the address of the access conditions contract in a collection titled "observations".

`CLEAN_HANDS_ISSUER_ADDRESS` - The address that issued the credentials used as inputs to the ZKP. This is used to validate the issuer address output by the ZKP.

`ATTESTOR_PRIVATE_KEY` - The private key of the account used to issue attestations. This private key is used to create transactions on Optimism. It's account remain funded; otherwise attestations will not be issued.

`OP_RPC_URL` - URL for Optimism RPC node.

`SUI_PRIVATE_KEY` - The output of the `sui keytool export` command.

`SUI_PRIVATE_KEY_SCHEME` - ed25519 | secp256k1 | secp256r1

## Run

```bash
docker pull holonym/observer
```

```
docker run --env-file .env holonym/observer
```
