---
description: How to read Holonym attestations on Verax
---

# Verax Attestations

Holonym issues a Verax attestation for every SBT it sends. The following is an example of how to query and validate Holonym attestations.

### Install

```bash
npm i @verax-attestation-registry/verax-sdk
```

### Code

In this example, we check the attestation for the address `0xdcA2e9AE8423D7B0F94D7F9FC09E698a45F3c851`.

It's important to filter for Holonym's schema ID and attester account and to validate the attestation payload. Here, we are making sure the user has a uniqueness KYC SBT.

See [here](https://github.com/holonym-foundation/holonym-api/blob/main/src/constants/misc.js#L6) for the circuit IDs, action ID, and issuers.

```javascript
const { VeraxSdk } = require('@verax-attestation-registry/verax-sdk');

// Initialize Verax
const address = '0xdcA2e9AE8423D7B0F94D7F9FC09E698a45F3c851';
const veraxSdk = new VeraxSdk(
  VeraxSdk.DEFAULT_LINEA_MAINNET, 
  address
);

// Query attestation registry
const attestations = await veraxSdk.attestation.findBy(
  undefined,
  undefined,
  {
    // Holonym's schemaId
    schemaId: "0x1c14fd320660a59a50eb1f795116193a59c26f2463c0705b79d8cb97aa9f419b",
    // Holonym's attester account
    attester: "0xB1f50c6C34C72346b1229e5C80587D0D659556Fd",
    // Our address of interest
    subject: "0xdcA2e9AE8423D7B0F94D7F9FC09E698a45F3c851"
  }
);

const { circuitId, publicValues, revoked } = attestations[0].decodedPayload[0];

const actionId = publicValues[2].toString();
const issuer = publicValues[4];

// Make sure circuitId matches KYC circuit ID
if (circuitId != "0x729d660e1c02e4e419745e617d643f897a538673ccf1051e093bbfa58b0a120b") {
    throw new Error("Invalid circuit ID");
}

// Validate action ID
if (actionId != "123456789") {
    throw new Error("Invalid action ID");
}

// Make sure issuer is the KYC Holonym issuer
if (issuer != BigInt("0x03fae82f38bf01d9799d57fdda64fad4ac44e4c2c2f16c5bf8e1873d0a3e1993")) {
    throw new Error("Invalid issuer")
}
```
