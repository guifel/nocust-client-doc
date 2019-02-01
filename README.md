# Getting Started

[**Full API reference**](https://liquidity-network.gitbook.io/project/)

## Introduction

The `nocust-client-library` allows you to interact with the [NOCUST](https://liquidity.network/NOCUST_Liquidity_Network_Paper.pdf) commit-chain. It is a layer 2 solution to scale blockchains such as Ethereum and it works today on the mainet! In this document, we describe the client library that allows developers to build wallets with full commit-chain capabilities. The library enables you to deposit (convert to commit-chain coins) and withdraw (convert to Ethereum coins) Ether or ERC-20 tokens, in or out of the commit-chain, make payments and initiate disputes. The library internally ensures the security of the commit-chain wallet by monitoring the smart-contract of the commit-chain operator (Henry) and the state of the commit-chain. Furthermore, the library allows users to make commit-chain token swaps! 

![NOCUST setup](https://raw.githubusercontent.com/guifel/nocust-client-doc/master/img/setup.png)

The Nocust client library will be located on Bob's end. This schema illustrates the diffrent roles of each component in a NOCUST payment system. Find an introduction to NOCUST payment systems [here](https://blog.liquidity.network/2018/11/21/nocust-101/).

## Installation

To install the library, simply run:

```text
npm install nocust-client
```

The library has to be used with Web3 \(version 1.0.0-beta.36 only for now\) to interact with the Ethereum Network. Additionally, as we are manipulating exclusively Ethers amounts in wei \(10^-18 Ether\). And that these amounts are potentially very large, [bigger than the Javascript safe limit](https://stackoverflow.com/questions/307179/what-is-javascripts-highest-integer-value-that-a-number-can-go-to-without-losin). We use the `bignumber.js` library for Ether and token amounts.

Required dependencies:

```text
npm install web3@1.0.0-beta.36 bignumber.js
```

For typescript users:

The following configuration is recommended in your `tsconfig.json` file.

```javascript
{
  "compilerOptions": {
      "baseUrl": ".",
      "paths": { "*": ["types/*"] },
    "target": "es6",
    "module": "commonjs",
    "outDir": "dist",
    "declaration": true,
    "sourceMap": true,
    "removeComments": true,
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "typeRoots": [
      "node_modules/@types"
    ],
    "types": [
      "node"
   ],
  }
}
```

## Deployed Payment Hubs

This is a list of the currently deployed payment hubs. See in the example below how to use these values to get started with the nocust-client

| Ethereum Mainnet hub |  |
| :--- | :--- |
| NOCUST smart-contract address \(`contractAddress`\) | To be deploy |
| Hub API URL \(`hubApiUrl`\) | [https://public.liquidity.network/](https://public.liquidity.network/) |
| LQD ERC-20 contract address | 0xD29F0b5b3F50b07Fe9a9511F7d86F4f4bAc3f8c4 |

| Rinkeby hub | Value |
| :--- | :--- |
| NOCUST smart-contract address \(`contractAddress`\) | 0x6B9f10931E88349A572F2f0883E49528902B4b5D |
| Hub API URL \(`hubApiUrl`\) | [https://rinkeby.liquidity.network/](http://rinkeby.liquidity.network/) |
| Test ERC-20 contract address | 0xA9F86DD014C001Acd72d5b25831f94FaCfb48717 |



This is a test hub on a private blockchain with a shorter block interval \(4 sec instead of 15 sec\) and a shorter round time \(4 min instead of 36 hours\). Initiate Web3 with a HTTP provider with the RPC URL provided in the table below. 

| Private test Hub |  |
| :--- | :--- |
| NOCUST smart-contract address \(`contractAddress`\) | 0x9561C133DD8580860B6b7E504bC5Aa500f0f06a7 |
| Hub API URL \(`hubApiUrl`\) | [https://limbo.liquidity.network/](https://limbo.liquidity.network/admission/) |
| Test ERC-20 contract address | 0xe982E462b094850F12AF94d21D470e21bE9D0E9C |
| Ethereum node RPC URL | [https://limbo.liquidity.network/ethrpc](https://limbo.liquidity.network/ethrpc) |

## Examples

### Off-chain transfers

The following code enables the setup of the library and for Bob to make an Ether transfer to Alice.

```typescript
import web3 from 'Web3' // Web3 1.0.x
import BigNumber from "bignumber.js"
import { LQDManager } from 'nocust-client'

const bob = '0xFFcf8FDEE72ac11b5c542428B35EEF5769C409f0';
const alice = '0x22d491Bde2303f2f43325b2108D26f1eAbA1e32b';

const bobPrivateKey = '0x6cbed15c793ce57650b9877cf6fa156fbef513c4e6134f022a85b1ffdd59b2a1'
const alicePrivateKey ='0x6370fd033278c143179d81c5526140625662b8daa446c22ee2d73db3707e620c'

// Setup web3 with Infura
const web3 = new Web3(new Web3.providers.HttpProvider('https://rinkeby.infura.io/'));
// Add private key to web3 for signing 
web3.eth.accounts.wallet.add(bobPrivateKey)
web3.eth.accounts.wallet.add(alicePrivateKey)

// Setup the LQDManager
const lqdManager = new LQDManager({
  rpcApi: web3,
  hubApiUrl: 'https://rinkeby.liquidity.network/',
  contractAddress: '0x6B9f10931E88349A572F2f0883E49528902B4b5D',
  });

const sendToALice = async () => {

  // Register an address to be used with the LQD manager
  await lqdManager.register(bob)
  await lqdManager.register(alice)

  // Send 0.01 Ether off-chain to Alice  
  const txId = await lqdManager.postTransfer({
      to: alice,
      // 0.01 Ether in wei as BigNumber. 
      // Note that you need the funds first, see deposits below
      amount: (new BigNumber(0.01)).shiftedBy(-18),
      from: bob,
   });

  console.log("Transfer to Alice sent ! Transaction ID: ", txId)

}

sendToALice()

```

NOCUST hubs currently require recipients of transfers to be online, and to sign a message in order to receive an off-chain transfer. Once the library has been setup and after calling the `register` function, transfers will automatically be accepted. Alice needs to setup a lqdManager with her private key as follows:

```typescript
import web3 from 'Web3' // Web3 1.0.x
import { LQDManager } from 'nocust-client'

const alice = '0x22d491Bde2303f2f43325b2108D26f1eAbA1e32b';

const alicePrivateKey = '0x6370fd033278c143179d81c5526140625662b8daa446c22ee2d73db3707e620c'

// Setup web3 with Infura
const web3 = new Web3(new Web3.providers.HttpProvider('https://mainnet.infura.io/'));
web3.eth.accounts.wallet.add(alicePrivateKey) // Add private to web3 for signing

// Setup the LQDManager
const lqdManager = new LQDManager({
  rpcApi: web3,
  hubApiUrl: 'https://rinkeby.liquidity.network/',
  contractAddress: '0x6B9f10931E88349A572F2f0883E49528902B4b5D',
});

function async register() {

  // Register an address to be used with the LQD manager
  const incomingTransferEventEmitter = await lqdManager.register(alice)

  // Log on incoming transfers
  incomingTransferEventEmitter.on('IncomingTransfer',
    (transfer: TransferDataInterface) => {
      console.log(`Alice is receiving a transfer of  ${transfer.amount} wei from ${transfer.wallet.address}`),
    });
  )

  console.log("Alice is ready to receive transfers !")

}

register()
```

Note that we will soon be releasing an upgrade of the protocol to allow passive accept. This means that no specific actions will be required from the recipient in order to receive a transfer.

### ERC-20 token transfers

NOCUST hubs support off-chain transfers of ERC-20 tokens. However, the operator chooses which tokens can be used. To see which tokens are supported by a hub call `getSupportedTokens`:

```typescript
const supportedTokenArray : string[] = await lqdManager.getSupportedTokens()
const NocustContract : string = supportedTokenArray[0]
const tokenXYZcontract : string = supportedTokenArray[1]
```

`supportedTokenArray` will contain an array of ERC-20 smart-contract addresses available to use.

The address at index 0 is the address of the NOCUST smart-contract. This simply means that the hub supports Ether transfers.

We need to specify to the `register` function what token we want to use:

```typescript
await lqdManager.register(bob, tokenXYZcontract)
// Bob can receive Ether and the token at the address `tokenXYZcontract`
```

The register function always registers Ether by default at the very least. Additionally it registers the tokens passed in a second parameter. A single token address or multiple token addresses can be passed. Note that the recipient will also have to `register` the token.

To make a token transfer, simply specify the address of the token in the `tokenAddress` field.

```typescript
  const txId : number = await lqdManager.sendTransfer({
      to: alice,
      amount: web3.utils.toWei(0.01,'ether'), // Amount
      from: bob,
      tokenAddress: tokenXYZ,
   });
```

### Deposits

In order to make transfers, off-chain funds are required. Off-chain funds are simply funds deposited into the NOCUST smart-contract. The library provides a function to deposit funds into the hub contract.

```typescript
const transactionHash : string = await lqdManager.deposit(
  bob,                             // Account from which to make a deposit (Its private key needs to be in the Web3 instance)
  web3.utils.toWei(0.5,'ether'), // Amount to deposit
  web3.utils.toWei(10,'gwei'),   // Gas price, 10 Gwei
  150000                         // Gas Limit
);
```

This function makes an on-chain contract call to the NOCUST contract and sends the specified amount to the contract. If the transaction is successful, after `60` confirmation blocks the funds will be available in the off-chain account. To verify the off-chain balance call the `getOffChainBalance` function:

```typescript
const balance : BigNumber = await lqdManager.getOffChainBalance(bob);
console.log("Bob's off-chain balance is: ", balance.toString())
```

All of these functions can be used with a parameter `tokenAddress` to similarly manipulate ERC-20 tokens. Don't forget to give transfer allowance to the nocust contract for the ERC-20 you wish to use. \(Explanations [here](https://medium.com/ethex-market/erc20-approve-allow-explained-88d6de921ce9)\)

### Withdrawals

If the user wishes to send their off-chain funds back on-chain \(also called an exit\) he needs to execute a withdrawal. Withdrawals take a longer time and are a 2 step process requiring 2 contract calls.

The amount of off-chain funds available for withdrawal may differ from the current off-chain balance. Recently acquired off-chain funds cannot be withdrawn instantly, they will be made fully available over time. Recently acquired funds will need between 36h and 72h \(one full round\) to be available. To check the current balance available for withdrawal call the function `getWithdrawalLimit` :

```typescript
const withdrawalLimit : BigNumber = lqdManager.getWithdrawalLimit(bob)
```

Any amount inferior or equal to this amount can be withdrawn.

To initiate a withdrawal the function `withdrawalRequest` will need to be used :

```typescript
const transactionHash : string = await lqdManager.withdrawalRequest(
  bob,                             // Account from which to make the withdrawal
  web3.utils.toWei(0.5,'ether'), // Amount to withdraw
  web3.utils.toWei(10,'gwei'),   // Gas price, 10 Gwei
  300000                         // Gas Limit
);
```

This will make a contract call to initiate a withdrawal.

The funds will not be transfered back to Bob's address instantly. A wait of between 36h and 72h is needed \(again one full round\), to check how much time is left before the withdrawal can be confirmed the function `getBlocksToWithdrawalConfirmation` can be used:

```typescript
const blocksToConfirmation : number = await lqdManager.getBlocksToWithdrawalConfirmation(bob)
```

This function returns the number of blocks needed before we can send the confirmation transaction. If it returns `0` the withdrawal is ready for confirmation. Note that the function will return `-1` if there is no withdrawal pending.

Finally, to confirm the withdrawal we need to:

```typescript
const transactionHash : string = await lqdManager.withdrawalConfirmation(
  bob,                             // Account from which to make the withdrawal
  web3.utils.toWei(10,'gwei'),   // Gas price, 10 Gwei
  300000                         // Gas Limit
);
```

This contract call will effectively transfer the funds from the NOCUST smart contract to Bob's address.

