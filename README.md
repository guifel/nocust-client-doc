# Getting Started

## Introduction

The `nocust-client-library` allows you to interact with [NOCUST](https://liquidity.network/NOCUST_Liquidity_Network_Paper.pdf) non-custodian off-chain payment hubs. It is a layer 2 solution to scale blockchain such as Ethereum and it is working today ! In this document we will describe the client library allowing developers to build wallets with full off-chain capabilities. The library allows you to deposit and Withdraw Ether or ERC-20 token into the payment hub, make payments and Initiate disputes. The library insure internally the security of the off-chain wallet by monitoring the smart-contract of the payment hub and the state of the payment hub. Further the library allow users to make off-chain token swaps. 

## Installation



Installation:

```
npm install nocust-client
```



## Example

The following code setup the library and make a transfer to to Alice

```typescript
import web3 from 'Web3' // Web3 1.0.x
import { LQDManager } from 'nocust-client' 

const bob = '0xFFcf8FDEE72ac11b5c542428B35EEF5769C409f0';
const alice = '0x22d491Bde2303f2f43325b2108D26f1eAbA1e32b';

const bobPrivateKey = '0x6cbed15c793ce57650b9877cf6fa156fbef513c4e6134f022a85b1ffdd59b2a1'

function async foo() {
    
  // Setup web3 with Infura
  const web3 = new Web3(new Web3.providers.HttpProvider('https://mainnet.infura.io/'));
  web3.eth.accounts.wallet.add(bobPrivateKey) // Add private to web3 for signing

  // Setup the LQDManager
  lqdManager = new LQDManager({
    rpcApi: web3,
    hubApiUrl: 'https://public.liquidity.network/', // TODO UPDATE
    contractAddress: '0xad471Bde2303f2f43325b2108D26f1eAbA1e32b', // TODO UPDATE
  });
  
  // Register an address to be used with the LQD manager
  await lqdManager.register(bob)
  
  // Send 0.01 Ether off-chain to Alice  
  lqdManager.sendTransfer({
      to: alice,
      amount: web3.utils.toWei(0.01,'ether'), // Amount in wei
      from: bob,
   });  
  
}

```