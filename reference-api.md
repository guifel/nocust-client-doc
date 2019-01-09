# Reference API

### LQDManager  (Constructor) 
The `LQDManager` object provides all the methods. This constructor requires a Web3 instance version 1.0.x with a private key loaded. The private will be used later to generate signatures to register, send transfer, receive transfer, etc..

__Parameters:__ 

| Name   | Type                                    |                                      Description |
| :----- | :-------------------------------------- | -----------------------------------------------: |
| config | [LQDManagerConfig](#lqd-manager-config) | Configuration object for LQD manager constructor |

__Return:__
| Type       |            Description |
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



### LQDManagerConfig (Type) <a name="lqd-manager-config"></a>

Configuration object for the `LQDManager` constructor.

| Name            | Type   |                          Description |
| :-------------- | :----- | -----------------------------------: |
| rpcApi          | Web3   |            Instance of a Web3 object |
| contractAddress | string | Address of the NOCUST smart contract |
| hubApiUrl       | string |         Url of the NOCUST server API |
|  rpcProvider (Optional) | RPCClientProviderInterface | Custom rpc provider|
|  web3Service (Optional)|  Web3ServiceInterface | Custom web3 service |
|  storageManager (Optional) |  StorageManagerInterface | |
|  logging (Optional) | LoggingInterface | |
|  restApi (Optional) | RestApiInterface | |