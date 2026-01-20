---
description: How to read Clean Hands attestations
---

# Clean Hands Attestations

Human ID issues its Clean Hands attestation to users who prove that they are not on any sanctions lists (see all the lists here: [#lists-checked-for-proof-of-clean-hands](../architecture/clean-hands.md#lists-checked-for-proof-of-clean-hands "mention")).

## Off-chain with Sign Protocol

Use Sign Protocol's scan API to see whether a user has a valid Clean Hands attestation.

```typescript
// Set user address
const address = '0x123'

const resp = await fetch(`https://mainnet-rpc.sign.global/api/scan/addresses/${address}/attestations`)
const data = await resp.json()
const cleanHandsAttestations = data.data.rows.filter((att) => (
  att.fullSchemaId == 'onchain_evm_10_0x8' &&
  att.attester == '0xB1f50c6C34C72346b1229e5C80587D0D659556Fd' &&
  att.isReceiver == true && 
  !att.revoked &&
  att.validUntil > (new Date().getTime() / 1000)
))
const hasValidAttestation = cleanHandsAttestations.length > 0
```

## On Optimism with Sign Protocol

Tutorial coming soon...

## On-chain (not Optimism)

Use our off-chain attester, and verify its signature on-chain. Our attester returns a signature of the circuit ID, action ID, and user address, if the address has a clean hands attestation.

Our attester address is `0xa74772264f896843c6346ceA9B13e0128A1d3b5D`.

#### Query for signature

```typescript
// Set user address
const userAddress = '0x123'

const actionId = 123456789
const resp = await fetch(`https://api.holonym.io/attestation/sbts/clean-hands?action-id=${actionId}&address=${userAddress}`)
const { isUnique, signature, circuitId } = await resp.json();
```

#### Verify signature in Solidity

Warning: this Solidity code is untested.

```solidity
pragma solidity ^0.8.4;

import "openzeppelin-contracts/contracts/utils/cryptography/ECDSA.sol";

contract SignatureVerification {
    using ECDSA for bytes32;

    address public humanIdAttester = 0xa74772264f896843c6346ceA9B13e0128A1d3b5D;

    function verifySignature(
        uint256 circuitId,
        uint256 actionId,
        address userAddress,
        bytes memory signature
    ) public pure returns (bool) {
        bytes32 digest = keccak256(abi.encodePacked(
            circuitId,
            actionId,
            userAddress
        ));

        bytes32 personalSignPreimage = keccak256(abi.encodePacked(
            "\x19Ethereum Signed Message:\n32",
            digest
        ));

        (
            address recovered,
            ECDSA.RecoverError err,
            bytes32 _sig
        ) = ECDSA.tryRecover(personalSignPreimage, publicSignalsSig);

        return recovered == humanIdAttester;
    }
}
```

#### Verify signature in JavaScript

```typescript
// Verify using ethers v5
const digest = ethers.utils.solidityKeccak256(
  ["uint256", "uint256", "address"],
  [circuitId, actionId, userAddress]
);
const personalSignPreimage = ethers.utils.solidityKeccak256(
  ["string", "bytes32"],
  ["\x19Ethereum Signed Message:\n32", digest]
);
const recovered = ethers.utils.recoverAddress(personalSignPreimage, signature)
console.log(recovered === '0xa74772264f896843c6346ceA9B13e0128A1d3b5D')
```

## Sui SBTs

#### TypeScript

We also allow users to mint SBTs on Sui. The following code verifies that the address possesses a Clean Hands SBT on Sui.

```typescript
import { SuiClient, getFullnodeUrl } from '@mysten/sui.js/client';
const PACKAGE_ID = '0x53ddebd997f0e57dc899d598f12001930e228dddadf268a41d4c9a7c1df47a97';
const SBT_TYPE = `${PACKAGE_ID}::sbt::SoulBoundToken`;
const client = new SuiClient({
    url: getFullnodeUrl('mainnet'),
});
async function hasSBT(address: string): Promise<boolean> {
    try {
        // Query all objects owned by the address
        const objects = await client.getOwnedObjects({
            owner: address,
            options: { showType: true }, // Include object type in the response
        });
        // Check if any object matches the SBT type
        const hasSbt = objects.data.some((obj) => {
            const type = obj.data?.type;
            return type === SBT_TYPE;
        });
        return hasSbt;
    } catch (error) {
        console.error('Error querying SBT ownership:', error);
        return false;
    }
}
```

#### Move

See the example [SBT verifier package](https://github.com/holonym-foundation/id-hub-contracts/blob/main/contracts/sui/sbt-verifier/sources/sbt_verifier.move) for how to verify the SBT on chain.

## Decrypt

See [Decryption of Provably Encrypted Data](https://docs.mishti.network/usage-instructions/making-requests-to-mishti-network/decryption-of-provably-encrypted-data).
