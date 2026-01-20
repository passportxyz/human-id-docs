# On-Chain Proofs

To put the proofs on-chain, a Verifier and Relayer are used. The Verifier attests to the proof being complete (since the proof size is too large to cheaply be put on-chain ) and the Relayer posts the attestations on-chain to the Hub contract. _We plan to progressively decentralize this Verifier by achieving consensus amongst a permissionless set of verifiers._



### Hub Contract

Before a user can submit a proof on-chain, the Hub ensures&#x20;

1. An allowed verifier, either a server, network, or smart contract, has verified a VOLE-based ZKP
2. This verifier has posted any necessary data off-chain so others can check its steps in verifying the proof
3. Any nullifier revealed by the ZKP has not been spent

Once a proof is on-chain, the Hub sends a non-transferable ERC721 representing a SBT. The Hub also allows reading the relevant metadata pertaining to the SBT, i.e. the public values of the corresponding proof, via the method `getSBT`.



{% hint style="danger" %}
**Attention** The Hub does not check any custom logic on the public values of the proof beyond the following two standard public input values: nullifier and expiry. While it checks the nullifier has not been used before and the expiry has, it cannot check custom logic. For example, some proofs require that you check the credential's issuer given in the public outputs is actually a trusted issuer of credentials. To do so, you must call `getSBT`and check the public value corresponding to the issuer address is valid.
{% endhint %}



For more information, you may look at the [source code](https://github.com/holonym-foundation/id-hub-contracts/blob/main/contracts/Hub.sol)

