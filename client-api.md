# Client API

## NOCUSTTransfer <a id="nocusttransfer"></a>

Interface For the parameters object required to make nocust transfers.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| to | `string` | Recipient of the transfer. |
| from | `string` | Sender of the transfer. |
| `Optional` tokenAddress | `string` | Token to send. Use Ether if not specified. |
| Amount | `BigNumber` \| `BN` \| `string` | Amount in wei \(for Ether\) of the transfer |
| `Optional` nonce | `BigNumber` | Specify a nonce, can be used to attribute an identifier to the transaction. Set randomly if not specified. |

## TransferDataInterface

Full transfer details including signatures and status.

| Name | Type | Description |
| :--- | :--- | ---: |
| id | number | Transaction ID, uniquely identify the transfer |
| amount | string | Transfer amount |
| wallet | TransferWalletDataInterface | Sender details. |
| recipient | TransferWalletDataInterface | Recipient details. |
| eon\_number | number | NOCUST operator eon or round of the transfer. |
| processed | boolean | If the payment operator processed the the transfer. The operator wait for the recipient confirmation before processing the transfer. |
| completed | boolean | If the transfer is fully confirmed. |
| canceled | boolean | If the transfer is canceled. |

## NOCUSTManager

### approveAndDeposit <a id="approveanddeposit"></a>

▸ **approveAndDeposit**\(address: `string`, amount: `BigNumber` _\|_ `BN` _\|_ `string`, gasPrice: `BigNumber` _\|_ `BN` _\|_ `string`, gas: `number`, tokenAddress?: `string`\): `Promise`&lt;`string`&gt;

Similarly to the `deposit` function but prior check to send the deposit transaction it checks for token transfer allowance of the NOCUST smart contract. If the allowance is not sufficient it sends an ERC-20 `aprove` transaction. Approving token transfers is a required operation by the ERC-20 standard. If the depositor does not have sufficient allowance the deposit will fail. Note that this function will make 2 on-chain transaction \(contract calls\) with the specified gas price and gas limit. Approvals are not required for Ether.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Targeted Ethereum address |
| amount | `BigNumber` \| `BN` \| `string` | Amount to deposit, this amount needs to be available on-chain for the specified address. |
| gasPrice | `BigNumber` \| `BN` \| `string` | Gas price for the on-chain transaction. |
| gas | `number` | Gas limit for the on-Chain transaction. Around 100'000 gas is advised. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. |

**Returns:** `Promise`&lt;`string`&gt; Hash of the on-chain transaction of the deposit.

### buySLA <a id="buysla"></a>

▸ **buySLA**\(address: `string`\): `Promise`&lt;`void`&gt;

Buys a SLA. The address needs to have the token amount available as a nocust balance on the address as specified by the `getSLADetail` function. This function will make a nocust transfer to pay for the SLA.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | - |

**Returns:** `Promise`&lt;`void`&gt;

### deposit <a id="deposit"></a>

▸ **deposit**\(address: `string`, amount: `BigNumber` _\|_ `BN` _\|_ `string`, gasPrice: `BigNumber` _\|_ `BN` _\|_ `string`, gas: `number`, tokenAddress?: `string`\): `Promise`&lt;`string`&gt;

Make an on-chain transaction to deposit funds into the NOCUST commit-chain. This funds can be later used for making off-chain NOCUST transfers.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Targeted Ethereum address |
| amount | `BigNumber` \| `BN` \| `string` | Amount to deposit, this amount needs to be available on-chain for the specified address. |
| gasPrice | `BigNumber` \| `BN` \| `string` | Gas price for the on-chain transaction. |
| gas | `number` | Gas limit for the on-Chain transaction. Around 100'000 gas is advised. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified. |

**Returns:** `Promise`&lt;`string`&gt; Hash of the on-chain transaction of the deposit.

### getBlockPerEon <a id="getblockpereon"></a>

▸ **getBlockPerEon**\(\): `Promise`&lt;`number`&gt;

Get the number of Eras \(blocks\) per Eon.

**Returns:** `Promise`&lt;`number`&gt;

### getBlocksToWithdrawalConfirmation <a id="getblockstowithdrawalconfirmation"></a>

▸ **getBlocksToWithdrawalConfirmation**\(address: `string`, txHash?: `string`, tokenAddress?: `string`\): `Promise`&lt;`number`&gt;

Get the number of blocks until it is possible to send the withdrawal confirmation on-chain transaction. It will return -1 if no withdrawals are pending or 0 if the withdrawal is ready to be confirmed.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Address that started the withdrawal. |
| `Optional` txHash | `string` | Transaction hash of the on-chain transaction of the of the withdrawal request. Will use the oldest pending withdrawal if not specified. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified. |

**Returns:** `Promise`&lt;`number`&gt;

### getEonNumber <a id="geteonnumber"></a>

▸ **getEonNumber**\(\): `Promise`&lt;`number`&gt;

Get the current Eon or round number.

**Returns:** `Promise`&lt;`number`&gt;

### getEraNumber <a id="geteranumber"></a>

▸ **getEraNumber**\(\): `Promise`&lt;`number`&gt;

Get the current Era or sub-block number. The number of block since the current Eon started.

**Returns:** `Promise`&lt;`number`&gt; Era

### getNOCUSTBalance <a id="getnocustbalance"></a>

▸ **getNOCUSTBalance**\(address: `string`, tokenAddress?: `string`\): `Promise`&lt;`BigNumber`&gt;

Fetch the current NOCUST balance.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Targeted Ethereum address. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified. |

**Returns:** `Promise`&lt;`BigNumber`&gt; Nocust balance

### getOnChainBalance <a id="getonchainbalance"></a>

▸ **getOnChainBalance**\(address: `string`, tokenAddress?: `string`\): `Promise`&lt;`BigNumber`&gt;

Get current on-chain balance.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Targeted Ethereum address. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified. |

**Returns:** `Promise`&lt;`BigNumber`&gt; On-chain balance.

### getOrderBook <a id="getorderbook"></a>

▸ **getOrderBook**\(buyTokenAddress: `string`, sellTokenAddress: `string`\): `Promise`&lt;`OrderBookDataInterface`&gt;

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| buyTokenAddress | `string` | - |
| sellTokenAddress | `string` | - |

**Returns:** `Promise`&lt;`OrderBookDataInterface`&gt;

### getSLADetail <a id="getsladetail"></a>

▸ **getSLADetail**\(\): `Promise`&lt;`SLADetailsInterface`&gt;

Get informations about the SLA pricing model.

**Returns:** `Promise`&lt;`SLADetailsInterface`&gt; Object with the token address with which to pay the SLA, the cost/amount of the SLA in this token, the recipient of the SLA payment, the transaction limit per month without SLA.

### getSLAStatus <a id="getslastatus"></a>

▸ **getSLAStatus**\(address: `string`\): `Promise`&lt;`number`&gt;

Get the SLA status for a given address.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Targeted Ethereum address. |

**Returns:** `Promise`&lt;`number`&gt; 0 if not under SLA, expiry date unix timestamp in millisecond if currently enroll with a SLA.

### getSupportedTokens <a id="getsupportedtokens"></a>

▸ **getSupportedTokens**\(\): `Promise`&lt;`object`\[\]&gt;

Fetch the smart contract addresses of the supported token by the commit-chain.

**Returns:** `Promise`&lt;`object`\[\]&gt; Promise that resolves with an array of objects `{ tokenAddress: string, name: string, shortName: string }` for each token supported by the commit-chain. Address is the address of the ERC-20 contract of the token.

### getTransaction <a id="gettransaction"></a>

▸ **getTransaction**\(transactionId: `number`\): `Promise`&lt;`TransferDataInterface`&gt;

Fetch the transaction details given a transaction ID

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| transactionId | `number` | Transaction ID of the transaction to fetch |

**Returns:** `Promise`&lt;`TransferDataInterface`&gt; Transaction details }\`

### getTransactionsForAddress <a id="gettransactionsforaddress"></a>

▸ **getTransactionsForAddress**\(address: `string`, tokenAddress?: `string`, roundNumber?: `number`\): `Promise`&lt;`TransferDataInterface`\[\]&gt;

Get the list of transactions for a given address and token \(Incoming and outgoing\).

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Targeted Ethereum address. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified. |
| `Optional` roundNumber | `number` | Get the transactions only starting from a specific round number. Return all transactions if unspecified. |

**Returns:** `Promise`&lt;`TransferDataInterface`\[\]&gt; Array of transactions

### getWalletState <a id="getwalletstate"></a>

▸ **getWalletState**\(address: `string`, tokenAddress?: `string`\): `Promise`&lt;`WalletState`&gt;

Get the `WalletState` object for lower level API use.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Targeted Ethereum address. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified. |

**Returns:** `Promise`&lt;`WalletState`&gt;

### getWithdrawalFee <a id="getwithdrawalfee"></a>

▸ **getWithdrawalFee**\(withdrawalRequestGasPrice: `BigNumber` _\|_ `BN` _\|_ `string`\): `Promise`&lt;`BigNumber`&gt;

Doing a withdrawal request involve a fee to the operator. This function gets the current fee.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| withdrawalRequestGasPrice | `BigNumber` \| `BN` \| `string` | Gas price that will be used for the withdrawal request. The Fee is dependant on the gas price to be used. |

**Returns:** `Promise`&lt;`BigNumber`&gt; Fee amount

### getWithdrawalLimit <a id="getwithdrawallimit"></a>

▸ **getWithdrawalLimit**\(address: `string`, tokenAddress?: `string`\): `Promise`&lt;`BigNumber`&gt;

Withdrawals are limited to a certain amount \(Because funds need to committed into a check point\) and the limit increase over time. This method gets you the current available off-chain funds for withdrawal.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Targeted Ethereum address. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified. |

**Returns:** `Promise`&lt;`BigNumber`&gt; Withdrawal limit.

### subscribeToIncomingTransfer <a id="subscribetoincomingtransfer"></a>

▸ **subscribeToIncomingTransfer**\(address: `string`, callBack: `function`, tokenAddress?: `any`\): `function`

Return an observable to notify of incoming transfers.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Address to watch for. |
| callBack | `function` | Callback function that will be called on incoming transfers. The function provides the incoming transfer as argument. |
| `Optional` tokenAddress | `any` | Targeted ERC-20 token. Use Ether if not specified. The string `'all'` can be used to be notified for all tokens available on the commit-chain. |

**Returns:**  `Promise`&lt; `unsubscribe () => Void>` A promise that resolves when the callback is registered. The promise resolves with the unsubscribe function of the callback.

**Example:**

```text
const unsubscribe = await nocustManager.subscribeToIncomingTransfer(
        bob,
        tx => console.log(`Incoming transaction from: ${tx.wallet.address} of: ${tx.amount.toString()} wei of token ${tx.wallet.token}.`), 
        'all'
    )
// Unsubscribe when it is not needed anymore
unsubscribe()
```

### isAddressRegistered <a id="isaddressregistered"></a>

▸ **isAddressRegistered**\(address: `string`, tokenAddress?: `string`\): `Promise`&lt;`boolean`&gt;

Check if an address is registered with the nocust commit-chain.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Address to check. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified. |

**Returns:** `Promise`&lt;`boolean`&gt; Return `true` if the registration is successful

### isAddressRegisteredWithOperator <a id="isaddressregisteredwithhub"></a>

▸ **isAddressRegisteredWithOperator**\(address: `string`, tokenAddress?: `string`\): `Promise`&lt;`boolean`&gt;

Check whether an address is registered with the commit-chain and can therefore receive payments.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Targeted Ethereum address. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified. |

**Returns:** `Promise`&lt;`boolean`&gt;

### isRecovery <a id="isrecovery"></a>

▸ **isRecovery**\(\): `Promise`&lt;`boolean`&gt;

Check whether the commit-chain is in recovery mode. If it is the case, the commit-chain can't be used anymore and funds need to be recovered. 

**Returns:** `Promise`&lt;`boolean`&gt; Recovery status.

### issueDeliveryChallenge <a id="issuedeliverychallenge"></a>

▸ **issueDeliveryChallenge**\(address: `string`, txId: `number`, gasPrice: `any`, gas: `any`, tokenAddress?: `string`\): `Promise`&lt;`string`&gt;

Make an on-chain transaction to initiate a delivery challenge.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Targeted Ethereum address. |
| txId | `number` | Transaction id to challenge for delivery. |
| gasPrice | `any` | Gas price for the on-chain transaction. |
| gas | `any` | Gas limit for the off-Chain transaction. Around 350'000 gas is advised. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified. |

**Returns:** `Promise`&lt;`string`&gt;

### issueStateUpdateChallenge <a id="issuestateupdatechallenge"></a>

▸ **issueStateUpdateChallenge**\(address: `string`, gasPrice: `any`, gas: `any`, tokenAddress?: `string`\): `Promise`&lt;`string`&gt;

Make an on-chain transaction to initiate a state update challenge.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Targeted Ethereum address. |
| gasPrice | `any` | Gas price for the on-chain transaction. |
| gas | `any` | Gas limit for the off-Chain transaction. Around 350'000 gas is advised. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified. |

**Returns:** `Promise`&lt;`string`&gt;

### recoverFunds <a id="recoverfunds"></a>

▸ **recoverFunds**\(address: `string`, gasPrice: `any`, gas: `any`, tokenAddress?: `string`\): `Promise`&lt;`string`&gt;

Make an on-chain transaction to recover funds whenever the commit-chain falls into recovery mode.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Targeted Ethereum address. |
| gasPrice | `any` | Gas price for the on-chain transaction. |
| gas | `any` | Gas limit for the off-Chain transaction. Around 350'000 gas is advised. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified. |

**Returns:** `Promise`&lt;`string`&gt;

### registerAddress <a id="registeraddress"></a>

▸ **registerAddress**\(address: `string`, tokenAddress?: `string`\): `Promise`&lt;`void`&gt;

Register an address for a given token with the commit-chain. An address needs to be registered with the commit-chain for each token separately in order to send or receive transfers. This operation is done implicitly when sending a transfer if the address is not yet registered. Note that for a transfer to succeed the recipient need to have registered.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Address to register with the commit-chain. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified. |

**Returns:** `Promise`&lt;`void`&gt;

### sendSwap <a id="sendswap"></a>

▸ **sendSwap**\(address: `string`, buyTokenAddress: `string`, sellTokenAddress: `string`, buyAmount: `BigNumber` _\|_ `BN` _\|_ `string`, sellAmount: `BigNumber` _\|_ `BN` _\|_ `string`\): `Promise`&lt;`number`&gt;

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Main wallet address. |
| buyTokenAddress | `string` | Token address to buy. |
| sellTokenAddress | `string` | Token address to sell. |
| buyAmount | `BigNumber` \| `BN` \| `string` | Amount of buy token to buy. |
| sellAmount | `BigNumber` \| `BN` \| `string` | Amount of sell token to swap for buying amount. |
| `Optional` subWalletSeedPhrase | `string` | \(optional\) A string signed by wallet's private key to generate a seed used to create auxilary swap accounts. Default value is "I want to use Liquidity Network Swaps" |

**Returns:** `Promise`&lt;`number`&gt;

### sendTransaction <a id="sendtransaction"></a>

▸ **sendTransaction**\(transfer: _NocustTransfer_\): `Promise`&lt;`number`&gt;

Send a NOCUST transfer.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| transfer | NocustTransfer | Parameter object of an off-chain transfer. |

**Returns:** `Promise`&lt;`number`&gt; Transaction Id of the NOCUST transfer

### synchronizeSwapOrders

▸ **synchronizeSwapOrders**\(address: `string`, subWalletSeedPhrase?: `string`\): `Promise`&lt;`TransferDataInterface[]`&gt;

Claims all auxilary wallet's fulfilled swap orders and returns a list of unfulfilled swaps.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Main wallet address. |
| `Optional` subWalletSeedPhrase | `string` | \(optional\) A string signed by wallet's private key to generate a seed used to create auxiliary swap accounts. Default value is "I want to use Liquidity Network Swaps" |

### syncWallet <a id="syncwallet"></a>

▸ **syncWallet**\(address: `string`, tokenAddress?: `string`\): `Promise`&lt;`void`&gt;

Force synchronize the wallet of the given public key for the given token. The result will be cached making later calls of this wallet faster. This function can be useful to pre-cache required internal data \(for example on starup of your app\) to make later function calls faster.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Targeted Ethereum address |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified |

**Returns:** `Promise`&lt;`void`&gt;

### withdrawalConfirmation <a id="withdrawalconfirmation"></a>

▸ **withdrawalConfirmation**\(address: `string`, gasPrice: `BigNumber` _\|_ `BN` _\|_ `string`, gas: `number`, tokenAddress?: `string`\): `Promise`&lt;`string`&gt;

Make an on-chain transaction to confirm a withdrawal previously initialized and effectively transfer the funds.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Targeted Ethereum address. |
| gasPrice | `BigNumber` \| `BN` \| `string` | Gas price for the on-chain transaction. |
| gas | `number` | Gas limit for the on-Chain transaction. Around 100'000 gas is advised. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified. |

**Returns:** `Promise`&lt;`string`&gt; Hash of the on-chain transaction of the withdrawal confirmation.

### withdrawalRequest <a id="withdrawalrequest"></a>

▸ **withdrawalRequest**\(address: `string`, amount: `BigNumber` _\|_ `BN` _\|_ `string`, gasPrice: `BigNumber` _\|_ `BN` _\|_ `string`, gas: `number`, tokenAddress?: `string`\): `Promise`&lt;`string`&gt;

Make an on-chain transaction to initiate a withdrawal to remove the funds from the NOCUST smart contract and to get them at the specified address. The amount specified needs to be available for withdrawal . The withdrawal will have to be confirmed after a certain period of time \(Currently between 36 and 72h\). On the top the gas fee, this function will take an Ether fee from the on-chain balance for the commit-chain operator.

**Parameters:**

| Name | Type | Description |
| :--- | :--- | :--- |
| address | `string` | Targeted Ethereum address |
| amount | `BigNumber` \| `BN` \| `string` | Amount to deposit, this amount needs to be available on-chain for the specified address |
| gasPrice | `BigNumber` \| `BN` \| `string` | Gas price for the on-chain transaction. |
| gas | `number` | Gas limit for the on-chain transaction. Around 100'000 gas is advised. |
| `Optional` tokenAddress | `string` | Targeted ERC-20 token. Use Ether if not specified. |

**Returns:** `Promise`&lt;`string`&gt; Hash of the on-chain transaction of the withdrawal request.

