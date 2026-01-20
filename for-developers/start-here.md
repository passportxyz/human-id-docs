---
description: What you can do with Human  ID and how to start
---

# Integrating Human ID

While Holonym can be used off-chain it is by far most commonly used on-chain. We use the term Soul-Bound Token (SBT) to refer to an on-chain record linked to a user's address. To use Holonym, you simply must direct the user to get their SBT, then you can read the SBT from from our API in one line of code, or from the blockchain itself.

## 1. Send a user to Holonym to get an SBT

### Option A: Via Link or Redirect

You may send the user to

```
https://id.human.tech/<credentialType>
```

&#x20;`credentialType` is one of:

* `gov-id` (for government ID credentials).
* `clean-hands` (for sanctions checks).
* `phone` (for phone number credentials).
* `biometrics` (for non-personally identifying face uniqueness and liveness check).

#### ePassport

Users can also verify for free using an NFC-enabled government ID document. Please see [how-to-verify-epassport.md](../for-users/how-to-verify-epassport.md "mention") for user instructions.

Note that the ePassport SBT is distinct from the gov-id SBT. A user can get both an ePassport SBT and a gov-id (KYC) SBT.

### Option B: Via Human ID SDK

To directly embed Human ID into your website, you can import the [Human ID SDK](https://www.npmjs.com/package/@holonym-foundation/human-id-sdk) and call

```javascript
humanID.requestSBT(credentialType)
```

where `credentialType` is either `'kyc'` (for government ID credentials), `'clean-hands'`,  `'phone'` (for phone number credentials) or `'biometrics'`.\
This will prompt the user to complete the SBT minting flow.

## 2. Read a user's SBT

### Option A: Holonym API

To check whether a user has a certain SBT, you can query the Holonym API. The JavaScript example shows how to call the Holonym API (which calls the SBT smart contract) to get whether the user is unique for the given action ID. (Use the default action ID of `123456789`.)

{% tabs %}
{% tab title="JavaScript" %}
```javascript
// `action-id` is the action ID. The only action ID currently 
// in use is 123456789.
// `user` is the address to be checked.
const resp = await fetch('https://api.holonym.io/sybil-resistance/gov-id/optimism?action-id=123456789&user=0x0000000000000000000000000000000000000000');
const { result: isUnique } = await resp.json();
```
{% endtab %}
{% endtabs %}

### Option B (DEPRECATED): On chain

You can call the SBT contract directly. The Solidity example gives anyone 1 gwei who has proven they're from the US. (See more examples in the [examples repo](https://github.com/holonym-foundation/id-hub-contracts/tree/main/contracts/examples).)

{% tabs %}
{% tab title="Solidity" %}
<pre class="language-solidity"><code class="lang-solidity"><strong>// NOTE: This example is deprecated with Holonym V3.
</strong>
<strong>pragma solidity ^0.8.0;
</strong>import "../interfaces/IResidencyStore.sol";

// US Residents have had it hard this year! let's send 1 gwei to anyone who can prove they're from the US
contract USResidency {
    IResidencyStore resStore;
    constructor() {
        resStore = IResidencyStore(0x7497636F5E657e1E7Ea2e851cDc8649487dF3aab); //ResidencyStore address can be found on https://github.com/holonym-foundation/app.holonym.id-frontend/blob/main/src/constants/proofContractAddresses.json
    }

    // NOTE: there are better ways to send ETH. Please don't copy this code
    function sendStimmy() public {
        require(resStore.usResidency(msg.sender), "You have not proven you are from the US");
        payable(msg.sender).send(1);
    }
}
</code></pre>
{% endtab %}
{% endtabs %}

You may have noticed one person can claim the gwei many times in the Solidity example! This an example; please do not implement a US stimulus program from it.

{% hint style="info" %}
**Note:** by default, proofs can only be done once-per-ID to prevent Sybil attacks & bribery. You don't want somebody to sell their US residency proof to others. This means a user can only verify one of their addresses as being a US resident.
{% endhint %}

## Example: Sybil Resistance

### 1. Send users to Holonym to get government ID uniqueness SBTs

Send users to the following URL.

```
https://id.human.tech/gov-id/kyc/issuance/prereqs
```

Users will complete verification and get an SBT at the address of their choosing.

### 2. Read users' uniqueness SBTs

The below JS example checks whether an address belongs to a unique person.

The smart contract in Solidity gives a privacy-preserving airdrop once-per-person to only unique people.

{% tabs %}
{% tab title="JavaScript" %}
<pre class="language-javascript"><code class="lang-javascript"><strong>//`user` is the address that should be checked for uniqueness
</strong><strong>// `action-id` should be 123456789, unless you specified a custom action ID
</strong><strong>const resp = await fetch('https://api.holonym.io/sybil-resistance/gov-id/optimism?user=0x0000000000000000000000000000000000000000&#x26;action-id=123456789');
</strong>const { result: isUnique } = await resp.json();
</code></pre>
{% endtab %}

{% tab title="Solidity" %}
```solidity
// NOTE: This example is deprecated with Holonym V3

pragma solidity ^0.8.0;
import "../interfaces/IAntiSybilStore.sol";

/* NOTE: you can replace airdrop with any other action that needs Sybil resistance. 
This uses an airdrop as an example, but it can be adapted for voting, play-to-earn, play-to-learn, etc.
Example: You want to give an airdrop to all your early users. You want to make your early users get their fare share and far more than they would get if it was just bots swooping up the airdrop and dumping.
*/
contract SybilFreeAction {
    IAntiSybilStore registeredActions; // Where actions (including the actionId we care about) are registered
    uint actionId; // The ID for this particular action. We reccomend the default ID of 123456789
    mapping (address => bool) hasClaimedAirdrop;

    constructor(uint actionId_) {
        registeredActions = IAntiSybilStore(0xFcA7AC96b1F8A2b8b64C6f08e993D6A85031333e); // AntiSybilStore address for different chains can be found on https://github.com/holonym-foundation/app.holonym.id-frontend/blob/main/src/constants/proofContractAddresses.json
        actionId = actionId_;
    }

    function claimAirdrop() public {
        require(registeredActions.isUniqueForAction(msg.sender, actionId), "You have not yet claimed this action with your Holo");
        require(!hasClaimedAirdrop[msg.sender], "You already got your airdrop!");
        hasClaimedAirdrop[msg.sender] = true;
        _giveAirdrop(msg.sender);
    }

    function _giveAirdrop(address recipient) private {
        // not implemented
    }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**Note**: we highly recommend using the default actionID of `123456789`. If you would like to explore custom actionIDs, please see [custom-sybil-resistance.md](custom-sybil-resistance.md "mention"). Otherwise, feel free to ignore the concept of an actionID and just use `123456789`as shown in the example.
{% endhint %}
