# Custom Sybil Resistance

Political, social, and economic systems often need Sybil resistance. Sybil resistance is anything of the form:

_1 person => 1 x_

e.g.,&#x20;

* [ ] 1 person => 1 vote (democracy)
* [ ] 1 person => 1 stimulus check
* [ ] 1 person => social security check
* [ ] 1 person => 1 number of tokens (fair airdrop)
* [ ] 1 person => 1 player (play-to-earn game preventing spam)
* [ ] 1 person => 1 number of comments (social network preventing spam)
* [ ] 1 person => 1 account (generic application preventing spam)
* [ ] 1 person => 1 number of accounts (generic application preventing spam but a little more lenient)



## Standard Sybil Resistance

To just see whether a unique person owns a wallet address, you may visit [start-here.md](start-here.md "mention")



## More Advanced Sybil Resistance

Sometimes you want to prove Sybil resistance for a specific action. E.g., if a user wants to vote in multiple elections from different addresses as to not reveal their voting history. This can be done by giving each election a unique `actionID`.&#x20;

Importantly, this also means bribery would be easier. Imagine an action somebody doesn't care about. That person can provide a new address that `actionID`. One can imagine a DAO vote where a bad actor is passionate about the outcome, and can create 1000 new address, then bribe 1000 random people to register one of those addresses for that specific `actionID`. Since the random people don't care about the vote outcome, they don't mind giving away their uniqueness to the new address for that particular `actionID`.&#x20;

For the default `actionID`, it's far harder to launch a large-scale bribery attack, as giving up a vote to a briber is giving up everything to a briber: all future votes, universal basic income, and airdrops will go to the briber. While some people would gladly give up these rewards for the right price, doing so would require the presentation of government ID in order to participate in a black market. This risk far outweighs any rewards, so we assume such bribery on the default `actionID` will be negligible.&#x20;

## How to set an actionID

To prove uniqueness with respect to a specific action, you must create an actionId. Please use a large random number less than `21888242871839275222246405745257275088548364400416034343698204186575808495617` (the bn254 scalar field order). Let's say your actionId is `11223344556677`, and after your users are done proving uniqueness, you want to them to be sent back to yourwebsite.com/yourcallback.&#x20;

Then, they may visit [https://holonym.io/prove/uniqueness/11223344556677/yourwebsite%2E](https://holonym.io/prove/uniqueness/1234567890/yourwebsite.com)[com%2Fyourcallback](https://holonym.io/prove/uniqueness/1234567890/yourwebsite.com)

You can then view their verification status with respect to the custom actionID by the standard steps in [start-here.md](start-here.md "mention"). Simply replace the `actionID` in the code examples with your custom `actionID`.&#x20;

