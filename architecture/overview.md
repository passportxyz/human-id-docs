---
description: How Holonym works
---

# Overview

### Actors and actions in Holonym

At a high level, the Holonym system can be understood in terms of the following _actors_ and _actions_.

#### **Actors**

* **User.** Someone who receives credentials and prove facts about themselves, often for the purpose of fulfilling requirements set by an _organization_.
* **Issuer.** A party that issues credentials to users. In the process of issuing credentials, an issuer may use 3rd party identity providers to verify users' identities.
* **Consumer.** A party that modifies its users' access or actions based on the facts its users have proven about themselves using. For example, an organization may require its users to reside in a certain country in order to vote on certain proposals.

#### **Actions**

1. Issuer issues a credential to a user. In the user flow, this step is called "issuance".
2. User proves facts about themselves using their credentials. In the user flow, this step is called "proving".
3. Consumer allows a user to access something or to perform an action, depending on what the user has proven.
