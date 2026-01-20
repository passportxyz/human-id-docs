---
description: Reference for https://api.holonym.io
---

# API Reference

## Endpoints

* [**GET** `/sybil-resistance/gov-id/<network>`](api-reference.md#get-sybil-resistance-less-than-credential-type-greater-than-less-than-network-greater-than-user-less)
* [**GET** `/sybil-resistance/epassport/<network>`](api-reference.md#get-sybil-resistance-less-than-credential-type-greater-than-less-than-network-greater-than-user-less)
* [**GET** `/sybil-resistance/phone/<network>`](api-reference.md#get-sybil-resistance-less-than-credential-type-greater-than-less-than-network-greater-than-user-less)
* [**GET** `/sybil-resistance/biometrics/<network>`](api-reference.md#get-sybil-resistance-less-than-credential-type-greater-than-less-than-network-greater-than-user-less)
* [**GET** `/residence/country/us/<network>`](api-reference.md#get-residence-country-us-less-than-network-greater-than-user-less-than-user-address-greater-than)
* [**GET** `/snapshot-strategies/residence/country/us`](api-reference.md#get-snapshot-strategies-residence-country-us-network-less-than-network-greater-than-and-snapshot-les)
* [**GET** `/snapshot-strategies/sybil-resistance/gov-id`](api-reference.md#get-snapshot-strategies-sybil-resistance-gov-id-network-less-than-network-greater-than-and-snapshot)
* [**GET** `/snapshot-strategies/sybil-resistance/phone`](api-reference.md#get-snapshot-strategies-sybil-resistance-phone-network-less-than-network-greater-than-and-snapshot-l)
* [**GET** `/snapshot-strategies/sybil-resistance/biometrics`](api-reference.md#get-snapshot-strategies-sybil-resistance-biometrics-network-less-than-network-greater-than-and-snaps)

#### **GET** `/sybil-resistance/<credential-type>/<network>?user=<user-address>&action-id=<action-id>`

Get whether the user has registered for the given action-id.

When a user "registers", they are establishing that the given blockchain address is a unique person for the action ID. See the section Sybil resistance for more information about how action IDs can be used.

If `credential-type` is `gov-id`, this endpoint uses Holonym smart contracts to check whether the user has completed KYC with a unique government ID. If `credential-type` is `epassport`, this endpoint uses Holonym smart contracts to check whether the user has a unique NFC-enabled passport. If `credential-type` is `phone`, this endpoint uses Holonym smart contract to check whether the user has proven ownership of a unique phone number. If `credential-type` is `biometrics`, this endpoint uses Holonym smart contract to check whether the user has proven ownership of a unique (non-personally identifying) face.

See the following documentation [How to get user's proofs](https://holonym.gitbook.io/holonym-alpha/usage/how-to-stop-sybil-attacks-using-holonym#how-to-get-the-proof) for how to use action IDs.

*   Parameters

    | name              | description                         | type   | in    | required |
    | ----------------- | ----------------------------------- | ------ | ----- | -------- |
    | `credential-type` | 'gov-id' or 'phone' or 'biometrics' | string | path  | true     |
    | `network`         | 'optimism' or 'base-sepolia'        | string | path  | true     |
    | `user`            | User's blockchain address           | string | query | true     |
    | `action-id`       | Action ID                           | string | query | true     |
*   Example

    ```JavaScript
    const resp = await fetch('https://api.holonym.io/sybil-resistance/gov-id/optimism?user=0x0000000000000000000000000000000000000000&action-id=123456789');
    const { result: isUnique } = await resp.json();
    ```
* Responses
  *   200

      ```JSON
      {
          "result": true,
      }
      ```
  *   200

      Result if user has not submitted a valid proof.

      ```JSON
      {
          "result": false,
      }
      ```

#### **GET** `/residence/country/us/<network>?user=<user-address>`

Get whether the user resides in the US.

For the `/residence/country/<country-code>` endpoints, `<country-code>` will be a 2-letter country code following the [ISO 3166 standard](https://www.iso.org/iso-3166-country-codes.html). Holonym currently only supports queries for US residency.

*   Parameters

    | name      | description                     | type   | in    | required |
    | --------- | ------------------------------- | ------ | ----- | -------- |
    | `network` | 'optimism' or 'optimism-goerli' | string | path  | true     |
    | `user`    | User's blockchain address       | string | query | true     |
*   Example

    ```
    const resp = await fetch('https://api.holonym.io/residence/country/us/optimism?user=0x0000000000000000000000000000000000000000');
    const { result: isUSResident } = await resp.json();
    ```
* Responses
  *   200

      Result if user resides in the US.

      ```
      {
          "result": true,
      }
      ```
  *   200

      Result if user has not submitted a valid proof that they reside in the US.

      ```
      {
          "result": false,
      }
      ```

#### **GET** `/snapshot-strategies/residence/country/us?network=<network>&snapshot=<snapshot>&addresses=<addresses>`

Returns a list of scores indicating, for each address, whether the address has submitted a valid and unique proof of US residency.

Every score is either 1 or 0.

| score | description                           |
| ----- | ------------------------------------- |
| 1     | Address has proven US residency       |
| 0     | Address has _not_ proven US residency |



**Use with Snapshot**

To use with the ["api"](https://github.com/snapshot-labs/snapshot-strategies/tree/master/src/strategies/api) Snapshot strategy, specify the strategy parameters using the following format.

```
{
  "api": "https://api.holonym.io",
  "symbol": "",
  "decimals": 0,
  "strategy": "snapshot-strategies/residence/country/us"
}
```



**Use without Snapshot**

*   Parameters

    | name        | description                                    | type   | in    | required |
    | ----------- | ---------------------------------------------- | ------ | ----- | -------- |
    | `network`   | Chain ID                                       | string | query | true     |
    | `snapshot`  | Block height                                   | string | query | true     |
    | `addresses` | List of blockchain address separated by commas | string | query | true     |
*   Example

    ```
    const resp = await fetch('https://api.holonym.io/snapshot-strategies/residence/country/us?network=420&snapshot=9001&addresses=0x0000000000000000000000000000000000000000,0x0000000000000000000000000000000000000001');
    const data = await resp.json();
    ```
* Responses
  *   200

      ```
      {
        "score" : [
            {
              "address" : "0x0000000000000000000000000000000000000000",
              "score" : 0
            },
            {
              "address" : "0x0000000000000000000000000000000000000001",
              "score" : 1
            }
        ]
      }
      ```

#### **GET** `/snapshot-strategies/sybil-resistance/gov-id?network=<network>&snapshot=<snapshot>&addresses=<addresses>&action-id=<action-id>`

Returns a list of scores indicating, for each address, whether the address has submitted a valid proof of uniqueness for the given action-id.

Every score is either 1 or 0.

| score | description                                       |
| ----- | ------------------------------------------------- |
| 1     | Address has proven uniqueness for action-id       |
| 0     | Address has _not_ proven uniqueness for action-id |



**Use with Snapshot**

To use with the ["api"](https://github.com/snapshot-labs/snapshot-strategies/tree/master/src/strategies/api) Snapshot strategy, specify the strategy parameters using the following format. We highly recommend that projects use the default action-id `123456789` to avoid cases where users sell actions associated with action-ids that they do not care about.

```
{
  "api": "https://api.holonym.io",
  "symbol": "",
  "decimals": 0,
  "strategy": "snapshot-strategies/sybil-resistance/gov-id",
  "additionalParameters": "action-id=123456789"
}
```



**Use without Snapshot**

*   Parameters

    | name        | description                                    | type   | in    | required |
    | ----------- | ---------------------------------------------- | ------ | ----- | -------- |
    | `network`   | Chain ID                                       | string | query | true     |
    | `snapshot`  | Block height                                   | string | query | true     |
    | `addresses` | List of blockchain address separated by commas | string | query | true     |
*   Example

    ```
    const resp = await fetch('https://api.holonym.io/snapshot-strategies/sybil-resistance/gov-id?network=420&snapshot=9001&addresses=0x0000000000000000000000000000000000000000,0x0000000000000000000000000000000000000001&action-id=123');
    const data = await resp.json();
    ```
* Responses
  *   200

      ```
      {
        "score" : [
            {
              "address" : "0x0000000000000000000000000000000000000000",
              "score" : 0
            },
            {
              "address" : "0x0000000000000000000000000000000000000001",
              "score" : 1
            }
        ]
      }
      ```

#### **GET** `/snapshot-strategies/sybil-resistance/phone?network=<network>&snapshot=<snapshot>&addresses=<addresses>&action-id=<action-id>`

Returns a list of scores indicating, for each address, whether the address has submitted a valid proof of uniqueness (using phone number) for the given action-id.

Every score is either 1 or 0.

| score | description                                       |
| ----- | ------------------------------------------------- |
| 1     | Address has proven uniqueness for action-id       |
| 0     | Address has _not_ proven uniqueness for action-id |

**Use with Snapshot**

To use with the ["api"](https://github.com/snapshot-labs/snapshot-strategies/tree/master/src/strategies/api) Snapshot strategy, specify the strategy parameters using the following format. We suggest that you use the default action-id `123456789`. If you are using a different action-id, replace `123456789` with your action-id.

```
{
  "api": "https://api.holonym.io",
  "symbol": "",
  "decimals": 0,
  "strategy": "snapshot-strategies/sybil-resistance/phone",
  "additionalParameters": "action-id=123456789"
}
```

**Use without Snapshot**

*   Parameters

    | name        | description                                    | type   | in    | required |
    | ----------- | ---------------------------------------------- | ------ | ----- | -------- |
    | `network`   | Chain ID                                       | string | query | true     |
    | `snapshot`  | Block height                                   | string | query | true     |
    | `addresses` | List of blockchain address separated by commas | string | query | true     |
*   Example

    ```JavaScript
    const resp = await fetch('https://api.holonym.io/snapshot-strategies/sybil-resistance/phone?network=420&snapshot=9001&addresses=0x0000000000000000000000000000000000000000,0x0000000000000000000000000000000000000001&action-id=123');
    const data = await resp.json();
    ```
* Responses
  *   200

      ```JSON
      {
        "score" : [
            {
              "address" : "0x0000000000000000000000000000000000000000",
              "score" : 0
            },
            {
              "address" : "0x0000000000000000000000000000000000000001",
              "score" : 1
            }
        ]
      }
      ```

#### **GET** `/snapshot-strategies/sybil-resistance/biometrics?network=<network>&snapshot=<snapshot>&addresses=<addresses>&action-id=<action-id>`

Returns a list of scores indicating, for each address, whether the address has submitted a valid proof of uniqueness (using non-personally identifying face vectors) for the given action-id.

Every score is either 1 or 0.

| score | description                                       |
| ----- | ------------------------------------------------- |
| 1     | Address has proven uniqueness for action-id       |
| 0     | Address has _not_ proven uniqueness for action-id |

**Use with Snapshot**

To use with the ["api"](https://github.com/snapshot-labs/snapshot-strategies/tree/master/src/strategies/api) Snapshot strategy, specify the strategy parameters using the following format. We suggest that you use the default action-id `123456789`. If you are using a different action-id, replace `123456789` with your action-id.

```
{
  "api": "https://api.holonym.io",
  "symbol": "",
  "decimals": 0,
  "strategy": "snapshot-strategies/sybil-resistance/biometrics",
  "additionalParameters": "action-id=123456789"
}
```

**Use without Snapshot**

*   Parameters

    | name        | description                                    | type   | in    | required |
    | ----------- | ---------------------------------------------- | ------ | ----- | -------- |
    | `network`   | Chain ID                                       | string | query | true     |
    | `snapshot`  | Block height                                   | string | query | true     |
    | `addresses` | List of blockchain address separated by commas | string | query | true     |
*   Example

    ```JavaScript
    const resp = await fetch('https://api.holonym.io/snapshot-strategies/sybil-resistance/biometrics?network=420&snapshot=9001&addresses=0x0000000000000000000000000000000000000000,0x0000000000000000000000000000000000000001&action-id=123');
    const data = await resp.json();
    ```
* Responses
  *   200

      ```JSON
      {
        "score" : [
            {
              "address" : "0x0000000000000000000000000000000000000000",
              "score" : 0
            },
            {
              "address" : "0x0000000000000000000000000000000000000001",
              "score" : 1
            }
        ]
      }
      ```
