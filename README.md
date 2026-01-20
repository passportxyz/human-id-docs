# Private Identity Verification

Human ID is a privacy-preserving identity protocol developed by Holonym Foundation for the [human.tech](https://human.tech/) framework. Human ID uses zero knowledge proofs to allow for proving facts about oneself without revealing the whole identity, enabling organizations and smart contracts to verify their users without storing any sensitive information.

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td>Explore the human.tech framework</td><td><a href=".gitbook/assets/u1666481745_a_glassy_translucent_human_figure_stepping_throug_4da004d5-8514-43b0-8fb0-0038ad413c6b_0 3.png">u1666481745_a_glassy_translucent_human_figure_stepping_throug_4da004d5-8514-43b0-8fb0-0038ad413c6b_0 3.png</a></td><td><a href="https://docs.human.tech/">https://docs.human.tech/</a></td></tr></tbody></table>

## Motivation

Identity verification is often required for legal or practical reasons such as KYC, fraud prevention, proof-of-age, and Sybil resistance. However, verifying identity while also preserving privacy has historically been difficult. This problem is even more prominent in web3, where data is stored on a public ledger. Not only is obtaining privacy on a blockcchain difficult, but the adverse affects of losing privacy are greater: "doxxing" a user's wallet address once reveals all of the address' past and future activity.

Human ID is a private credential system part of the human.tech tool-set that makes privacy-preserving identity possible and simple for both on- and off-chain use cases.

## Privacy-preserving proofs

With a Human ID, user can prove a variety of statements without revealing their identity. These include facts needed for meeting legal requirements, Sybil resistance to prevent bot attacks, and account or key recovery. Example statements are:

> "I have US residency"
>
> "I have non-US residency"
>
> "I have never received this airdrop from any other crypto address"
>
> "I am an accredited investor"
>
> "I am over 18"
>
> "I am the same person who created this wallet" if the wallet needs recovery
>
> "I have never voted in this DAO's governance"
>
>

{% hint style="success" %}
Many of these are ready to use out-of-the-box, and others can be enabled by [contacting us](mailto:hello@holonym.id) and/or contributing to our [GitHub](https://github.com/holonym-foundation)
{% endhint %}

{% hint style="info" %}
**Note**: Proof generation currently takes about 30ms-4s on standard consumer devices, depending on the proof type and the device's specifications.&#x20;
{% endhint %}

To start using Human ID in your (d)App, continue to

{% content-ref url="for-developers/start-here.md" %}
[start-here.md](for-developers/start-here.md)
{% endcontent-ref %}

Otherwise, take a peak into the architecture of human.tech

{% content-ref url="architecture/overview.md" %}
[overview.md](architecture/overview.md)
{% endcontent-ref %}

Continue on to learn about programmable privacy for meeting legal requirements across compliance regimes,
