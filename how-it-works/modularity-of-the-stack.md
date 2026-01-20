---
description: Holonym creates modular infrastructure for privacy, flexibility, and security.
---

# Modularity of the Stack

Holonym applies modularity to the identity verification stack. This fixes a trilemma of security, flexibility, and privacy in identity verification. It has historically been difficult to combine the benefits of centralization (e.g., rigor and ease of use) with decentralization (e.g., flexibility, privacy, and composability). Holonym introduces four layers of identity verification to enable "best of both worlds" approaches.

1. Issuance layer
2. Privacy layer
3. Audit layer
4. ReLayer

## Issuance layer

Unlike blockchains which benefit from being decentralized, _some_ use cases of identity benefit from being centralized. The most common form of rigorous identification is government ID, as centralized as possible. Governments have successfully disbursed over $1 trillion in annual benefits with a relatively low amount of benefit fraud through identity theft. They have been able to conduct large-scale elections using identity to prevent fraudulent votes. These have been possible due to extensive personal information governments are privy to.&#x20;

## Privacy layer

While there is clearly utility (e.g., social security, secure voting processes) in having an institution like a government privy to personal information, the other risks of centralization should be mitigated. Thus, we introduce a privacy layer to separate the issuer from all other aspects of the protocol. This way, an issuer's power is only limited to verifying once; it cannot track subsequent actions of a user.

The privacy is obtained by a global Merkle tree of all credentials. For more information, see

{% content-ref url="hub.md" %}
[hub.md](hub.md)
{% endcontent-ref %}

or

{% content-ref url="credentials.md" %}
[credentials.md](credentials.md)
{% endcontent-ref %}

## Audit Layer

Regulators often need to investigate activities when something goes wrong. Not having identity data available for regulators is illegal in most countries and can have penalties exceeding $100M. Currently, companies record all user activity to have an audit record for regulators, as there has been no other way to comply. In current systems, absent of a way to selectively enforce data privacy, every user is monitored regardless of whether or not they are being investigated.&#x20;

However, in Holonym, the user can be required to encrypt their own audit trail to the data audit layer. The audit layer should have two functions: data availability and permissioned access, which can in turn be two separate layers. Permissioned access involves a smart contract that ensures that nobody can unscrupulously spy on user activity without proper consent or a court order. It enforces that only certain actors under certain conditions can decrypt the data, allowing for provable privacy for honest users while retaining regulatory oversight for bad actors. These conditions are enforced via a smart contract rather than trusting a third party to maintain privacy.

## ReLayer

The relayer layer is the way in which zero-knowledge proofs are sent. Sending and receiving reveal IP addresses and/or wallet addresses if gas is needed. Currently, Holonym uses a relayer to submit transactions and hide the address they came from. It is important that this layer remains flexible so that a relayer node can be replaced by a relayer network, and mixnets can be employed to hide IP addresses.





&#x20;



