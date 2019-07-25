---
description: >-
  How to create commit-chain invoices (QR codes and links) to easily track
  payments for merchants and better shopping experience for the users.
---

# Invoicing

We provide a separated [npm](https://www.npmjs.com/package/liquidity-invoice-generation) package to generate and manipulate commit-chain invoices. Invoices are an easy way to integrate Liquidity Network payment solution for merchants and provide a better shopping experience for users.

#### Install

```
npm install liquidity-invoice-generation
```

#### Usage

```typescript
const invoice: Invoice = createInvoice({
  network: 4, // Network id
  // Public key of recipient
  publicKey: '0xE6987CD613Dfda0995A95b3E6acBAbECecd41376',
  
  // Generate unique id for the invoice, payment by this invoice can be tracked by nonce.
  // If this is set to true, the invoice is payable only once. Otherwise the invoice is payable multiple-times.
  generateId: true,
  
  // Nocust contract address, if not defined, on-chain invoice will be generated.
  contractAddress: '0x4FED1fC4144c223aE3C1553be203cDFcbD38C581',
  
  // Token address of the payment, if not defined, Ether is used.
  tokenAddress: '0x4FED1fC4144c223aE3C1553be203cDFcbD38C581',
  
  // Optional. BigNumber or number also can be used.
  amount: '12000000000000',
})

// Encode the invoice as a compact string to displayed as QR code.  
const encodedInvoice: string = encodeInvoice(invoice)

// Decode invoice after QR code scanning
const decodedInvoice: Invoice = decodeInvoice(encodedInvoice)

// Note: Here, decodedInvoice equals invoice
```

* Feed the output of `encodeInvoice` into a QR code generator as plain text. \(i.g: use [https://www.the-qrcode-generator.com](https://www.the-qrcode-generator.com) or a QR library\) to be scanned by the user.
* Feed to output of `decodInvoice` into the `sendTransfer` function of the NOCUSManager. 
* If no amount is specified `invoice.amount` will be undefined, it needs to be set before making a transfer with the nocust manager.
* If tokenAddress is not specified, Ether will be used.
* To make an on-chain transactions invoice don't set a contract address. 

The typical merchant needs to track the completion of payments by its customers. We use`generateId`.

1. SERVER SIDE, generate an invoice with `generateId` set to `true` and save the nonce in the invoice object `invoice.nonce` in the merchant server database. It is important this is done server side. Otherwise, the client can manipulate the value. 
2. Encode the invoice with the `encodeInvoice` function and send it to the client front-end to display the QR code that will have to be scanned by the developer's mobile wallet. 
3. Wait for an incoming payment with the nocust-manager that has the correct nonce value AND amount.

#### Liquidity wallet deeplink invoice

It is possible to create a deeplinks invoices that will automatically open and fill the invoice details on the Liquidity Mobile Wallet on [Android](https://play.google.com/store/apps/details?id=com.liquiditynetwork.wallet) and [iOS](https://itunes.apple.com/ch/app/liquidity-network-wallet/id1395924630). Use the URL `lqdnet://send` and set the query parameters identical to the fields outputted by `createInvoice`

Example:

```text
lqdnet://send?network=4&publicKey=0x688...0E2&tokenAddress=0xA9F...717&amount=100000000
```

