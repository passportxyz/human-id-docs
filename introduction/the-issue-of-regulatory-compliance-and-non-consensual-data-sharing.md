---
description: While Zero Knowledge Proofs are helpful, they aren't enough...
---

# The Issue of Regulatory Compliance & Non-Consensual Data Sharing

Zero-knowledge KYC prevents data honeypots and massive surveillance. However, on its own, it isn't sufficient for cases where regulators or law enforcement need to investigate users suspected of criminal activity.

As a result, maintaining honeypots has become a necessary compliance practice: organizations store sensitive data that is frequently breached, just to make sure it remains available for investigations.

## Solving The Dilemma: Proof of Clean Hands With Human Network

While ZK proofs (ZKP) aren't trivial, ZKPs with compliance are even more complex. Holonym has built [Human Network](https://x.com/mishtinetwork), an AVS on EigenLayer that allows provable threshold encryption, to unlock ZKPs with programmable privacy for selective and consensual disclosure. The network allows developers to hard-code policies that require pre-existing conditions to occur before a third party can decrypt user data. Users can verify and encrypt their data to the Network's key to create ZKPs which can be "decrypted" by pre-approved parties only when a smart contract is triggered.&#x20;

While this function can be used to gate any sort of data behind programmable policies, it is especially useful for provable encryption of identity.

Human ID packages this functionality into a very easy to use solution called [**Proof of Clean Hands**](https://human.tech/blog/proof-of-clean-hands) that serves as a general-purpose tool for meeting compliance requirements.&#x20;

Once a user has a proof-of-clean-hands SBT (soul bound token), we know the data is encrypted to an observer that can only decrypt a small amount of data per day, making surveillance or honeypots impossible while enabling compliance with law enforcement and regulators.

For more information, see the architecture:

{% content-ref url="../architecture/clean-hands.md" %}
[clean-hands.md](../architecture/clean-hands.md)
{% endcontent-ref %}

Or usage:

{% content-ref url="../for-developers/clean-hands.md" %}
[clean-hands.md](../for-developers/clean-hands.md)
{% endcontent-ref %}
