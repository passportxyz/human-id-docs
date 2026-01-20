---
hidden: true
---

# Off-Chain Proofs

### Overview

The flow is the following:

1. Your site redirects user to Holonym where the user generates the requested proof.
2. After generating the proof, the user is redirected to a callback specified by the organization. The proof is included in the URL query parameters. At the callback location, the organization can verify the proof.

### 1. Redirect user to Holonym

Redirect user to the following URL:

```
https://app.holonym.id/prove/off-chain/<proofType>?callback=<callback>
```

Parameter descriptions:

* `proofType` - Either `uniqueness` (for government ID proof of uniqueness), `uniqueness-phone` (for phone proof of uniqueness), or `us-residency` (for proof of US residency).
* `callback` - The URL to which the user should be redirected once they've generated their proof. The proof will be added a query parameter to this callback. For example, if `callback` is `https://example.com`, the user will be redirected to `https://example.com?proof=<proof>`.

### 2. Verify proof

The user will not be redirected to the callback unless the proof is valid. But you may want to verify the proof yourself.&#x20;

You can use the [@holonym-foundation/off-chain-sdk](https://www.npmjs.com/package/@holonym-foundation/off-chain-sdk) package to verify proofs.

```javascript
import { 
  verifyUniquenessProof
} from '@holonym-foundation/off-chain-sdk';

/**
 * @param proof - The value of the "proof" query parameter passed to the callback URL
 * @returns {Promise<boolean>}
 */
async function verify(proof) {
  const parsedProof = JSON.parse(proof);
  const result = await verifyUniquenessProof(parsedProof);
  console.log(`User is unique: ${result}`);
  return result;
}
```

