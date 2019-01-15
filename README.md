# Getting Started

## Introduction

The `nocust-client-library` allows you to interact with [NOCUST](https://liquidity.network/NOCUST_Liquidity_Network_Paper.pdf) non-custodian off-chain payment hubs. It is a layer 2 solution to scale blockchain such as Ethereum and it is working today ! In this document we will describe the client library allowing developers to build wallets with full off-chain capabilities. The library allows you to deposit and Withdraw Ether or ERC-20 token into the payment hub, make payments and Initiate disputes. The library insure internally the security of the off-chain wallet by monitoring the smart-contract of the payment hub and the state of the payment hub. Further the library allow users to make off-chain token swaps.

## Installation



Installation:

```
npm install nocust-client
```



## Examples

### Off-chain transfers

The following code setup the library and make an Ether transfer to to Alice

```typescript
import web3 from 'Web3' // Web3 1.0.x
import { LQDManager } from 'nocust-client'

const bob = '0xFFcf8FDEE72ac11b5c542428B35EEF5769C409f0';
const alice = '0x22d491Bde2303f2f43325b2108D26f1eAbA1e32b';

const bobPrivateKey = '0x6cbed15c793ce57650b9877cf6fa156fbef513c4e6134f022a85b1ffdd59b2a1'

// Setup web3 with Infura
const web3 = new Web3(new Web3.providers.HttpProvider('https://mainnet.infura.io/'));
web3.eth.accounts.wallet.add(bobPrivateKey) // Add private to web3 for signing

// Setup the LQDManager
const lqdManager = new LQDManager({
  rpcApi: web3,
  hubApiUrl: 'https://public.liquidity.network/', // TODO UPDATE
  contractAddress: '0xad471Bde2303f2f43325b2108D26f1eAbA1e32b', // TODO UPDATE
});

function async sendToALice() {

  // Register an address to be used with the LQD manager
  await lqdManager.register(bob)

  // Send 0.01 Ether off-chain to Alice  
  const txId = await lqdManager.sendTransfer({
      to: alice,
      amount: web3.utils.toWei(0.01,'ether'), // Amount in wei as BigNumber
      from: bob,
   });

  console.log("Transfer to Alice sent ! Transaction ID: ", txId)

}

sendToALice()

```



NOCSUT hubs currently require recipients of transfers to be online and to sign a message to receive an off-chain transfer.  The library once setup, after calling the `register` function, will automatically accept transfers. Alice needs to setup a lqdManager with her private key as follow:



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
  hubApiUrl: 'https://public.liquidity.network/', // TODO UPDATE
  contractAddress: '0xad471Bde2303f2f43325b2108D26f1eAbA1e32b', // TODO UPDATE
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

Note that soon we will release an upgrade of the protocol to allow passive accept. This means that no specific actions will be required from the recipient in order to receive a transfer.  



### ERC-20 token transfers

NOCUST hubs support off-chain transfers of ERC-20 tokens. However, the operator chooses which tokens could be used.  To see which tokens are supported by a hub call `getSupportedTokens`:

```typescript
const supportedTokenArray : string[] = await lqdManager.getSupportedTokens()
const NocustContract : string = supportedTokenArray[0]
const tokenXYZcontract : string = supportedTokenArray[1]
```

`supportedTokenArray` will contain an array of ERC-20 smart-contract addresses available for use.

The address at index 0 is the address of the NOCUST smart-contract. This simply means that the hub support Ether transfers.



We need to specify to the `register` function what token we want to use:

```typescript
await lqdManager.register(bob, tokenXYZcontract)
// Bob can receive Ether and the token at the address `tokenXYZcontract`
```



The register function always register by default at least Ether. Additionally it registers for the tokens passed in second parameter. A single token address can be passed or an array of token addresses.  Note that the recipient will also have to `register` the token.



To make a token transfer simply specify the address of the token in the `tokenAddress` field.

```typescript
  const txId : number = await lqdManager.sendTransfer({
      to: alice,
      amount: web3.utils.toWei(0.01,'ether'), // Amount
      from: bob,
      tokenAddress: tokenXYZ,
   });
```



### Deposits



In order to make transfers you first needs some off-chain funds. Off-chain funds are simply funds deposited into the NOCUST smart-contract. The library provides a function to deposit funds into the hub contract.



```typescript
const transactionHash : string = await lqdManager.deposit(
  bob,							 // Account from which to make a deposit (Its private key needs to be in the Web3 instance)
  web3.utils.toWei(0.5,'ether'), // Amount to deposit
  web3.utils.toWei(10,'gwei'),   // Gas price, 10 Gwei
  150000						 // Gas Limit
);
```



This function make an on-chain contract call to the NOCUST contract and sending the specified amount to the contract. If the transaction succeed, after `60` confirmation blocks the funds will be available in the off-chain account. To verify the off-chain balance call the `getOffChainBalance` function:

```typescript
const balance : BigNumber = await lqdManager.getOffChainBalance(bob);
console.log("Bob's off-chain balance is: ", balance.toString())
```



All these functions can be used with a parameter `tokenAddress` to manipulate similarly ERC-20 tokens.



### Withdrawals



If the user wishes to send its off-chain funds back on-chain (Also call an exit) he needs to do a withdrawal. Withdrawals take longer time and are a 2 step process requiring 2 contract calls.  



The amount of off-chain funds available for withdrawals may differ from the current off-chain balance. Recently acquired off-chain funds cannot be withdrawn directly, they will be made fully available over time.  Recently acquired funds will need between 36h and 72h (one full round) to be available.  To check the current the current balance available for withdraw the function `getWithdrawalLimit` :



```typescript
const withdrawalLimit : BigNumber = lqdManager.getWithdrawalLimit(bob)
```



Any amount inferior or equal to this amount can be withdraw.



To initiate a withdrawal the function ` withdrawalRequest` need to be used :

```typescript
const transactionHash : string = await lqdManager.withdrawalRequest(
  bob,							 // Account from which to make the withdrawal
  web3.utils.toWei(0.5,'ether'), // Amount to withdraw
  web3.utils.toWei(10,'gwei'),   // Gas price, 10 Gwei
  300000						 // Gas Limit
);
```

This will make a contract call to initiate a withdrawal.



The funds don't get transfered back to bob address directly.  It is needed to wait a period of time between 36h and 72h (again one full round).  To check how much time is left before we can confirm the withdrawal the function `getBlocksToWithdrawalConfirmation` can be used:

```typescript
const blocksToConfirmation : number = await lqdManager.getBlocksToWithdrawalConfirmation(bob)
```



This function returns the number of block to wait before we can send the confirmation transaction. If it returns `0`  the withdrawal is ready for confirmation .  Note that the function will return `-1` if there is no withdrawal pending.



Finally, to confirm the withdrawal we need to do:

```typescript
const transactionHash : string = await lqdManager.withdrawalRequest(
  bob,							 // Account from which to make the withdrawal
  web3.utils.toWei(10,'gwei'),   // Gas price, 10 Gwei
  300000						 // Gas Limit
);
```



This contract call will effectively transfer the funds from the NOCUST smart contract to bob address.
