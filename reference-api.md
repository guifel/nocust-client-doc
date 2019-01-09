# Reference API

### LQDManager  (Constructor) 
The `LQDManager` object provides all the methods. This constructor requires a Web3 instance version 1.0.x with a private key loaded. The private will be used later to generate signatures to register, send transfer, receive transfer, etc..

| Parameter   | Type                                    |                                      Description |
| :----- | :-------------------------------------- | -----------------------------------------------: |
| config | [LQDManagerConfig](#lqdmanagerconfig-type) | Configuration object for LQD manager constructor |

| Return     |            Description |
| :--------- | ---------------------: |
| LQDManager | Main LQDManager object |


__Example:__
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


### LQDManagerConfig (Type)

Configuration object for the `LQDManager` constructor.

| Name            | Type   |                          Description |
| :-------------- | :----- | -----------------------------------: |
| rpcApi          | Web3   |            Instance of a Web3 object |
| contractAddress | string | Address of the NOCUST smart contract |
| hubApiUrl       | string |         Url of the NOCUST server API |
|  rpcProvider (Optional) | RPCClientProviderInterface | Interface to implement to provide a custom rpc provider |
|  web3Service (Optional)|  Web3ServiceInterface |      Interface to implement to provide a custom web3 service |
|  storageManager (Optional) |  StorageManagerInterface | Interface to implement to provide persistent storage of the wallet. This is critical to persist the necessary data to open challenges. |
|  logging (Optional) | LoggingInterface | Interface to implement to customize logging |
|  restApi (Optional) | RestApiInterface | Interface to implement to customize HTTP requests. (By default XHR is used) |


### lqdManager.register 

Method to register the addresses for use with specific tokens for the `lqdManager`. It is required to call this method before doing any operation with `lqdManager`.  It returns an event emitter that can be subscribed to get notified on incoming transfers.

| Parameter            | Type   |                          Description |
| :-------------- | :----- | -----------------------------------: |
| address  | string | Address to initialise to be used with the `lqdManager` |
tokenAddress (Optional) | string or string[] | Addresses of the ERC-20 contract to register to be used with the `lqdManager`.  Ether will be always register, if nothing is specified only Ether will be usable 
| Return            |                      Description |
| :-------------- | -----------------------------------: |
|Promise<[IncomingTransferEmitter](#incomingtransferemitter-interface)> | Event emitter to get notified on incoming transfer|

### IncomingTransferEmitter (Interface)

### lqdManager.sendTransfer

### lqdManagerPostTransfer

### OffChainTransfer (Type)

### lqdManager.syncWallet

### lqdManager.deposit

### lqdManager.withdrawalrequest

### lqdManager.withdrawalConfirmation

### lqdManager.getOffChainBalance

### lqdManager.getSupportedTokens

### lqdManager.getTransaction

### lqdManager.getWithdrawalLimit

### lqdManager.getOnChainBalance

### lqdManager.getWithdrawalFee

### lqdManager.getRoundNumber

### lqdManager.getRoundBlockNumber

### lqdManager.getBlockPerRound

### lqdManager.getWalletState

### lqdManager.getOrderBook

### lqdManager.isRecovery

### lqdManager.isAddressRegisteredWithHub

### lqdManager.isConnectedToHub

### lqdManager.challenge.issueStateUpdateChallenge

### lqdManager.challenge.issueDeliveryChallenge

### lqdManager.challenge.recoverFunds

