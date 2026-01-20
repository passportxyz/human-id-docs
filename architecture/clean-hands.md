# Clean Hands Architecture

Human ID's Proof of Clean Hands establishes that a user

1. has completed KYC,
2. has verified that they are not on any sanctions or PEP lists,
3. has encrypted their identifying data to Human Network, and
4. has selected a smart contract that determines the conditions under which their data can be decrypted.

## Components

The Clean Hands system is made possible by the following:

* Credential issuer.
* Zero knowledge circuit.
* Observer node.
* Smart contract(s).
* Human Network.

The novelty of the Clean Hands system is in its encryption and decryption. Each user encrypts their data to Human Network and _proves_ encryption. When they encrypt, they select a smart contract that determines who is allowed to decrypt. The user's ciphertext is sent to an "observer" node. An entity granted access to the user's ciphertext by the observer node and granted decryption rights by the smart contract can decrypt the user's ciphertext.

## Flow

There are five steps in the Clean Hands flow.

#### 1. Credential issuance

The user verifies their identity and that they are not on any sanctions or PEP lists. Human ID's credential issuer issues a signature to the user attesting to the veracity of the user's identity data and sanctions list status.

#### 2. Proof generation

The user generates a zero knowledge proof. This proof uses the credentials and signature issued by the credential issuer. The proof establishes that the user has completed KYC and sanctions list checks and that they have encrypted their name and date of birth to Human Network. At this step, the user also signs, using the ephemeral private key used in encryption, the address of a smart contract. This smart contract determines who can decrypt the user's data.

#### 3. Attestation issuance

The user sends the proof to an observer node. The observer verifies the proof, stores the proof's public outputs so that the user's ciphertext can be retrieved at a later date by authorized entities, and issues an on-chain attestation to the user. This attestation attests to the fact that the user has completed all of these first 3 steps.

#### 4. On-chain activity

With the attestation, the user is now able to interact with smart contracts that require proof of clean hands, for example, [the Ethereum-Aztec bridge](https://github.com/holonym-foundation/aztec/) which allows verified users to transaction privately.

#### 5. Decryption

If necessary, the ciphertext from the user’s zero knowledge proof is decrypted. The entity that wants to decrypt must request the user's ciphertext from the observer. They must also be granted decryption rights from the smart contract associated with the ciphertext. With the ciphertext and decryption rights, the decrypter requests decryption from Human Network.

## Lists checked for Proof of Clean Hands

* US / OFAC Capta List (CAP)
* US / OFAC Non-SDN Chinese Military-Industrial Complex Companies List (CMIC)
* US / BIS Denied Persons List (DPL)
* US / DOS ITAR Debarred (DTC)
* US / BIS Entity List (EL)
* INT / Financial Action Task Force
* US / FBI Most Wanted
* US / FINCEN Financial Crimes Enforcement Network - 311
* US / OFAC Foreign Sanctions Evaders (FSE)
* INT / Interpol Red Notices
* US / DOS Nonproliferation Sanctions (ISN)
* US / BIS Military End User (MEU) List
* US / OFAC Consolidated Sanctions List (Non-SDN Lists)
* US / OFAC Non-SDN Menu-Based Sanctions List (NS-MBS List)
* US / HRJ US Department of the Treasury Sanctioned Countries
* US / HRJ OFAC Sanctioned Countries (Military)
* US / HRJ OFAC Sanctioned Countries
* INT / Politically Exposed Persons
* US / OFAC Palestinian Legislative Council List (PLC)
* US / OFAC Specially Designated Nationals (SDN)
* US / OFAC Sectoral Sanctions Identifications List (SSI)
* US / DOS Cuba Restricted List
