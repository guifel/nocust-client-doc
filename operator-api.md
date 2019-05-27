# Operator API

## Websockets

### `sync/wallets/`

#### Inputs

* subscribe to multi-wallet events:

  ```javascript
  { 
   "action": "subscribe", 
   "wallets": ["<wallet address without 0x prefix>"] 
  }
  ```

#### Outputs

* on subscribe action: 

  ```javascript
  { 
   "status": "success", 
   "message": "subscription group updated successfully" 
  }
  ```

* on incoming transfer:

  ```javascript
  { 
   "status": "notify", 
   "message": { 
       "type": "incoming_transfer", 
       "topic": "<wallet address without 0x prefix>",
       "data": "<tx object>" 
   } 
  }
  ```

* on incoming receipt:

  ```javascript
  { 
   "status": "notify", 
   "message": { 
       "type": "incoming_receipt", 
       "topic": "<wallet address without 0x prefix>",
       "data": "<tx object>" 
   } 
  }
  ```

* on incoming transfer confirmation:

  ```javascript
  { 
   "status": "notify", 
   "message": { 
       "type": "incoming_confirmation", 
       "topic": "<wallet address without 0x prefix>",
       "data": "<tx object>" 
   } 
  }
  ```

* on transfer timeout:

  ```javascript
  { 
   "status": "notify", 
   "message": { 
       "type": "timeout_transfer", 
       "topic": "<wallet address without 0x prefix>",
       "data": "<tx object>" 
   } 
  }
  ```

* on swap match:

  ```javascript
  { 
   "status": "notify", 
   "message": { 
       "type": "matched_swap", 
       "topic": "<wallet address without 0x prefix>",
       "data": "<tx object>" 
   } 
  }
  ```

* on swap finalization:

  ```javascript
  { 
   "status": "notify", 
   "message": { 
       "type": "finalized_swap", 
       "topic": "<wallet address without 0x prefix>",
       "data": "<tx object>" 
   } 
  }
  ```

* on wallet admission:

  ```javascript
  { 
   "status": "notify", 
   "message": { 
       "type": "registered_wallet", 
       "topic": "<wallet address without 0x prefix>",
       "data": { "address": "<wallet address>", "token": "<token address>" }
   } 
  }
  ```

* on deposit confirmation:

  ```javascript
  { 
   "status": "notify", 
   "message": { 
       "type": "confirmed_deposit", 
       "topic": "<wallet address without 0x prefix>",
       "data": { 
           "txid": "<ethereum tx id>", 
           "block": "<ethereum block number>", 
           "eon_number": "<commit-chain eon number>", 
           "amount": "<deposit amount>", 
           "time": "<deposit time>"
       }
   } 
  }
  ```

* on withdrawal request:

  ```javascript
  { 
   "status": "notify", 
   "message": { 
       "type": "requested_withdrawal", 
       "topic": "<wallet address without 0x prefix>",
       "data": {
           "txid": "<ethereum tx id>", 
           "block": "<ethereum block number>", 
           "eon_number": "<commit-chain eon number>", 
           "amount": "<withdrawal request amount>", 
           "time": "<withdrawal request time>",
           "slashed": "<whether withdrawal request was slashed or not, this event will usually return slashed == false>"
       } 
   } 
  }
  ```

* on withdrawal confirmation:

  ```javascript
  { 
   "status": "notify", 
   "message": { 
       "type": "confirmed_withdrawal",
       "topic": "<wallet address without 0x prefix>", 
       "data": {
           "txid": "<ethereum tx id>", 
           "block": "<ethereum block number>", 
           "eon_number": "<commit-chain eon number>", 
           "amount": "<withdrawal request amount>", 
           "time": "<withdrawal request time>",
           "request": "<withdrawal request object>",
       } 
   } 
  }
  ```

### `sync/swaps/`

#### Inputs

* subscribe to token-pair events:

  ```javascript
  { 
   "action": "subscribe", 
   "swaps": ["<sell token address without 0x prefix><buy token address withour 0x prefix>"] 
  }
  ```

#### Outputs

* on subscribe action: 

  ```javascript
  { 
   "status": "success", 
   "message": "subscription group updated successfully" 
  }
  ```

* on incoming swap:

  ```javascript
  { 
   "status": "notify", 
   "message": { 
       "type": "incoming_swap", 
       "topic": "<sell token address without 0x prefix><buy token address withour 0x prefix>",
       "data": "<tx object>" 
   } 
  }
  ```

> **Notes** to override the subscription list, simply send out another subscribe action message with the new list.

## HTTP

### Admission

Defines the endpoints and methods for registration.

* Getting registered wallets

  ```http
  GET admission/?[format=json]
  ```

* Registering new wallet

  ```http
  POST admission/
  body: {
  "authorization": "<commit-chain_authorisation>",
  "address": "<wallet address without 0x prefix>",
  "token": "<contract address without 0x prefix>"
  }
  ```

### Auditor

Defines the endpoints for data retrieval by clients.

* Commit-chain status overview

  ```http
  GET audit/?[format=json]
  ```

* List of commit-chain tokens

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

### Transactor

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

### Swapper

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
  ```

