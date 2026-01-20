---
description: How to read Human ID SBTs on Stellar
---

# Stellar

Users can opt to receive SBTs on Stellar. See [using-with-stellar.md](for-users/using-with-stellar.md "mention") for user-facing instructions.

### Off-chain

Use the Stellar SDK to simulate a read transaction to get the user's SBT.

```typescript
import {
  Horizon,
  rpc,
  TransactionBuilder,
  Networks,
  Contract,
  scValToNative,
  nativeToScVal,
} from '@stellar/stellar-sdk'

/// Types ///
type StellarSbt = {
  action_nullifier: bigint,
  circuit_id: bigint,
  expiry: bigint,
  id: bigint,
  minter: string, // Stellar address
  public_values: Array<bigint>,
  recipient: string, // Stellar address
  revoked: boolean
}
type StellarSbtStatus = 'valid' | 'expired' | 'revoked' | 'none'
type GetStellarSBTRetVal = {
  sbt?: StellarSbt
  status: StellarSbtStatus
}

/// Constants ///
const horizonServerUrl = 'https://horizon.stellar.org'
const sorobanRpcUrl = 'https://mainnet.sorobanrpc.com'
const stellarSBTContractAddress = 'CCNTHEVSWNDOQAMXXHFOLQIXWUINUPTJIM6AXFSKODNVXWA4N7XV3AI5'

/// Get SBT ///
async function getStellarSBTByAddress(
  address: string,
  circuitId: string
): Promise<GetStellarSBTRetVal | null> {
  const sorobanServer = new rpc.Server(sorobanRpcUrl)
  const userAccount = await sorobanServer.getAccount(address)
  const contract = new Contract(stellarSBTContractAddress)

  const operation = contract.call(
    'get_sbt',
    nativeToScVal(address, { type: 'address' }),
    nativeToScVal(circuitId, { type: 'u256' })
  )

  const transaction = new TransactionBuilder(userAccount, {
    Networks.PUBLIC,
    fee: '100',
  })
    .addOperation(operation)
    .setTimeout(60)
    .build()

  const simulationResponse = await sorobanServer.simulateTransaction(transaction)
  const parsed = rpc.parseRawSimulation(simulationResponse)

  // Happy path. User has the desired SBT.
  if (rpc.Api.isSimulationSuccess(simulationResponse)) {
    const _parsed = parsed as rpc.Api.SimulateTransactionSuccessResponse

    if (!_parsed.result) {
      throw new Error('Unexpected: Could not get "result" field from parsed Stellar transaction simulation for SBT query')
    }

    const sbt = scValToNative(_parsed.result?.retval)

    return {
      sbt,
      status: 'valid'
    }
  }
  // Error cases. User does not have the SBT.
  else if (rpc.Api.isSimulationError(simulationResponse)) {
    const _parsed = parsed as rpc.Api.SimulateTransactionErrorResponse

    // Error code 1 is "SBT not found"
    if (_parsed.error.includes('HostError: Error(Contract, #1)')) {
      return { status: 'none' }
    }

    // Error code 5 is "SBT revoked"
    if (_parsed.error.includes('HostError: Error(Contract, #5)')) {
      return { status: 'revoked' }
    }

    // Error code 6 is "SBT expired"
    if (_parsed.error.includes('HostError: Error(Contract, #6)')) {
      return { status: 'expired' }
    }

    throw new Error(`Stellar transaction simulation for SBT query failed: ${_parsed.error}`)
  } else {
    throw new Error('Unexpected: Stellar transaction simulation for SBT query returned an unexpected response')
  }
}

```

### On-chain (Soroban)

#### 1. Fetch the wasm for the Human ID SBT contract.

```bash
soroban contract fetch --id CCNTHEVSWNDOQAMXXHFOLQIXWUINUPTJIM6AXFSKODNVXWA4N7XV3AI5 -o ./human_id_sbt_contract.wasm --network mainnet --network-passphrase "Public Global Stellar Network ; September 2015" --rpc-url https://mainnet.sorobanrpc.com
```

#### 2. Incorporate into your Soroban contract.

This example contract has a `get_sbt` function that simply wraps the function from the SBT contract.

Check out the full [Human ID SBT contract here](https://github.com/holonym-foundation/id-hub-contracts/blob/main/contracts/stellar/human-id/contracts/sbt/src/lib.rs).

For the list of circuit IDs, see [here](https://github.com/holonym-foundation/holonym-api/blob/235c57310781e66d19cf5ad675132088d266da87/src/constants/misc.js#L18C1-L25C72).

```rust
#![no_std]
use soroban_sdk::{contract, contractimpl, Env, Address, U256};

mod human_id_sbt_contract {
    soroban_sdk::contractimport!(
        file = "../../human_id_sbt_contract.wasm"
    );
}

#[contract]
pub struct MyContract;

#[contractimpl]
impl MyContract {
    /// * `recipient` - The address of the SBT recipient.
    /// * `circuit_id` - This determines the type of SBT to look up. For example, KYC, ePassport, and phone number
    ///   SBTs each have a different circuit ID.
    pub fn get_sbt(env: Env, recipient: Address, circuit_id: U256) -> human_id_sbt_contract::SBT {
        let human_id_sbt_contract_addr = Address::from_str(&env, "CCNTHEVSWNDOQAMXXHFOLQIXWUINUPTJIM6AXFSKODNVXWA4N7XV3AI5");
        let human_id_sbt_client = human_id_sbt_contract::Client::new(&env, &human_id_sbt_contract_addr);

        human_id_sbt_client.get_sbt(&recipient, &circuit_id)
    }
}
```
