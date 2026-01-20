---
description: How to read Holonym attestations on Sign Protocol
---

# Sign Protocol Attestations

Holonym issues an Sign Protocol attestation for every SBT it sends. The following is an example of how to query and validate Holonym attestations.

### **Query**

See the [Sign Protocol docs](https://docs.sign.global/developer-apis/index/api/get/index-service#query-attestations) for how to construct queries.

This query gets the first page of attestations issued by Holonym.&#x20;

Query parameters used:

* `schemaId` - Filters for the HolonymV3 Sign Protocol schema.
* `attestor` - Filters for Holonym's attestor address, `0xB1f50c6C34C72346b1229e5C80587D0D659556Fd`.&#x20;

```bash
curl 'https://mainnet-rpc.sign.global/api/index/attestations?schemaId=onchain_evm_10_0x1&attester=0xB1f50c6C34C72346b1229e5C80587D0D659556Fd'
```

The response looks like this...

```json
{
   "data" : {
      "page" : 1,
      "rows" : [
         {
            "attestTimestamp" : "1714819085000",
            "attestationId" : "0x65",
            "attester" : "0xB1f50c6C34C72346b1229e5C80587D0D659556Fd",
            "chainId" : "10",
            "chainType" : "evm",
            "data" : "0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000004230783732396436363065316330326534653431393734356536313764363433663839376135333836373363636631303531653039336262666135386230613132306200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000050000000000000000000000000000000000000000000000000000000068080e7f000000000000000000000000ededf460a77928f59c27f37f73d4853fd8a0798400000000000000000000000000000000000000000000000000000000075bcd151cb8413a579d6138d257450bb5209579bde35c885f7a4405e3767a5a5d2ea6df03fae82f38bf01d9799d57fdda64fad4ac44e4c2c2f16c5bf8e1873d0a3e1993",
            "dataLocation" : "onchain",
            "extra" : {},
            "from" : "0xB1f50c6C34C72346b1229e5C80587D0D659556Fd",
            "fullSchemaId" : "onchain_evm_10_0x1",
            "id" : "onchain_evm_10_0x65",
            "indexingValue" : "0x1cb8413a579d6138d257450bb5209579bde35c885f7a4405e3767a5a5d2ea6df",
            "lastSyncAt" : null,
            "linkedAttestation" : "",
            "mode" : "onchain",
            "recipients" : [
               "0xEdedf460A77928f59c27f37F73D4853FD8a07984"
            ],
            "revokeReason" : null,
            "revokeTimestamp" : null,
            "revokeTransactionHash" : "",
            "revoked" : false,
            "schema" : {
               "chainId" : "10",
               "chainType" : "evm",
               "data" : [
                  {
                     "name" : "circuitId",
                     "type" : "string"
                  },
                  {
                     "name" : "publicValues",
                     "type" : "uint256[]"
                  }
               ],
               "dataLocation" : "onchain",
               "description" : "Holonym V3 SBT",
               "extra" : {
                  "data" : "{\"name\":\"HolonymV3\",\"description\":\"Holonym V3 SBT\",\"data\":[{\"name\":\"circuitId\",\"type\":\"string\"},{\"name\":\"publicValues\",\"type\":\"uint256[]\"}]}"
               },
               "id" : "onchain_evm_10_0x1",
               "maxValidFor" : "0",
               "mode" : "onchain",
               "name" : "HolonymV3",
               "originalData" : "{\"name\":\"HolonymV3\",\"description\":\"Holonym V3 SBT\",\"data\":[{\"name\":\"circuitId\",\"type\":\"string\"},{\"name\":\"publicValues\",\"type\":\"uint256[]\"}]}",
               "registerTimestamp" : "1714776243000",
               "registrant" : "0xcaFe2eF59688187EE312C8aca10CEB798338f7e3",
               "resolver" : "0x0000000000000000000000000000000000000000",
               "revocable" : true,
               "schemaId" : "0x1",
               "syncAt" : "1714776266473",
               "transactionHash" : "0xfd378e1a6758a5fbafad18f21da353485c8106013dccf501268b6cf126a0eef0"
            },
            "schemaId" : "0x1",
            "syncAt" : "1714819103980",
            "transactionHash" : "0x6216e5793cb20d350680942e97f33688bb075be6c2bbbc960b71d254a2e7bc96",
            "validUntil" : "0"
         },
         ...
      ],
      "size": 100,
      "total": 195
   },
   "message" : "ok",
   "statusCode" : 200,
   "success" : true
}
```

If you are querying for a specific recipient, filter by the recipient.

### Validate

For a Holonym V3 SBT, we always want to verify the _circuit ID_ as well as the _action ID_ and _issuer_ used to generate the ZKP. We want the circuit ID to match whatever circuit we are filtering for. We want the action ID to be the default action ID used for Holonym Sybil resistance proofs. We want the issuer to match whatever Holonym issuer we are filtering for.

See [here](https://github.com/holonym-foundation/holonym-api/blob/main/src/constants/misc.js#L6) for the circuit IDs, action ID, and issuers.

{% code overflow="wrap" %}
```javascript
// We are using Ethers v5
const { ethers } = require("ethers");

// Extract attestation data. 
// This is data.rows[0].data in the API response above
const data = "0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000004230783732396436363065316330326534653431393734356536313764363433663839376135333836373363636631303531653039336262666135386230613132306200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000050000000000000000000000000000000000000000000000000000000068080e7f000000000000000000000000ededf460a77928f59c27f37f73d4853fd8a0798400000000000000000000000000000000000000000000000000000000075bcd151cb8413a579d6138d257450bb5209579bde35c885f7a4405e3767a5a5d2ea6df03fae82f38bf01d9799d57fdda64fad4ac44e4c2c2f16c5bf8e1873d0a3e1993";

const decoded = ethers.utils.defaultAbiCoder.decode(["string", "uint256[]"], data);

const circuitId = decoded[0];

// Public values of the ZKP
const publicValues = decoded[1];

const actionId = publicValues[2];
const issuer = publicValues[4];

// Make sure circuitId matches KYC circuit ID
if (circuitId != "0x729d660e1c02e4e419745e617d643f897a538673ccf1051e093bbfa58b0a120b") {
    throw new Error("Invalid circuit ID");
}

// Validate action ID
if (actionId.toString() != "123456789") {
    throw new Error("Invalid action ID");
}

// Make sure issuer is the KYC Holonym issuer
if (issuer.toHexString() != "0x03fae82f38bf01d9799d57fdda64fad4ac44e4c2c2f16c5bf8e1873d0a3e1993") {
    throw new Error("Invalid issuer")
}
```
{% endcode %}

