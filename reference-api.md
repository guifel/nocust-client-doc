# Reference API

## syncLQDManager  \(Constructor\)

The `LQDManager` object provides all the methods. This constructor requires a Web3 instance version 1.0.x with a private key loaded. The private will be used later to generate signatures to register, send transfer, receive transfer, etc..

| Parameter | Type | Description |
| :--- | :--- | ---: |
| config | [LQDManagerConfig](reference-api.md#lqdmanagerconfig-type) | Configuration object for LQD manager constructor. |

| Return | Description |
| :--- | ---: |
| LQDManager | Main LQDManager object. |

**Example:**

```typescript
import { LQDManager, LQDManagerConfig } from 'nocust-client' 

const bobPrivateKey = '0x6cbed15c793ce57650b9877cf6fa156fbef513c4e6134f022a85b1ffdd59b2a1'

// Setup web3 with Infura
const web3 = new Web3(new Web3.providers.HttpProvider('https://mainnet.infura.io/'));
web3.eth.accounts.wallet.add(bobPrivateKey) // Add private to web3 for signing

// Setup the LQDManager
const config : LQDManagerConfig = {
  rpcApi: web3,
  hubApiUrl: 'https://public.liquidity.network/', // TODO UPDATE
  contractAddress: '0xad471Bde2303f2f43325b2108D26f1eAbA1e32b', // TODO UPDATE,
}

lqdManager = new LQDManager(config);
```

## LQDManagerConfig \(Type\)

Configuration object for the `LQDManager` constructor.

| Name | Type | Description |
| :--- | :--- | ---: |
| rpcApi | Web3 | Instance of a Web3 object. |
| contractAddress | string | Address of the NOCUST smart contract. |
| hubApiUrl | string | Url of the NOCUST server API. |
| rpcProvider \(Optional\) | RPCClientProviderInterface | Interface to implement to provide a custom rpc provider. |
| web3Service \(Optional\) | Web3ServiceInterface | Interface to implement to provide a custom web3 service. |
| storageManager \(Optional\) | StorageManagerInterface | Interface to implement to provide persistent storage of the wallet. This is critical to persist the necessary data to open challenges. |
| logging \(Optional\) | LoggingInterface | Interface to implement to customize logging. |
| restApi \(Optional\) | RestApiInterface | Interface to implement to customize HTTP requests \(By default XHR is used\). |

## lqdManager.register

Method to register the addresses for use with specific tokens for the `lqdManager`. It is required to call this method before doing any operation with `lqdManager`. It returns an event emitter that can be subscribed to get notified on incoming transfers.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Address to initialize to be used with the `lqdManager`. |

tokenAddress \(Optional\) \| string or string\[\] \| Addresses of the ERC-20 contract to register to be used with the `lqdManager`. Ether will be always register, if nothing is specified only Ether will be usable

| Return | Description |
| :--- | ---: |
| Promise&lt;[IncomingTransferEmitter](reference-api.md#incomingtransferemitter-interface)&gt; | Event emitter to get notified on incoming transfer. |

**Example:**

```typescript
// Public key for which the private key is loaded in the Web3 instance used by the LQD manager
const bob = '0xFFcf8FDEE72ac11b5c542428B35EEF5769C409f0'; 

// ERC-20 contract address of LQD token
const lqdContractAddress = '0xe982E462b094850F12AF94d21D470e21bE9D0E9C'; 

// Register the lqdManager for the use of Ether and LQD token for Bob
const eventEmitter = await lqdManager.register(bob, lqdContractAddress);

// Log on incoming transfer
eventEmitter.on('IncomingTransfer', (transfer: TransferDataInterface) =>
      console.log(`Bob is receiving a transfer of  ${transfer.amount} wei from ${transfer.wallet.address}`),
    );
```

## lqdManager.sendTransfer

Send an off-chain transfer. Return a PromiEvent \(Promise + Event emitter\) that resolves when the transfer is fully confirmed \(Waiting for recipient and hub confirmations\). The PromiEvent allows to trigger an event when the transfer is posted \(Transfer signed by the sender of the transfer and posted to the hub\)

| Parameter | Type | Description |
| :--- | :--- | ---: |
| transfer | OffChainTransfer | Parameter object of an off-chain transfer. |

| Return | Description |
| :--- | ---: |
| PromiEvent | Promise and event at the same time, it can be subscribe on event 'Posted' and the promise return the transaction ID. |

**Exemple:**

```typescript
// Send 0.1 Ether from bob to Alice
const transferPromise = lqdManager.sendTransfer({
      to: bob,                                       // Bob Ethereum public key
      amount: (new BigNumber(0.1)).shiftedBy(18), // Amount in wei
      from: alice,                                  // Alice Ethereum public key
    });

    // Log on transfer posted
transferPromise.on('Posted', txId => console.log(`Transfer from Bob to Alice Posted. Transaction ID: ${txId}`));

// Wait for the promise to resolve 
const txId = await transferPromise;

console.log(`Transfer from Bob to Alice is confirmed. Transaction ID: ${txId}`)
```

## lqdManagerPostTransfer

Simply post a transfer without waiting for its confirmation \(Sub step of [senTransfer](https://github.com/guifel/nocust-client-doc/tree/2405cafaaceba70946411d032a341e21a69472a6/lqdmanager-sendtransfer/README.md)\).

| Parameter | Type | Description |
| :--- | :--- | ---: |
| transfer | OffChainTransfer | Parameter object of an off-chain transfer. |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with the transaction ID. |

## OffChainTransfer \(Type\)

Parameters to execute an off-chain transfer

| Name | Type | Description |
| :--- | :--- | ---: |
| to | string | Recipient of the transfer, this address needs to have been registered with the payment hub at least once in the past. You can insure that the receipient is registered by using the [isAddressRegisteredWithHub](reference-api.md#lqdmanager-isaddressregisteredwithhub) method |
| amount | BigNumber | Amount in wei \(for Ether\) of the transfer |
| from | string | Sender of the off-chain transfer \(The corresponding private key will need to be loaded in the web3 instance of the lqdManager \) |
| tokenAddress \(Optional\) | string | Address of the ERC-20 token to transfer, execute an Ether transfer if not specified. |
| nonce \(Optional\) | BigNumber | Specify a nonce, can be used to attribute an identifier to the transaction. Set randomly if not specified. |

**Example:**

```typescript
import {  OffChainTransfer  } from 'nocust-client' 

// Transfer 12  of an ERC-20 Token
const transfer :  OffChainTransfer {
  to:  "0xFFcf8FDEE72ac11b5c542428B35EEF5769C409f0"
  amount:  (new BigNumber(12)).shiftedBy(18)  
  tokenAddress: "0xe982E462b094850F12AF94d21D470e21bE9D0E9C" // ERC-20 token contract
  from: "0x22d491Bde2303f2f43325b2108D26f1eAbA1e32b"
}
```

## TransferDataInterface \(Type\)

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

**TransferWalletDataInterface:**

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Ethereum address of the recipient/sender |
| token | string | Address of the ERC-20 smart contract of the transfer. For Ether this field is equal to the address of the payment hub. |

## lqdManager.syncWallet

Force synchronize the wallet of the given public key for the given token. The result will be cached making later calls of this wallet faster.  This function can be useful to pre-cache required internal data \(for example on starup of your app\) to make later function calls faster.  Additionally, it returns a \`WalletState\` object that can be used for lower level operations.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Targeted Ethereum address |
| tokenAddress \(Optional\) | string | Targeted ERC-20 token. Use Ether if not specified |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves when the operation finishes |

## lqdManager.deposit

Make an on-chain transaction to deposit funds into the NOCUST smart-contract. This funds can be later used for making off-chain transfers.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Targeted Ethereum address |
| tokenAddress \(Optional\) | string | Targeted ERC-20 token. Use Ether if not specified |
| amount | BigNumber | Amount to deposit, this amount needs to be available on-chain for the specified address |
| gasPrice | BigNumber | Gas price for the on-chain transaction. |
| gas | number | Gas limit for the off-Chain transaction. Around 100'000 gas is advised. |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with the on-chain transaction hash when the operation finishes |

## lqdManager.withdrawalrequest

Make an on-chain transaction to initiate a withdrawal to remove the funds from the NOCUST smart contract and to get them at the specified address. The amount specified needs [to be available for withdrawal](reference-api.md#lqdmanager-getwithdrawallimit) . The withdrawal will have to be [confirmed](reference-api.md#lqdmanager-withdrawalconfirmation) after a certain period of time \(Currently between 36 and 72h\). On the top the gas fee, this function will take an [Ether fee](reference-api.md#lqdmanager-getwithdrawalfee) from the on-chain balance for the hub operator.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Targeted Ethereum address |
| tokenAddress \(Optional\) | string | Targeted ERC-20 token. Use Ether if not specified |
| amount | BigNumber | Amount to withdraw, this amount needs to be available off-chain for the specified address |
| gasPrice | BigNumber | Gas price for the on-chain transaction. |
| gas | number | Gas limit for the off-Chain transaction. Around 300'000 gas is advised. |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with the on-chain transaction hash when the operation finishes |

## lqdManager.withdrawalConfirmation

Make an on-chain transaction to confirm a withdrawal previously [initialized](reference-api.md#lqdmanager-withdrawalrequest) and effectively transfer the funds.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Targeted Ethereum address |
| tokenAddress \(Optional\) | string | Targeted ERC-20 token. Use Ether if not specified |
| gasPrice | BigNumber | Gas price for the on-chain transaction. |
| gas | number | Gas limit for the off-Chain transaction. Around 100'000 gas is advised. |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with the on-chain transaction hash when the operation finishes |

## lqdManager.getOffChainBalance

Fetch off-chain balance.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Targeted Ethereum address |
| tokenAddress \(Optional\) | string | Targeted ERC-20 token. Use Ether if not specified |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with the off-chain balance. |

## lqdManager.getSupportedTokens

Fetch the smart contract addresses of the supported token by the payment hub.

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with an array of the ERC-20 smart contracts addresses supported by the payment hub. Note that Ether will appear with the address of the NOCUST smart contract. |

## lqdManager.getTransaction

Get the transaction details for a given transaction ID.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| transactionId | number | Transaction of the desired transaction. |

| Return | Description |
| :--- | ---: |
| Promise&lt;[TransferDataInterface](reference-api.md#transferdatainterface-type)&gt; | Promise that resolves with the required transfer details. |

## lqdManager.getWithdrawalLimit

Withdrawals are limited to a certain amount \(Because funds need to committed into a check point\) and the limit increase over time. This method gets you the current available off-chain funds for withdrawal.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Targeted Ethereum address |
| tokenAddress \(Optional\) | string | Targeted ERC-20 token. Use Ether if not specified |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with the current withdrawal limit |

## lqdManager.getOnChainBalance

Get current on-chain balance.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Targeted Ethereum address |
| tokenAddress \(Optional\) | string | Targeted ERC-20 token. Use Ether if not specified |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with the current on-chain balance. |

## lqdManager.getWithdrawalFee

Gets the current withdrawal fee to the payment hub operator.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Any Ethereum address registered with the hub |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with the current withdrawal fee. |

## lqdManager.getRoundNumber

Gets the current round of the payment hub \(Number of checkpoints submitted since its creation\). Note that one round is currently about 36h \(8640 blocks\).

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Any Ethereum address registered with the hub |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with the current round |

## lqdManager.getRoundBlockNumber

Gets the number of blocks since the beginning of the current round \(Sub block\).

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Any Ethereum address registered with the hub |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with the current sub block. |

## lqdManager.getBlockPerRound

Gets the current number of block for one round \(Currently 8640 blocks\)

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Any Ethereum address registered with the hub |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with the number of block per round. |

## lqdManager.getWalletState

Get the `WalletState` object for lower level API use.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Targeted Ethereum address |
| tokenAddress \(Optional\) | string | Targeted ERC-20 token. Use Ether if not specified |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with required wallet State. |

## lqdManager.isRecovery

Check weather the payment is on recovery mode \(Meaning that the payment lost a challenge or missed to submit a checkpoint\). If it returns true the payment hub is no longer in operation, the only operation possible to withdraw your off-chain balance using the [recoverFunds](reference-api.md#lqdmanager-recoverfunds) method.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Any Ethereum address registered with the hub. |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves the recovery boolean. |

## lqdManager.isAddressRegisteredWithHub

Check if the address is registered with the payment hub for the specified token. This address is therefore capable of receiving off-chain funds for this token.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Targeted Ethereum address |
| tokenAddress \(Optional\) | string | Targeted ERC-20 token. Use Ether if not specified |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with the registration boolean. |

## lqdManager.isConnectedToHub

Check if the lqdManager is connected to the payment hub and is capable of receiving off-chain payments.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Targeted Ethereum address |
| tokenAddress \(Optional\) | string | Targeted ERC-20 token. Use Ether if not specified |

| Return | Description |
| :--- | ---: |
| boolean | Is the lqdManager connected. |

## lqdManager.challenge.issueStateUpdateChallenge

Make an on-chain transaction to initiate a state update challenge.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Targeted Ethereum address |
| gasPrice | BigNumber | Gas price for the on-chain transaction. |
| gas | number | Gas limit for the off-Chain transaction. Around 350'000 gas is advised. |
| tokenAddress \(Optional\) | string | Targeted ERC-20 token. Use Ether if not specified |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with the on-chain transaction hash when the operation finishes |

## lqdManager.challenge.issueDeliveryChallenge

Make an on-chain transaction to initiate a delivery challenge.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Targeted Ethereum address |
| gasPrice | BigNumber | Gas price for the on-chain transaction. |
| gas | number | Gas limit for the off-Chain transaction. Around 350'000 gas is advised. |
| tokenAddress \(Optional\) | string | Targeted ERC-20 token. Use Ether if not specified |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with the on-chain transaction hash when the operation finishes |

## lqdManager.challenge.recoverFunds

Make an on-chain transaction to recover funds whenever the hub falls into recovery mode.

| Parameter | Type | Description |
| :--- | :--- | ---: |
| address | string | Targeted Ethereum address |
| gasPrice | BigNumber | Gas price for the on-chain transaction. |
| gas | number | Gas limit for the off-Chain transaction. Around 350'000 gas is advised. |
| tokenAddress \(Optional\) | string | Targeted ERC-20 token. Use Ether if not specified |

| Return | Description |
| :--- | ---: |
| Promise | Promise that resolves with the on-chain transaction hash when the operation finishes |

