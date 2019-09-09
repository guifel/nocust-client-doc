# Getting Started

## Intro

Liquidity Network's underlying technology are NOCUST commit-chains, a layer 2 solution to scale blockchains such as Ethereum and it works today on the mainet!

A commit-chain is a _chain of commits_ ⛓️, that means that NOCUST is committing regularly, every _round_, a commit of the commit-chain to the parent Ethereum chain. The commit-chain is run by a **non-custodial hub**, or operator, and clients communicate with the hub.

Contrary to side-chains, commit-chains don't need an additional consensus mechanism and rely on the security of the parent chain \(here Ethereum\). We refer the interested developer to a basic introduction of [basic introduction of NOCUST](https://blog.liquidity.network/2018/11/21/nocust-101/), we provide extensive details in our [background section](background.md), and for the formal geeks [we provide a paper](https://eprint.iacr.org/2018/642.pdf) 🤓 .

For developers, we start by describing the JavaScript client library for NOCUST 📱 \(how to make transactions\).

## NOCUST JavaScript Client library 📱

The `nocust-client` allows you to interact with the NOCUST 🌊 commit-chain.

In this document, we describe the client that allows developers to build wallets or dApps with _full commit-chain capabilities_. The client enables you to:

* Deposit \(convert Ethereum ➡️ NOCUST coins\)
* Withdraw \(convert NOCUST coins ➡️ Ethereum\)
* Make payments from address A ➡️ B
* Make atomic swaps between address A ↔️ B

The client currently supports Ether or ERC-20 tokens. Once Ether or tokens are on a NOCUST commit-chain, we refer to them as fast, free \(and furious? 😛\) assets, for example fETH, or fTOKEN. Transactions on the NOCUST commit-chain **cost zero gas** and are **instant** enabling plenty new use-cases 😲. The client internally ensures the security of the NOCUST wallet by monitoring the smart-contract of the NOCUST operator \(Henry\) and the state of the NOCUST commit-chain.

The following figure illustrates the diffrent roles of each component in a NOCUST commit-chain. Bob is the one running the NOCUST client to interact with Henry, the NOCUST operator and the smart contract.

![NOCUST setup](.gitbook/assets/setup%20%281%29.png)

## Installation

To install the NOCUST JavaScript API, simply run:

```text
➜ npm install nocust-client
```

The client requires Web3 \(version 1.0.0-beta.37 only for now\) to interact with the Ethereum Network. Additionally, as we are manipulating exclusively Ether amounts in Wei \(10^-18 Ether\), we use the `bignumber.js` library for Ether and token amounts \([to go beyond the Javascript safe limit](https://stackoverflow.com/questions/307179/what-is-javascripts-highest-integer-value-that-a-number-can-go-to-without-losin)\).

Required dependencies to be installed:

```text
➜ npm install web3@1.0.0-beta.37 bignumber.js
```

For typescript users we recomment the following configuration in your `tsconfig.json` file.

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

## Currently Deployed NOCUST commit-chains

### Public test network

The following tables show the currently deployed NOCUST instances available for use.

| Ethereum Mainnet Hub |  |
| :--- | :--- |
| NOCUST smart-contract address \(`contractAddress`\) | 0x83aFD697144408C344ce2271Ce16F33A74b3d98b |
| Hub API URL \(`hubApiUrl`\) | [https://public.liquidity.network/](https://public.liquidity.network/) |
| LQD ERC-20 contract address | 0xD29F0b5b3F50b07Fe9a9511F7d86F4f4bAc3f8c4 |

| Ethereum Mainnet XCHF only Hub |  |
| :--- | :--- |
| NOCUST smart-contract address \(`contractAddress`\) | 0x13490D2143Dd9f2Bb30925710cEBd6CA44a594cB |
| Hub API URL \(`hubApiUrl`\) | [hrrps://xchf.liquidity.network/](%20hrrps://xchf.liquidity.network/) |
| XCHF ERC-20 contract address | 0xB4272071eCAdd69d933AdcD19cA99fe80664fc08 |

| Rinkeby Testnet Hub | Value |
| :--- | :--- |
| NOCUST smart-contract address \(`contractAddress`\) | 0x66b26B6CeA8557D6d209B33A30D69C11B0993a3a |
| Hub API URL \(`hubApiUrl`\) | [https://rinkeby.liquidity.network/](http://rinkeby.liquidity.network/) |
| Test ERC-20 contract address | 0xA9F86DD014C001Acd72d5b25831f94FaCfb48717 |

### Private test network

The following is a test NOCUST on a private blockchain with a shorter block interval \(6 seconds instead of 15 seconds\) and a shorter checkpoint round time \(6 minutes instead of 36 hours\). This allows developers to test features much faster than waiting for long round times 😁.

Please do initiate Web3 with a HTTP provider given the RPC URL provided in the following table.

| Limbo test commit-chain |  |
| :--- | :--- |
| NOCUST smart-contract address \(`contractAddress`\) | 0x9561C133DD8580860B6b7E504bC5Aa500f0f06a7 |
| Hub API URL \(`hubApiUrl`\) | [https://limbo.liquidity.network/](https://limbo.liquidity.network/admission/) |
| Test ERC-20 contract address | 0xe982E462b094850F12AF94d21D470e21bE9D0E9C |
| Ethereum node RPC URL | [https://limbo.liquidity.network/ethrpc](https://limbo.liquidity.network/ethrpc) |

Note that the commit-chain and the Limbo blockchain are completely reset every 24H. To get some Ether on Limbo you can use the default Ganache accounts that owns Ether and test ERC-20: 

| Owns | Public Key | Private Key \(!! Test purpose only !!\) |
| :--- | :--- | :--- |
| Ether & Test ERC20 | 0x90F8bf6A479f320ead074411a4B0e7944Ea8c9C1 | 0x4f3edf983ac636a65a842ce7c78d9aa706d3b113bce9c46f30d7d21715b23b1d |
| Ether | 0xFFcf8FDEE72ac11b5c542428B35EEF5769C409f0 | 0x6cbed15c793ce57650b9877cf6fa156fbef513c4e6134f022a85b1ffdd59b2a1 |
| Ether | 0x22d491Bde2303f2f43325b2108D26f1eAbA1e32b | 0x6370fd033278c143179d81c5526140625662b8daa446c22ee2d73db3707e620c |
| Ether | 0xE11BA2b4D45Eaed5996Cd0823791E0C93114882d | 0x646f1ce2fdad0e6deeeb5c7e8e5543bdde65e86029e2fd9fc169899c440a7913 |
| Ether | 0xd03ea8624C8C5987235048901fB614fDcA89b117 | 0xadd53f9a7e588d003326d1cbf9e4a43c061aadd9bc938c843a79e7b4fd2ad743 |

### NOCUST Transfer - Full Example

We offer two out of the box examples 😍

#### `Browser` Example

Try [here](https://owallet.liquidity.network) a very simple browser wallet. It will automagically register two new wallets with the hub, and make a transfer among them. See the source code on [GitHub](https://github.com/liquidity-network/liquidity-browser-app).

#### `Node` Example

The following JavaScript code sets up the client and transfers 0 fETH 🤪 from Bob 🙋‍♂️ to Alice 🙋‍♀️.

To execute the example code below, follow those steps:

```text
➜ npm install nocust-client    
➜ npm install web3@1.0.0-beta.36 bignumber.js
➜ # copy and paste the code above into `test.js`
➜ node test.js
Transfer to Alice sent ! Transaction ID:  504
```

We tested this example with `npm` version 5.7.1 and `node` version 8.5.0.

```typescript
const Web3 = require('web3') // Web3 1.0.0-beta.37 only for now
const BigNumber = require('bignumber.js')
const { NOCUSTManager } = require('nocust-client')

// Setup web3 with Infura
const web3 = new Web3(new Web3.providers.HttpProvider('https://rinkeby.infura.io/'));

// create 2 new wallets
const wallets = web3.eth.accounts.wallet.create(2);
const BOB_PUB = wallets[0].address
const BOB_PRIV =  wallets[0].privateKey
const ALICE_PUB = wallets[1].address
const ALICE_PRIV = wallets[1].privateKey

// Specify to which commit-chain we want to connect
const nocustManager = new NOCUSTManager({
  rpcApi: web3,
  operatorApiUrl: 'https://rinkeby.liquidity.network/',
  contractAddress: '0x66b26B6CeA8557D6d209B33A30D69C11B0993a3a',
  });

const sendToALice = async () => {

  // Register Alice and Bob with the commit-chain. This is required to be done at least once per address in order to receive commit-chain transaction. 
  // Note that the registration is done implicitly when sending your first transfer. 
  await nocustManager.registerAddress(BOB_PUB)
  await nocustManager.registerAddress(ALICE_PUB)

  // Send 0.00 fETH on the commit-chain to Alice  
  // In this example, we send 0 fETH, because Alice doesn't have any funds yet, and yes, we can send 0-value commit-chain transaction, haha
  const txId = await nocustManager.sendTransaction({
      to: ALICE_PUB,
      // 0.00 fEther in Wei as BigNumber. 
      amount: (new BigNumber(0.00)).shiftedBy(-18),
      from: BOB_PUB,
   });
  console.log("Transfer to Alice sent ! Transaction ID: ", txId)
}

sendToALice()
```

### Deposits \(Ethereum ➡️ NOCUST\)

To make transfers, you need to have NOCUST funds. NOCUST funds are simply funds deposited into the NOCUST smart-contract, which can be done through the client as follows.

```typescript
const transactionHash = await nocustManager.deposit(
  BOB_PUB,                       // Account from which to make a deposit (its private key needs to be in the Web3 instance)
  web3.utils.toWei(0.5,'ether'), // Amount to deposit
  web3.utils.toWei(10,'gwei'),   // Gas price, 10 Gwei
  150000                         // Gas Limit
);
```

The function `deposit()` makes a contract call to the NOCUST smart contract with the specified amount. The commit-chain funds are available after `20` block confirmation. To check your NOCUST balance, you can call the `getNocustBalance()` function. Note that `deposit()`and `getNocustBalance()` take a parameter `tokenAddress` to similarly manipulate ERC-20 tokens.

```typescript
const balance : BigNumber = await nocustManager.getNOCUSTBalance(bob);
console.log("Bob's commit-chain balance is: ", balance.toString(), " wei")
```

⚠️ Don't forget to provide a [transfer allowance](https://medium.com/ethex-market/erc20-approve-allow-explained-88d6de921ce9) to the NOCUST contract for the ERC-20 you wish to use. You can also use the function `approveAndDeposit()` that will check for allowance and make an approve call prior to the deposit if necessary.

### NOCUST Transfers \(🙋‍♂️ ➡️ 🙋‍♀️\)

NOCUST transfers are free of gas and instant! See[ the example above](./#nocust-transfer-full-example) to learn how to simply make an Ether transfer. 

#### Listening for Incoming Transfers

If you want to trigger a special event upon an incoming transaction, you can define the following catcher:

```typescript
const Web3 = require('web3') // Web3 1.0.0-beta.36 only for now
const { NocustManager } = require('nocust-client')

// Setup web3 with Infura
const web3 = new Web3(new Web3.providers.HttpProvider('https://rinkeby.infura.io/'));

// create 2 new wallets
const wallets = web3.eth.accounts.wallet.create(1);
const ALICE_PUB = wallets[0].address
const ALICE_PRIV = wallets[0].privateKey

// Setup the LQDManager
const nocustManager = new NOCUSTManager({
  rpcApi: web3,
  operatorApiUrl: 'https://rinkeby.liquidity.network/',
  contractAddress: '0x66b26B6CeA8557D6d209B33A30D69C11B0993a3a',
});

const register = async () => {
  // First insure that the address is registered
  await nocustManager.register(ALICE_PUB)

  // Trigger a log upon an incoming transfer
  nocustManager.subscribeToIncomingTransfer(
    alice, 
    (transfer) => console.log(`Alice is receiving a transfer of  ${transfer.amount} wei from ${transfer.wallet.address}`)
  )

  console.log("Alice is ready to receive transfers !")
}
register()
```

#### NOCUST ERC-20 Transfers

NOCUST 🌊 support the transfer of ERC-20 tokens. The hub decides which tokens can be used on NOCUST. To see which tokens are currently supported by a NOCUST hub, call `getSupportedTokens()`:

```typescript
const supportedTokenArray = await nocustManager.getSupportedTokens()

// The first element in the array is for Ether token, its token address it set to the nocust smart-contract address.
const NocustContract = supportedTokenArray[0].tokenAddress

// Following elements are the tokens supported by the commit-chain 
const tokenXYZcontract = supportedTokenArray[1].tokenAddress
const tickerName = supportedTokenArray[1].shortName
```

`supportedTokenArray` contains an array of objects containing the information about the ERC-20 tokens supported by the commit-chain. The element at index 0 is for Ether \(reflecting the fETH on the NOCUST commit-chain\).

With the help of the `registerAddress()` function, we can register the tokens we want to use:

```typescript
await registerAddress.register(ALICE_PUB, tokenXYZcontract)
// Bob can receive fETH and the fToken at the address `tokenXYZcontract`
```

Note, that the recipient needs to have register at least once in the past with the commit-chain for the specific token prior receiving any transaction. To make a fToken transfer, simply specify the address of the fToken in the `tokenAddress` field.

```typescript
  const txId = await registerAddress.sendTransaction({
      to: ALICE_PUB,
      amount: web3.utils.toWei(0.01,'ether'), // Amount
      from: BOB_PUB,
      tokenAddress: tokenXYZcontract,
   });
```

### Withdrawals \(NOCUST ➡️ Ethereum\)

A withdrawal allows the user to send NOCUST funds back to Ethereum \(also called an exit\). Withdrawals take time ⌛ and are a two-step process requiring two separate contract calls.

The amount of NOCUST funds available for withdrawal may differ from the current NOCUST balance. Recently acquired NOCUST funds cannot be withdrawn instantly, and need between 36h and 72h \(one full round\) to be available. To check the current balance available for withdrawal call the function `getWithdrawalLimit()` :

```typescript
const withdrawalLimit = nocustManager.getWithdrawalLimit(bob)
```

To initiate a withdrawal, call `withdrawalRequest()` with an amount &lt;= `withdrawalLimit`:

```typescript
const transactionHash = await nocustManager.withdrawalRequest(
  BOB_PUB,                       // Account from which to make the withdrawal
  web3.utils.toWei(0.5,'ether'), // Amount to withdraw
  web3.utils.toWei(10,'gwei'),   // Gas price, 10 Gwei
  300000                         // Gas Limit
);
```

This makes a contract call to initiate a withdrawal. After 36h to 72h \(corresponding to one full NOCUST round\), the withdrawal needs to be confirmed. To query how much time is left before the withdrawal can be confirmed you can call `getBlocksToWithdrawalConfirmation()`:

```typescript
const blocksToConfirmation = await nocustManager.getBlocksToWithdrawalConfirmation(BOB_PUB)
```

`getBlocksToWithdrawalConfirmation()` returns the number of block confirmations required before to confirm a withdrawal. If the function returns `0`, the withdrawal is ready for confirmation. Note that the function will return `-1` if there is no withdrawal pending.

Finally, to confirm the withdrawal, you can call `withdrawalConfirmation()`:

```typescript
const transactionHash = await nocustManager.withdrawalConfirmation(
  BOB_PUB,                       // Account from which to make the withdrawal
  web3.utils.toWei(10,'gwei'),   // Gas price, 10 Gwei
  300000                         // Gas Limit
);
```

This contract call transfers the funds from the NOCUST smart contract to Bob's address.

