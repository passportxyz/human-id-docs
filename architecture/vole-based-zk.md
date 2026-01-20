# VOLE-based ZK

## What it enables

We implemented an ([open source](https://github.com/holonym-foundation/vole-zk-prover)) VOLE-based prover to enable fast identity proofs that otherwise aren't practical to do with privacy. Privacy requires all proofs to be done on consumer hardware without expensive servers. Yet most interesting proofs such as NFC-enabled passports and email proofs require expensive RSA, SHA256, or Keccak256 operations. The prover can take the existing circuits in circom, where these primitives have already been implemented and audited. We have utilized this to allow mobile browsers to perform proof of passport in seconds without running out of memory.



## How it works

We use a variant of VOLE-based ZK called [VOLE in the head](https://eprint.iacr.org/2023/996), where we modified the linear code to be the [repeat-multiple-accumulate (RMA) code](http://pfister.ee.duke.edu/papers/isita2000.pdf). VOLE-based ZK is orders of magnitude more efficient than popular proving systems, but it is not used for rollups because it is not succinct and difficult to make noninteractive. Yet for identity, especially when offchain, VOLE-based ZK is ideal. VOLE in the head renders VOLE-based ZK noninteractive with roughly a 2x performance cost. We replace the repetition code with the RMA code to have faster performance.

&#x20;VOLE-based ZK consists of two phases:

### 1. Commitment / Preprocessing:

VOLE is performed, giving a two-party "commitment": the verifier receives functions describing many lines with the same slope, and the prover receives random points on each of these lines. The parties do not learn the other parties' values. Notice that by showing a verifier one of his points, a prover can open one of his commitments.&#x20;

A derandomized step is performed so instead of commitment to random values, the parties commit to the witness.

### 2. Proving:

To see the committed output of any addition gates, the verifier just adds her commitments. In fact, any linear operation is trivial because the VOLE commitment scheme is linearly homomorphic.

For multiplication gates, a bit more effort is required: the prover must send the commited values and prove they are correct. We use [quicksilver](https://eprint.iacr.org/2021/076.pdf) for this.

Finally, after all of the circuit's gates' outputs have been learned by the verifier, she can ask the prover to open any of the public outputs.



### Making It Noninteractive

This involves a special type of VOLE based off of SoftSpokenOT, in which the outputs that the prover recieves are independent of the verifier's choices of randomness. This allows the verifier's randomness to be created by the Fiat-Shamir heuristic. However, with this variant of VOLE, the verifier's choices of random values are limited to a very small set, making it too easy for the prover to guess the verifier's choices -- in our case, the verifier has only two options, so this is highly insecure.&#x20;

As a result, linear codes are used to enforce that the prover must guess at least _d_ secrets the verifier chose, where _d_ is the minimum distance of the linear code. For more information see the [VOLE in the head](https://eprint.iacr.org/2023/996) paper.





