---
description: You got questions? We got answers!
---

# FAQs

<details>

<summary>What is human.tech?</summary>

human.tech is a technology platform that enhances the security, transparency, and compliance of digital interactions through advanced cryptographic techniques and decentralized networks.

</details>

<details>

<summary>How does human.tech's technology work?</summary>

human.tech leverages blockchain technology to create secure, immutable records of transactions and digital identities to ensure data integrity and privacy.

</details>

<details>

<summary>What are human.tech's main products?</summary>

* **Human ID:** A private digital identity solution that verifies users without storing or seeing sensitive information. Data stays on user devices, and only proofs of identity and compliance are submitted.
* **Human Network:** Provides decentralized primitives for a better web. It enables privacy-preserving KYC with AML, and supports account login and recovery based on passwords and privacy-preserving biometrics.
* **Human Wallet:** A secure, noncustodial onboarding solution that resembles a wallet as a service but is self-custodial, free, and addresses security issues inherent in embedded wallets. It uses the Human Network for secure onboarding and recovery.
* **Human Passport:** An identity verification application and Sybil resistance protocol with more than 2M users. It enables users to collect verifiable credentials, or Stamps, that prove their identity and trustworthiness without exposing personally identifying information. To date, Human Passport has protected over $225M in airdrop and grant funds.

</details>

<details>

<summary>How does Human ID ensure data security?</summary>

Human ID uses advanced cryptographic techniques to protect data from unauthorized access and tampering, ensuring only authorized parties can access sensitive information.

</details>

<details>

<summary>What measures does Human ID take to protect user privacy?</summary>

Human ID technology keeps data on user devices, submitting only proofs of compliance and identity. This approach ensures that sensitive information is never exposed or stored centrally.

</details>

<details>

<summary>What is ZK KYC? How is it different from KYC?</summary>

When most people hear KYC, they think “loss of privacy”, “centralization”, and “getting doxxed”. Holonym uses “Zero Knowledge” KYC that prevents any identity authority or third party (including us) from linking your credentials with your wallet address.

This means that even if a third party KYC provider snoops on your data, there is no way to know which person you are on-chain.\\

</details>

<details>

<summary>How does Human ID help with regulatory compliance?</summary>

Human ID automates compliance processes through real-time transaction monitoring and smart contracts, reducing administrative burdens and ensuring adherence to regulatory standards.

</details>

<details>

<summary>What specific regulatory standards does Human ID support?</summary>

Human ID supports various regulatory standards by providing privacy-preserving KYC and AML capabilities, facilitating easier compliance with financial regulations.

</details>

<details>

<summary>What should I do if my wallet was hacked?</summary>

We’re sorry to hear about your situation. Unfortunately, we cannot change your wallet address, as we do not support proof revocation. You will need to wait until your verification expires in one year before you can verify again. You may proceed with a refund then.

</details>

<details>

<summary>Where can I find the proof contracts?</summary>

You can find a complete list of ZK-SBT contracts supported by Holonym here: [https://github.com/holonym-foundation/holonym-relayer/blob/main/constants/contract-addresses.json\
<br>](https://github.com/holonym-foundation/holonym-relayer/blob/main/constants/contract-addresses.json)Just go to [https://optimistic.etherscan.io/](https://optimistic.etherscan.io/) and search the contract address to see more information.

</details>

<details>

<summary>I minted my ZK SBT to the wrong address! Can I transfer it?</summary>

It is not possible to transfer SBTs at this time because it breaks Sybil Defense with the current implementation.

</details>

<details>

<summary>How does Human ID guarantee privacy for me?</summary>

Human ID is designed to minimize any chance of data leakage. For any form of document-based verification, some agent has to assess a document and check it against a large centralized database. Almost all KYC providers in Web3 keep a centralized database that can be linked to an individual wallet address (see FTX disclosure).\
\
Holonym allows users to prove on-chain facts about themselves while making it highly difficult, near impossible, to link a real world document to an on-chain identity. Not even we can link your data to your wallet.

**How is this possible?**

1. During verification, all data is sent over an encrypted channel to a third party credential issuer.
2. We request deletion of the data by the third party as soon as it is successfully verified and handed back to the user. In the event document is not successfully verified, the record is logged and kept for up to 90 days to help users debug why their verification failed. It is deleted after the 90 day period.
3. We use a nullifier scheme to make it nearly impossible for the third party issuer to track the user after they verify them. This means that the user encrypts the cryptographically signed credential with a secret only they know after receiving it from the identity issuer.
4. We use a relayer to add a user’s leaf to the Merkle Tree so the verified credentials (VCs) can’t be linked to an on-chain address. The Merkle Tree is an identity mixer, or anonymity set, that the user can prove set membership and knowledge of specific attributes about from a pseudonymous ethereum address.
5. The user’s identity data is stored on the user’s local device. All ZK proofs on identity are computed on the client side, not our server.
6. We keep an encrypted backup of user data in the event that the user clears their cookies, local storage, or switched devices. The data is encrypted with the user’s ethereum key and can be requested to be deleted at any time by contacting the team.
7.  Users can increase their privacy by using:

    \-- a VPN network (be careful to use the same country when verifying)

    \-- a privacy preserving browser like Tor\
    \-- waiting some period of time after adding their leaf to the merkle tree before minting their SBT

</details>

<details>

<summary>How many points do I earn for verifying my identity?</summary>

You’ll earn 16 points for KYC verification and 1.5 points for verifying your phone number.

</details>

<details>

<summary>Is there a specific age limit for completing ID verification with Human ID?</summary>

Yes, you need to be at least 18 years old.

</details>

<details>

<summary>My ID verification always fails. Can I submit my ID through a ticket and have it verified there?</summary>

Unfortunately, manual review of IDs isn't supported; the process relies on automation.

</details>

<details>

<summary>Can I obtain a Human Passport stamp by verifying my phone number?</summary>

Yes, phone verification is now supported.

</details>

<details>

<summary>What type of documents can I use to verify my identity for proof of uniqueness?</summary>

You can verify with any of the following standard un-expired and valid physical documents that match your identity:

* passport
* driver's license
* national ID
* residence card
* voter's ID

We also have expanded support for less common identity cards to meet the communities needs such as:

* Provisional driver's licenses
* India PAN cards
* India Local ID cards
* Russian internal passport
* Nigeria voter cards
* USA passport card
* Swedish Identitetskort
* Spain ID

We do not support docs that have high likelihood of forgery such as:

* "Federal Limits Apply" US IDs
* Italian Refugee Cards
* Italian Paper ID cards
* Student ID cards
* Nigerian National Identification Numbers

</details>

<details>

<summary>Are there any countries that aren't supported for verification?</summary>

Yes, the unsupported countries are North Korea, Iran, Syria, and Cuba.

</details>

<details>

<summary>Verification complete! What’s next?</summary>

Continue to the minting page:

1. Connect your wallet: [https://silksecure.net/holonym/diff-wallet](https://silksecure.net/holonym/diff-wallet).
2. Select Phone Verification or KYC/Government ID Verification.
3. Click "Mint SBT" to finalize the process.
4. Pay on your preferred Network. Confirm the payment on your wallet.
5. Get SBT on Optimism Network if you are verifying for Human Passport.
6. Confirm that the wallet address is correct, as you can only receive **one** SBT of this type and cannot transfer it to another wallet.<br>

You're all set!

</details>

<details>

<summary>Are there any verification guidelines I can use to avoid common issues?</summary>

To ensure a smooth verification process, please follow these steps:

• **Clear Photos** – Avoid blurriness by holding your phone still while capturing your ID and selfie. \
• **Scan Both Sides** – If using a driver's license, upload both front and back (no duplicates). \
• **Unaltered Documents** – Russian internal passports must have a visible, unedited machine-readable area. \
• **Valid ID** – Ensure your document is not expired. \
• **Selfie Match** – Your selfie must clearly match the photo on your ID. \
• **Use a Physical Document** – Do not take a picture of an ID displayed on a screen. \
• **Age Requirement** – You must be 18+ years old to verify.

⚠️ **Unsupported Countries:** North Korea, Iran, Syria, and Cuba. ⚠️ Unsupported IDs: Nigeria's NIN and Bangladesh's resident permit.

Following these guidelines will help prevent verification issues.

</details>

<details>

<summary>Can I verify again but with a different wallet?</summary>

No, each user can verify only one wallet per identity. If the system detects multiple verification attempts with different wallets, they will all fail. If you encountered an _'already registered'_ error, do not attempt verification again. For now, your only option is to request a refund and wait until the first verification expires after a year before trying again.

</details>

<details>

<summary>How can I proceed with verification after making a payment?</summary>

If your payment was successful, you can go ahead and verify directly on the website.

Note: do not make another payment if you were not redirected to the verification page the first time. If you're having trouble and can't continue, open a support ticket for assistance.

</details>

<details>

<summary>What to do if my verification was unsuccessful?</summary>

If verification was unsuccessful, you can request a refund by following the refund steps outlined in our FAQs.

</details>

<details>

<summary>Can I retry verification if my first attempt failed?</summary>

After receiving your refund, you can try again, but we recommend using a different ID than the one you previously used.

</details>

<details>

<summary>How can I verify my identity?</summary>

You can verify your identity using standard unexpired and valid physical documents such as a passport, driver's license, national ID, or residence card. Unsupported documents include student IDs, Nigeria's NINS, and Bangladesh's resident permit IDs.

You can review the supported countries and documents with the following link: \
• [https://onfido.com/supported-documents](https://onfido.com/supported-documents/)

</details>

<details>

<summary>Is verification a one-time fee, or will I need to re-verify later?</summary>

Verification is valid for one year. After that, you’ll need to pay again to verify your identity.

</details>

<details>

<summary>Do I need to verify and mint SBT again if I’ve already minted it on the old app?</summary>

No, you don’t need to verify again as long as your previous verification is still valid.

</details>

<details>

<summary>What are the SBT Contract addresses?</summary>

Here are the SBT contract addresses:

**• For V3:** 0x2AA822e264F8cc31A2b9C22f39e5551241e94DfB \
&#xNAN;**• For Legacy ID:** 0x7A81f2f88b0eE30eE0927c9F672487689C6dD7Ce \
&#xNAN;**• For Legacy Phone:** 0xe337aD5aA1Cb84e12a7Aab85aEd1Ab6cb44C4a8e

</details>

<details>

<summary>I minted my ZK SBT to the wrong address! Can I transfer it?</summary>

SBTs (Soulbound Tokens) are intentionally non-transferable, making them a key part of our Sybil Defense system. To verify a different wallet, you must wait for the current verification proof to expire, which happens one year after minting.

</details>

<details>

<summary>Can I try verifying again with a different ID?</summary>

Each paid verification allows for one attempt. If it fails, you can request a refund and try again, but you'll need to submit a new payment and restart the process.&#x20;

</details>

<details>

<summary>Do I get a full refund if my verification failed?</summary>

If your verification fails, you can request a full refund.&#x20;

</details>

<details>

<summary>How do I request a refund?</summary>

Follow these steps to request a refund:

**Step 1 (Optional):** Watch a step-by-step refund walkthrough on YouTube: [https://www.youtube.com/watch?v=QkGi0VHvmuA](https://www.youtube.com/watch?v=QkGi0VHvmuA)

**Step 2:** Go to the Refund Page: [https://silksecure.net/holonym/diff-wallet](https://silksecure.net/holonym/diff-wallet)

**Step 3:** Look for the "Eligible for Refund" tag next to your card.

**Step 4:** Click the "Eligible for Refund" label and follow the instructions to complete the process.

</details>

<details>

<summary>What to do if I received the "Failed" message?</summary>

This means your verification attempt was unsuccessful. This is not a system bug, as the process is managed by the provider and subject to their approval. Verification failures can happen for various reasons, and unfortunately, we do not have the exact reason for each unsuccessful attempt. If you wish to try again, please use a different ID than the one you previously used.

</details>

<details>

<summary>Should I add my country code when entering phone number for verification?</summary>

Do NOT include the country code when entering your phone number. Select your country from the provided list instead.

</details>

<details>

<summary>I have troubles receiving OTP, what should I do?</summary>

**Follow this OTP Troubleshooting Guide:**

1. Do NOT include the country code when entering your phone number. Select your country from the provided list instead.
2. Restart your phone to refresh the connection with your network provider.
3. Check WhatsApp and Viber if SMS isn't received.

**Important Notes:**&#x20;

• Burner phones and virtual numbers are NOT accepted due to security reasons, as they weaken Sybil resistance.&#x20;

• Do NOT keep retrying without following these steps—doing so may lock you out after reaching the maximum attempts, preventing verification.&#x20;

• You have a maximum of three attempts. If you fail all three times, your only option is to request a refund. Please follow the refund process.

</details>

<details>

<summary>I received "Loading Credentials" error, what should I do?</summary>

If you received a _"Loading Credentials"_ error, follow these steps:

1. Allow up to 30 minutes for the provider to complete your ID verification.
2. DO NOT close, refresh, or exit the page during this process.
3. Once verification is complete, check your profile— the _"Verify"_ label on your picture will change to _"Verified"_.
4. After verification, click "Prove" to proceed with minting your SBT.

</details>

<details>

<summary>What to do if I received the "Failed to Fetch or V3SybilResistance.r1cs" error while minting SBT?</summary>

If you receive a _"Failed to Fetch or V3SybilResistance.r1cs"_ error during the minting process, try the following solutions:

• **Use a Different Browser or Device** \
Copy the minting page link and open it in another browser or device.

**• Uninstall Internet Download Manager (IDM)** \
IDM can interfere with the minting process, so removing it may resolve the issue.

If the issue persists, try clearing your cache or using incognito mode.

</details>

<details>

<summary>Why am I getting an "Already Registered" error?</summary>

If you receive an _"Already Registered"_ message, it means you have already completed verification with the same wallet or with different wallet.&#x20;

Each user can verify only once and submit a valid proof one single time. Any further attempts will result in errors. This rule ensures Holonym's sybil resistance.

</details>

<details>

<summary>What should I do if I'm facing issues with the payment page?</summary>

If you're having trouble with the payment page, try clearing your browser's cache and restarting it, then attempt again. You can also try using a different browser.

</details>

<details>

<summary>What should I do if verification still fails after two attempts?</summary>

• If you've failed twice, we don’t recommend  trying again unless you have a different ID and are willing to restart the entire process.&#x20;

• Even if you try again with a different ID, successful verification is not guaranteed, as it is still subject to the provider's approval.

</details>

<details>

<summary>How to unlock more Discord channels and roles?</summary>

:bulb: **Roles and Permissions**: Some channels require specific roles to access. Make sure you have the right roles to unlock more content.&#x20;

1. **For Token-Gated Roles**: Go to the :white\_check\_mark:│verify-holo channel and connect your wallet to Collab.Land.
2. **Discover More Channels & Roles**: \
   • Click “Channels & Roles” in the left sidebar. \
   • Select “Customize” to answer a few questions and gain access to additional channels and roles. \
   • Use “Browse Channels” to filter and join channels that match your interests.&#x20;
3. **View Hidden Channels**: If some channels appear collapsed, click on the category name to expand and see all available channels.

</details>
