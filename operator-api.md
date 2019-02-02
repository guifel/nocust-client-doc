# Operator API

The operator provides an API for wallets, clients, explorers and others to see registered wallets, register new wallets, audit the data from the operator and to perform swaps.


## Websockets

## `sync/wallets/`

### Inputs
 * subscribe to multi-wallet events:
  ```json
{ 
    "action": "subscribe", 
    "wallets": ["<wallet address without 0x prefix>"] 
}
  ```

### Outputs
 * on subscribe action: 
  ```json
{ 
    "status": "success", 
    "message": "subscription group updated successfully" 
}
  ```
 * on incoming transfer:
  ```json
{ 
    "status": "notify", 
    "message": { 
        "type": "incoming_transfer", 
        "data": "<tx object>" 
    } 
}
  ```
 * on incoming receipt:
  ```json
{ 
    "status": "notify", 
    "message": { 
        "type": "incoming_receipt", 
        "data": "<tx object>" 
    } 
}
  ```
 * on incoming hub confirmation:
  ```json
{ 
    "status": "notify", 
    "message": { 
        "type": "incoming_confirmation", 
        "data": "<tx object>" 
    } 
}
  ```
 * on transfer timeout:
  ```json
{ 
    "status": "notify", 
    "message": { 
        "type": "timeout_transfer", 
        "data": "<tx object>" 
    } 
}
  ```

> **Notes**
> to override the subscription list, simply send out another subscribe action message with the new list. 

***

# HTTP
## Admission

Defines the endpoints and methods for registration.

* Getting registered wallets
```http
GET admission/?[format=json]
```
* Registering new wallet
```http
POST admission/
body: {
  "authorization": "<hub_authorisation>",
  "address": "<wallet address without 0x prefix>",
  "token": "<contract address without 0x prefix>"
}
```

## Auditor

Defines the endpoints for data retrieval by clients.

* Hub status overview
```http
GET audit/?[format=json]
```
* List of hub tokens
```http
GET audit/tokens/?[format=json]
```
* Wallet synchronization data
```http
GET audit/<token>/<wallet>/?[format=json]
```
* Wallet registration data
```http
GET audit/<token>/<wallet>/whois?[format=json]
```
* List buy and sell order for a token pair `(left token, right token)`
```http
GET audit/swaps/<left token address>/<right token address>?[format=json]
```

## Transactor

Defines the endpoints for making transfers.


* Transfer submission
```http
PUT transfer/
body: {
  "wallet": {
    "address": "<wallet address without 0x prefix>",
    "token": "<contract address without 0x prefix>"
  },
  "amount": <decimal>,
  "round": <round the transfer is performed>,
  "recipient": "<recipient address without 0x prefix>",
  "nonce": <nonce of the transfer | decimal>,
  "wallet_signature": {
    "value": "wallet_signature"
  },
  "wallet_balance_signature": {
    "value": "wallet_balance_signature"
  },
  
  ["id": <transfer id>,]
  ["time": "<datetime>",]
  ["sender_aggregate": {
    "wallet_signature": "<wallet_signature>",
    "hub_signature": "<hub_signature>",
    "updated_spendings": "<updated_spendings>",
    "updated_gains": "<updated_gains>",
    "tx_set_hash": "<tx_set_hash>",
    "tx_set_proof": [<tx_proof>, <tx_proof>, ...],
    "tx_set_index": <decimal>
  },]
  ["recipient_aggregate": {
    "wallet_signature": "<wallet_signature>",
    "hub_signature": "<hub_signature>",
    "updated_spendings": "<updated_spendings>",
    "updated_gains": "<updated_gains>",
    "tx_set_hash": "<tx_set_hash>",
    "tx_set_proof": [<tx_proof>, <tx_proof>, ...],
    "tx_set_index": <decimal>
  },]
  ["processed": <has the transfer been processed>,]
  ["complete": <is the transfer complete>,]
  ["final_receipt_hashes": "<final_receipt_hashes>",]
  ["final_receipt_index": "<integer>",]
}
```
* Receipt
```http
GET transfer/<id>/[?format=json]
```
* Receipt submission
```http
POST transfer/<id>
body: {
  "wallet_signature": {
    "value": "wallet_signature"
  },
  
  ["wallet": {
    "address": "<wallet address without 0x prefix>",
    "token": "<contract address without 0x prefix>"
  },]
  ["recipient": "<recipient address without 0x prefix>",]
  ["time": "<datetime>",]
  ["sender_aggregate": {
    "wallet_signature": "<wallet_signature>",
    "hub_signature": "<hub_signature>",
    "updated_spendings": "<updated_spendings>",
    "updated_gains": "<updated_gains>",
    "tx_set_hash": "<tx_set_hash>",
    "tx_set_proof": [<tx_proof>, <tx_proof>, ...],
    "tx_set_index": <decimal>
  },]
  ["recipient_aggregate": {
    "wallet_signature": "<wallet_signature>",
    "hub_signature": "<hub_signature>",
    "updated_spendings": "<updated_spendings>",
    "updated_gains": "<updated_gains>",
    "tx_set_hash": "<tx_set_hash>",
    "tx_set_proof": [<tx_proof>, <tx_proof>, ...],
    "tx_set_index": <decimal>
  },]
  ["processed": <has the transfer been processed>,]
  ["complete": <is the transfer complete>,]
  ["amount": <decimal>,]
  ["nonce": <nonce of the transfer | decimal>,]
}
```
* Cancel a transfer
```http
XXX transfer/<id>/cancel
body: {
  TODO: "to be documented"
}
```

## Swapper

Defines the endpoints for making swaps, and contains the matching engine.

* Swap submission
```http
PUT swap/
body: {
  "wallet": {
    "address": "<wallet address without 0x prefix>",
    "token": "<contract address without 0x prefix>"
  },
  "amount": <decimal>,
  "round": <round the transfer is performed>,
  "recipient": {
    "address": "<wallet address without 0x prefix>",
    "token": "<contract address without 0x prefix>"
  },
  "nonce": <nonce of the transfer | decimal>,
  "debit_signature": {
    "value": "<debit_signature>"
  },
  "debit_balance_signature": {
    "value": "<debit_balance_signature>"
  },
  "credit_signature": {
    "value": "<credit_signature>"
  },
  "fulfillment_signature": {
    "value": "<fulfillment_signature>"
  },
  
  ["id": <transfer id>,]
  ["time": "<datetime>",]
  ["sender_aggregate": {
    "wallet_signature": "<wallet_signature>",
    "hub_signature": "<hub_signature>",
    "updated_spendings": "<updated_spendings>",
    "updated_gains": "<updated_gains>",
    "tx_set_hash": "<tx_set_hash>",
    "tx_set_proof": [<tx_proof>, <tx_proof>, ...],
    "tx_set_index": <decimal>
  },]
  ["recipient_aggregate": {
    "wallet_signature": "<wallet_signature>",
    "hub_signature": "<hub_signature>",
    "updated_spendings": "<updated_spendings>",
    "updated_gains": "<updated_gains>",
    "tx_set_hash": "<tx_set_hash>",
    "tx_set_proof": [<tx_proof>, <tx_proof>, ...],
    "tx_set_index": <decimal>
  },]
  ["processed": <has the transfer been processed>,]
  ["complete": <is the transfer complete>,]
  ["final_receipt_hashes": "<final_receipt_hashes>",]
  ["final_receipt_index": "<integer>",]
  ["amount_swapped": "<decimal>",]
}
```
* Swap finalization
```http
PUT swap/<id>/finalize
body: {
  "finalization_signature": {
    "value": "<finalization_signature>"
  }
}
```
* Swap freeze
```http
PUT swap/<id>/freeze
body: {
  "freezing_signature": {
    "value": "<freezing_signature>"
  }
}
```
* Swap cancellation
```http
PUT swap/<id>/cancel
body: {
  "sender_cancellation_signature": {
    "value": "<sender_cancellation_signature>"
  },
  "recipient_cancellation_signature": {
    "value": "<recipient_cancellation_signature>"
  }
}
​```s
```