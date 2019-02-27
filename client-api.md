<a id="nocusttransfer"></a>
# NocustTransfer

Interface For the parameters object required to make nocust transfers.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| to | `string` | Recipient of the transfer.|
| from | `string` | Sender of the transfer. |
|`Optional` tokenAddress | `string` | Token to send. Use Ether if not specified. |
| Amount | `BigNumber` \| `BN` \| `string` | Amount in wei (for Ether) of the transfer |
| `Optional` nonce| `BigNumber` |Specify a nonce, can be used to attribute an identifier to the transaction. Set randomly if not specified. |

# TransferDataInterface

Full transfer details including signatures and status.

| Name | Type | Description |
| :--- | :--- | ---: |
| id | number | Transaction ID, uniquely identify the transfer |
| amount | string | Transfer amount |
| wallet | TransferWalletDataInterface | Sender details. |
| recipient | TransferWalletDataInterface | Recipient details. |
| round | number | NOCUST hub round of the transfer. |
| processed | boolean | If the payment hub processed the the transfer. The payment hub wait for the recipient confirmation before processing the transfer. |
| completed | boolean | If the transfer is fully confirmed. |
| canceled | boolean | If the transfer is canceled. |

# NocustManager

<a id="approveanddeposit"></a>

##  approveAndDeposit

▸ **approveAndDeposit**(address: *`string`*, amount: *`BigNumber` \| `BN` \| `string`*, gasPrice: *`BigNumber` \| `BN` \| `string`*, gas: *`number`*, tokenAddress?: *`string`*): `Promise`<`string`>

Similarly to the `deposit` function but prior check to send the deposit transaction it checks for token transfer allowance of the NOCUST smart contract. If the allowance is not sufficient it sends an ERC-20 `aprove` transaction. Approving token transfers is a required operation by the ERC-20 standard. If the depositor does not have sufficient allowance the deposit will fail. Note that this function will make 2 on-chain transaction (contract calls) with the specified gas price and gas limit. Approvals are not required for Ether.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Targeted Ethereum address |
| amount | `BigNumber` \| `BN` \| `string` |  Amount to deposit, this amount needs to be available on-chain for the specified address. |
| gasPrice | `BigNumber` \| `BN` \| `string` |  Gas price for the on-chain transaction. |
| gas | `number` |  Gas limit for the on-Chain transaction. Around 100'000 gas is advised. |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`string`>
Hash of the on-chain transaction of the deposit.

___
<a id="buysla"></a>

##  buySLA

▸ **buySLA**(address: *`string`*): `Promise`<`void`>

Buys a SLA. The address needs to have the token amount available as a nocust balance on the address as specified by the `getSLADetail` function. This function will make a nocust transfer to pay for the SLA.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  \- |

**Returns:** `Promise`<`void`>

___
<a id="deposit"></a>

##  deposit

▸ **deposit**(address: *`string`*, amount: *`BigNumber` \| `BN` \| `string`*, gasPrice: *`BigNumber` \| `BN` \| `string`*, gas: *`number`*, tokenAddress?: *`string`*): `Promise`<`string`>

Make an on-chain transaction to deposit funds into the NOCUST hub. This funds can be later used for making off-chain NOCUST transfers.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Targeted Ethereum address |
| amount | `BigNumber` \| `BN` \| `string` |  Amount to deposit, this amount needs to be available on-chain for the specified address. |
| gasPrice | `BigNumber` \| `BN` \| `string` |  Gas price for the on-chain transaction. |
| gas | `number` |  Gas limit for the on-Chain transaction. Around 100'000 gas is advised. |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`string`>
Hash of the on-chain transaction of the deposit.

___
<a id="finalizeswap"></a>

##  finalizeSwap

▸ **finalizeSwap**(address: *`string`*, sellTokenAddress: *`string`*, txId: *`number`*): `Promise`<`void`>

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  \- |
| sellTokenAddress | `string` |  \- |
| txId | `number` |  \- |

**Returns:** `Promise`<`void`>

___
<a id="getblockpereon"></a>

##  getBlockPerEon

▸ **getBlockPerEon**(): `Promise`<`number`>

Get the number of Eras (blocks) per Eon.

**Returns:** `Promise`<`number`>

___
<a id="getblockstowithdrawalconfirmation"></a>

##  getBlocksToWithdrawalConfirmation

▸ **getBlocksToWithdrawalConfirmation**(address: *`string`*, txHash?: *`string`*, tokenAddress?: *`string`*): `Promise`<`number`>

Get the number of blocks until it is possible to send the withdrawal confirmation on-chain transaction. It will return -1 if no withdrawals are pending or 0 if the withdrawal is ready to be confirmed.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Address that started the withdrawal. |
| `Optional` txHash | `string` |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`number`>

___
<a id="geteonnumber"></a>

##  getEonNumber

▸ **getEonNumber**(): `Promise`<`number`>

Get the current Eon or round number.

**Returns:** `Promise`<`number`>

___
<a id="geteranumber"></a>

##  getEraNumber

▸ **getEraNumber**(): `Promise`<`number`>

Get the current Era or sub-block number. The number of block since the current Eon started.

**Returns:** `Promise`<`number`>
Era

___
<a id="getnocustbalance"></a>

##  getNocustBalance

▸ **getNocustBalance**(address: *`string`*, tokenAddress?: *`string`*): `Promise`<`BigNumber`>

Fetch the current NOCUST balance.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Targeted Ethereum address. |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`BigNumber`>
Nocust balance

___
<a id="getonchainbalance"></a>

##  getOnChainBalance

▸ **getOnChainBalance**(address: *`string`*, tokenAddress?: *`string`*): `Promise`<`BigNumber`>

Get current on-chain balance.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Targeted Ethereum address. |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`BigNumber`>
On-chain balance.

___
<a id="getorderbook"></a>

##  getOrderBook

▸ **getOrderBook**(buyTokenAddress: *`string`*, sellTokenAddress: *`string`*): `Promise`<`OrderBookDataInterface`>

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| buyTokenAddress | `string` |  \- |
| sellTokenAddress | `string` |  \- |

**Returns:** `Promise`<OrderBookDataInterface>

___
<a id="getsladetail"></a>

##  getSLADetail

▸ **getSLADetail**(): `Promise`<`SLADetailsInterface`>

Get informations about the SLA pricing model.

**Returns:** `Promise`<`SLADetailsInterface`>
Object with the token address with which to pay the SLA, the cost/amount of the SLA in this token, the recipient of the SLA payment, the transaction limit per month without SLA.

___
<a id="getslastatus"></a>

##  getSLAStatus

▸ **getSLAStatus**(address: *`string`*): `Promise`<`number`>

Get the SLA status for a given address.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Targeted Ethereum address. |

**Returns:** `Promise`<`number`>
0 if not under SLA, expiry time if currently enroll with a SLA.

___
<a id="getsupportedtokens"></a>

##  getSupportedTokens

▸ **getSupportedTokens**(): `Promise`<`object`[]>

Fetch the smart contract addresses of the supported token by the hub.

**Returns:** `Promise`<`object`[]>
Promise that resolves with an array of objects `{ address: string, name: string, short_name: string }` for each token supported by the hub. Address is the address of the ERC-20 contract of the token.

___
<a id="gettransaction"></a>

##  getTransaction

▸ **getTransaction**(transactionId: *`number`*): `Promise`<`TransferDataInterface`>

Fetch the transaction details given a transaction ID

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| transactionId | `number` |  Transaction ID of the transaction to fetch |

**Returns:** `Promise`<`TransferDataInterface`>
Transaction details
}`

___
<a id="gettransactionsforaddress"></a>

##  getTransactionsForAddress

▸ **getTransactionsForAddress**(address: *`string`*, tokenAddress?: *`string`*, roundNumber?: *`number`*): `Promise`<`TransferDataInterface`>

Get the list of transactions for a given address and token (Incoming and outgoing).

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Targeted Ethereum address. |
| `Optional` tokenAddress | `string` |
| `Optional` roundNumber | `number` |

**Returns:** `Promise`<`TransferDataInterface`>
Array of transactions

___
<a id="getwalletstate"></a>

##  getWalletState

▸ **getWalletState**(address: *`string`*, tokenAddress?: *`string`*): `Promise`<`WalletState`>

Get the `WalletState` object for lower level API use.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Targeted Ethereum address. |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`WalletState`>

___
<a id="getwithdrawalfee"></a>

##  getWithdrawalFee

▸ **getWithdrawalFee**(withdrawalRequestGasPrice: *`BigNumber` \| `BN` \| `string`*): `Promise`<`BigNumber`>

Doing a withdrawal request involve a fee to the operator. This function gets the current fee.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| withdrawalRequestGasPrice | `BigNumber` \| `BN` \| `string` |  Gas price that will be used for the withdrawal request. The Fee is dependant on the gas price to be used. |

**Returns:** `Promise`<`BigNumber`>
Fee amount

___
<a id="getwithdrawallimit"></a>

##  getWithdrawalLimit

▸ **getWithdrawalLimit**(address: *`string`*, tokenAddress?: *`string`*): `Promise`<`BigNumber`>

Withdrawals are limited to a certain amount (Because funds need to committed into a check point) and the limit increase over time. This method gets you the current available off-chain funds for withdrawal.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Targeted Ethereum address. |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`BigNumber`>
Withdrawal limit.

___
<a id="incomingtransactionobservable"></a>

##  incomingTransactionObservable

▸ **incomingTransactionObservable**(address: *`string`*, tokenAddress?: *`string`*): `Observable`<`TransferDataInterface`>

Return an observable to notify of incoming transfers.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Address to watch for. |
| `Optional` tokenAddress | `string` |

**Returns:** `Observable`<`TransferDataInterface`>
Observable of incoming transfers

___
<a id="isaddressregistered"></a>

##  isAddressRegistered

▸ **isAddressRegistered**(address: *`string`*, tokenAddress?: *`string`*): `Promise`<`boolean`>

Check if an address is registered with the nocust hub.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Address to check. |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`boolean`>
Return `true` if the registration is successful

___
<a id="isaddressregisteredwithhub"></a>

##  isAddressRegisteredWithHub

▸ **isAddressRegisteredWithHub**(address: *`string`*, tokenAddress?: *`string`*): `Promise`<`boolean`>

Check whether an address is registered with the payment hub and can therefore receive payments.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Targeted Ethereum address. |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`boolean`>

___
<a id="isrecovery"></a>

##  isRecovery

▸ **isRecovery**(): `Promise`<`boolean`>

Check whether the the hub is in recovery mode and therfore cna't be used anymore.

**Returns:** `Promise`<`boolean`>
Recovery status.

___
<a id="issuedeliverychallenge"></a>

##  issueDeliveryChallenge

▸ **issueDeliveryChallenge**(address: *`string`*, txId: *`number`*, gasPrice: *`any`*, gas: *`any`*, tokenAddress?: *`string`*): `Promise`<`string`>

Make an on-chain transaction to initiate a delivery challenge.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Targeted Ethereum address. |
| txId | `number` |  Transaction id to challenge for delivery. |
| gasPrice | `any` |  Gas price for the on-chain transaction. |
| gas | `any` |  Gas limit for the off-Chain transaction. Around 350'000 gas is advised. |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`string`>

___
<a id="issuestateupdatechallenge"></a>

##  issueStateUpdateChallenge

▸ **issueStateUpdateChallenge**(address: *`string`*, gasPrice: *`any`*, gas: *`any`*, tokenAddress?: *`string`*): `Promise`<`string`>

Make an on-chain transaction to initiate a state update challenge.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Targeted Ethereum address. |
| gasPrice | `any` |  Gas price for the on-chain transaction. |
| gas | `any` |  Gas limit for the off-Chain transaction. Around 350'000 gas is advised. |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`string`>

___
<a id="recoverfunds"></a>

##  recoverFunds

▸ **recoverFunds**(address: *`string`*, gasPrice: *`any`*, gas: *`any`*, tokenAddress?: *`string`*): `Promise`<`string`>

Make an on-chain transaction to recover funds whenever the hub falls into recovery mode.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Targeted Ethereum address. |
| gasPrice | `any` |  Gas price for the on-chain transaction. |
| gas | `any` |  Gas limit for the off-Chain transaction. Around 350'000 gas is advised. |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`string`>

___
<a id="registeraddress"></a>

##  registerAddress

▸ **registerAddress**(address: *`string`*, tokenAddress?: *`string`*): `Promise`<`void`>

Register an address for a given token with the payment hub. An address needs to be registered with the payment hub for each token separately in order to send or receive transfers. This operation is done implicitly when sending a transfer if the address is not yet registered. Note that for a transfer to succeed the recipient need to have registered.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Address to register with the payment hub . |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`void`>

___
<a id="sendswap"></a>

##  sendSwap

▸ **sendSwap**(address: *`string`*, buyTokenAddress: *`string`*, sellTokenAddress: *`string`*, buyAmount: *`BigNumber` \| `BN` \| `string`*, sellAmount: *`BigNumber` \| `BN` \| `string`*): `Promise`<`number`>

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  \- |
| buyTokenAddress | `string` |  \- |
| sellTokenAddress | `string` |  \- |
| buyAmount | `BigNumber` \| `BN` \| `string` |  \- |
| sellAmount | `BigNumber` \| `BN` \| `string` |  \- |

**Returns:** `Promise`<`number`>

___
<a id="sendtransaction"></a>

##  sendTransaction

▸ **sendTransaction**(transfer: *NocustTransfer*): `Promise`<`number`>

Send a NOCUST transfer.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| transfer | NocustTransfer |  Parameter object of an off-chain transfer. |

**Returns:** `Promise`<`number`>
Transaction Id of the NOCUST transfer

___
<a id="syncwallet"></a>

##  syncWallet

▸ **syncWallet**(address: *`string`*, tokenAddress?: *`string`*): `Promise`<`void`>

Force synchronize the wallet of the given public key for the given token. The result will be cached making later calls of this wallet faster. This function can be useful to pre-cache required internal data (for example on starup of your app) to make later function calls faster.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Targeted Ethereum address |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`void`>

___
<a id="withdrawalconfirmation"></a>

##  withdrawalConfirmation

▸ **withdrawalConfirmation**(address: *`string`*, gasPrice: *`BigNumber` \| `BN` \| `string`*, gas: *`number`*, tokenAddress?: *`string`*): `Promise`<`string`>

Make an on-chain transaction to confirm a withdrawal previously initialized and effectively transfer the funds.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Targeted Ethereum address. |
| gasPrice | `BigNumber` \| `BN` \| `string` |  Gas price for the on-chain transaction. |
| gas | `number` |  Gas limit for the on-Chain transaction. Around 100'000 gas is advised. |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`string`>
Hash of the on-chain transaction of the withdrawal confirmation.

___
<a id="withdrawalrequest"></a>

##  withdrawalRequest

▸ **withdrawalRequest**(address: *`string`*, amount: *`BigNumber` \| `BN` \| `string`*, gasPrice: *`BigNumber` \| `BN` \| `string`*, gas: *`number`*, tokenAddress?: *`string`*): `Promise`<`string`>

Make an on-chain transaction to initiate a withdrawal to remove the funds from the NOCUST smart contract and to get them at the specified address. The amount specified needs to be available for withdrawal . The withdrawal will have to be confirmed, after a certain period of time (Currently between 36 and 72h). On the top the gas fee, this function will take an Ether fee from the on-chain balance for the hub operator.

**Parameters:**

| Name | Type | Description |
| ------ | ------ | ------ |
| address | `string` |  Targeted Ethereum address |
| amount | `BigNumber` \| `BN` \| `string` |  Amount to deposit, this amount needs to be available on-chain for the specified address |
| gasPrice | `BigNumber` \| `BN` \| `string` |  Gas price for the on-chain transaction. |
| gas | `number` |  Gas limit for the on-chain transaction. Around 100'000 gas is advised. |
| `Optional` tokenAddress | `string` |

**Returns:** `Promise`<`string`>
Hash of the on-chain transaction of the withdrawal request.

___

