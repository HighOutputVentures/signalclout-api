# Introduction
This project is a work in progress. API keys which provides higher throughput will be distributed soon. For more information, please contact roger@hov.co.

This GraphQL API is compliant to the [Relay GraphQL Server Specification](https://relay.dev/docs/guides/graphql-server-specification/).

The GraphQL schema and documentation can be found at https://api.signalclout.com/graphql.

# Search profiles by keyword
Profiles can be searched given one or more keywords. These keywords are matched against the username, public key and profile description.
### Query
```graphql
query (first: Int, $search: [String!]) {
  profiles(first: $first, search: $search) {
    totalCount
    edges {
      node {
        id
        publicKey
        username
        image
        description
        hidden
        verified
        reserved
        creatorBasisPoints
        balanceNanos
        bitcloutLockedNanos
        coinsInCirculationNanos
        coinPriceBitCloutNanos
        coinPriceUSD
        walletPriceUSD
        followersCount
        followingCount
      }
      cursor
    }
  }
}
```
### Variables
```json
{
  "first": 10,
  "search": ["entrepreneur"]
}
```
### Example cURL request
```bash
curl --location --request POST 'https://api.signalclout.com/graphql' \
--header 'Content-Type: application/json' \
--data-raw '{"query":"query ($first: Int, $search: [String!]) {\n  profiles(first: $first, search: $search) {\n    totalCount\n    edges {\n      node {\n        id\n        publicKey\n        username\n        image\n        description\n        hidden\n        verified\n        reserved\n        creatorBasisPoints\n        balanceNanos\n        bitcloutLockedNanos\n        coinsInCirculationNanos\n        coinPriceBitCloutNanos\n        coinPriceUSD\n        walletPriceUSD\n        followersCount\n        followingCount\n      }\n      cursor\n    }\n  }\n}","variables":{"first":10,"search":["entrepreneur"]}}'
```
# Filter profiles
Profiles may be filtered in many different ways. To retrieve profiles with coin prices less than $50, we use the `coinPriceBitCloutNanos` filter. Given the BitClout price is $170.40, the corresponding `coinPriceBitCloutNanos` is `293427230`.
### Query
```graphql
query ($first: Int, $filter: ProfileFilterInput) {
  profiles(first: $first, filter: $filter) {
    totalCount
    edges {
      node {
        id
        publicKey
        username
        image
        description
        hidden
        verified
        reserved
        creatorBasisPoints
        balanceNanos
        bitcloutLockedNanos
        coinsInCirculationNanos
        coinPriceBitCloutNanos
        coinPriceUSD
        walletPriceUSD
        followersCount
        followingCount
      }
      cursor
    }
  }
}
```
### Variables
```json
{
    "first": 10,
    "filter": {
        "coinPriceBitCloutNanos": {
            "lt": 293427230
        }
    }
}
```
### Example cURL request
```bash
curl --location --request POST 'https://api.signalclout.com/graphql' \
--header 'Content-Type: application/json' \
--data-raw '{"query":"query ($first: Int, $filter: ProfileFilterInput) {\n  profiles(first: $first, filter: $filter) {\n    totalCount\n    edges {\n      node {\n        id\n        publicKey\n        username\n        image\n        description\n        hidden\n        verified\n        reserved\n        creatorBasisPoints\n        balanceNanos\n        bitcloutLockedNanos\n        coinsInCirculationNanos\n        coinPriceBitCloutNanos\n        coinPriceUSD\n        walletPriceUSD\n        followersCount\n        followingCount\n      }\n      cursor\n    }\n  }\n}","variables":{"first":10,"filter":{"coinPriceBitCloutNanos":{"lt":293427230}}}}'
```
# Retrieve profile
### Query
```graphql
query ($id: Binary!) {
  node(id: $id) {
    id
    ... on Profile {
      publicKey
      username
      image
      description
      hidden
      verified
      reserved
      creatorBasisPoints
      balanceNanos
      bitcloutLockedNanos
      coinsInCirculationNanos
      coinPriceBitCloutNanos
      coinPriceUSD
      walletPriceUSD
      followersCount
      followingCount
    }
  }
}
```
### Variables
```json
{
  "id": "AOtWE6YXob6O5L1yYun4dg=="
}
```
### Example cURL request
```bash
curl --location --request POST 'https://api.signalclout.com/graphql' \
--header 'Content-Type: application/json' \
--data-raw '{"query":"query ($id: Binary!) {\n    node(id: $id) {\n        id\n        ... on Profile {\n            publicKey\n            username\n            image\n            description\n            hidden\n            verified\n            reserved\n            creatorBasisPoints\n            balanceNanos\n            bitcloutLockedNanos\n            coinsInCirculationNanos\n            coinPriceBitCloutNanos\n            coinPriceUSD\n            walletPriceUSD\n            followersCount\n            followingCount\n        }\n    }\n}","variables":{"id":"AOtWE6YXob6O5L1yYun4dg=="}}'
```
# Retrieve profile by public key
### Query
```graphql
query ($publicKey: String!) {
  profile(publicKey: $publicKey) {
    id
    publicKey
    username
    image
    description
    hidden
    verified
    reserved
    creatorBasisPoints
    balanceNanos
    bitcloutLockedNanos
    coinsInCirculationNanos
    coinPriceBitCloutNanos
    coinPriceUSD
    walletPriceUSD
    followersCount
    followingCount
  }
}
```
### Variables
```json
{
  "publicKey": "BC1YLj4iUraQabhUBT8kS1gCieXhD1GGXeFCcU3qM3MGgf7GwBJ9Va7"
}
```
### Example cURL request
```bash
curl --location --request POST 'https://api.signalclout.com/graphql' \
--header 'Content-Type: application/json' \
--data-raw '{"query":"query ($publicKey: String!) {\n    profile(publicKey: $publicKey) {\n        id\n        publicKey\n        username\n        image\n        description\n        hidden\n        verified\n        reserved\n        creatorBasisPoints\n        balanceNanos\n        bitcloutLockedNanos\n        coinsInCirculationNanos\n        coinPriceBitCloutNanos\n        coinPriceUSD\n        walletPriceUSD\n        followersCount\n        followingCount\n    }\n}","variables":{"publicKey":"BC1YLj4iUraQabhUBT8kS1gCieXhD1GGXeFCcU3qM3MGgf7GwBJ9Va7"}}'
```
# Retrieve profile transfers
### Query
```graphql
query ($id: Binary!) {
  node(id: $id) { 
    id
    ... on Profile {
      transfers {
        totalCount
        totalDepositsUSD
        totalWithdrawalsUSD
        edges {
          node {
            id
            type
            sender {
              id
              username
              publicKey
            }
            receiver {
              id
              username
              publicKey
            }
            amountUSD
            satoshisBurned
            btcSpendAddress
            btcTransactionHash
            dateTimeCreated
          }
        }
      }
    }
  }
}
```
### Variables
```json
{
  "id": "AOtWE6YXob6O5L1yYun4dg=="
}
```
### Example cURL request
```bash
curl --location --request POST 'https://api.signalclout.com/graphql' \
--header 'Content-Type: application/json' \
--data-raw '{"query":"query ($id: Binary!) {\n    node(id: $id) { \n        id\n        ... on Profile {\n            transfers {\n                totalCount\n                totalDepositsUSD\n                totalWithdrawalsUSD\n                edges {\n                    node {\n                        id\n                        type\n                        sender {\n                            id\n                            username\n                            publicKey\n                        }\n                        receiver {\n                            id\n                            username\n                            publicKey\n                        }\n                        amountUSD\n                        satoshisBurned\n                        btcSpendAddress\n                        btcTransactionHash\n                        dateTimeCreated\n                    }\n                }\n            }\n        }\n    }\n}","variables":{"id":"AOtWE6YXob6O5L1yYun4dg=="}}'
```
# FAQs
## How do I get the founder reward percentage?
`Founder Reward Percentage = Profile.creatorBasisPoints / 100`
