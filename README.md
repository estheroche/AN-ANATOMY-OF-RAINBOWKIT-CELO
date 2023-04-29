# Building a Smart Contract with Celo Rainbowkit

## Table of Contents

- [Building a Smart Contract with Celo Rainbowkit]()
- [Introduction](#introduction)
  .[What is Celo?](#what-is-celo?)
  .[What is Celo-Rainbow-kit](#what-is-celo-rainbow-kit?)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)

  . [Step 1: Setting up the Rainbow CLI](#step-1-setting-up-the-rainbow-cli)

  . [Step 2: Connecting the Rainbowkit](#connecting-the-rainbowkit)

  . [Step 3: Creating a Smart Contract](#creating-a-smart-contract)

  . [Step 4: Compiling and Deploying the Smart Contract](#compiling-and-deploying-the-smart-contract)

- [Conclusion](#conclusion)

## Introduction

Decentralized applications are becoming increasingly popular, and smart contracts are at the core of these applications. Celo is a platform that makes it easy for developers to build and deploy decentralized applications. RainbowKit is a JavaScript library that provides a simple and convenient way to interact with Ethereum-based blockchains, including Celo. Hardhat is a development environment for building and testing Ethereum-based applications. In this tutorial, we'll walk through the process of building a simple smart contract using RainbowKit with Hardhat on Celo.

### What is Celo?

Celo is a mobile-first blockchain with a focus on building an accessible, green financial system. In response to the numerous difficulties that well-known blockchains like Bitcoin and Ethereum are experiencing, the Celo blockchain was created. Currency with a Steady Value The price volatility of cryptocurrencies is a significant barrier to their widespread use as a means of exchange and accessibility. A expanding family of stablecoins, including cUSD, cEUR, cREAL, and USDC, are offered by Celo. These coins have a stable value and can even be used to pay transaction fees. The consensus processes used by Proof-of-Stake Bitcoin take a great deal of energy and result in expensive and frequently delayed transactions.
By mapping customers' phone numbers to their public keys, Celo enables users to send cryptocurrency using only their phone numbers. EVM Compatibility in Full In addition to allowing programmers to use the well-liked Solidity smart contract language, Celo is fully EVM compliant. This means that ERC-20 or ERC721-compliant tokens and Ethereum's tooling can both be used in the Celo ecosystem (NFT).

### What is Celo-Rainbow-kit

A development kit called RainbowKit-celo enables programmers to create decentralized applications (dApps) quickly and easily on the Celo blockchain. An open-source blockchain technology called Celo wants to build an inclusive financial system. On top of Celo, RainbowKit-celo offers developers a collection of tools and resources to create dApps that can communicate with the Celo blockchain.
A development kit called RainbowKit-Celo enables programmers to create and launch decentralized applications (dApps) on the Celo network. It offers a full suite of tools and resources to developers, streamlining the development process and lowering the time and effort required to create dApps on Celo.

RainbowKit has several advantages that make it an attractive choice for developers building smart contracts on the Celo network. Here are a few of its benefits:

1. Ease of Use: RainbowKit is designed to be easy to use, even for developers who are new to smart contract development. It includes a number of helpful features, such as a built-in development environment, that make it easy to get started.

2. Security: RainbowKit is designed with security in mind. It includes a number of built-in security features, such as contract testing tools, that can help developers catch potential security vulnerabilities before they become a problem.

3. Compatibility: RainbowKit is designed to be compatible with a wide range of smart contract platforms, including Celo. This makes it a versatile tool that can be used to build smart contracts for a variety of use cases.

4. Community Support: RainbowKit has a strong and active community of developers who contribute to its development and provide support for users. This can be especially helpful for new developers who are learning the ropes of smart contract development.

## Prerequisites

Before we get started, here are the prerequisites:

1.  A computer running Windows, macOS, or Linux

2.  A Celo account and wallet (you can create one at celo.org)

3.  Basic knowledge of JavaScript and Solidity

4.  An internet connection

## Getting Started

### Step 1: Setting up the Rainbow CLI

The first step is to set up the Rainbow CLI, which is a command-line interface that enables you to interact with the Rainbowkit and deploy smart contracts to the Celo blockchain. To install the Rainbow CLI, follow these steps:

1. Install Node.js and NPM (if you haven't already).
2. Open a terminal window and run the following command to install the Rainbow CLI:

```bash
npm install -g @celo/celocli
```

3. Once the installation is complete, run the following command to log in to your Celo account:

```bash
celocli account:login
```

4. Follow the prompts to enter your account information and set up your account.

### Step 2: Connecting the Rainbowkit

The next step is to connect the Rainbowkit to your computer. To do this, follow these steps:

1. Connect the Rainbowkit to your computer using the USB cable.
2. Open a terminal window and run the following command to verify that the Rainbowkit is connected: run the command below;

`bash
celocli rainbow:status`

If the Rainbowkit is connected, you should see a message indicating that the Rainbowkit is connected and the firmware version.

### Step 3: Creating a Smart Contract

Now that you have a Hardhat project set up, you need to create a new smart contract.

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.9;

contract CohortBank {

    uint256 public unlockTime;
    mapping(address => uint256) public balances;
    uint256 public totalCohortBalance;

    address payable public owner;

    event Deposit(uint256 amount, uint256 when, address caller);
    event Withdrawal(uint256 amount, uint256 when);


    constructor(uint256 _unlockTime) payable {
        require(
            block.timestamp < _unlockTime,
            "Unlock time should be in the future"
        );

        unlockTime = _unlockTime;
        owner = payable(msg.sender);
    }

  function deposit() public payable {
         unlockTime, block.timestamp);
        require(msg.value > 0, "cannot deposit 0 amount");
        balances[msg.sender] += msg.value;
        totalCohortBalance += msg.value;

        emit Deposit(msg.value, block.timestamp, msg.sender);
    }

    function withdraw() public {
        require(block.timestamp >= unlockTime, "You can't withdraw yet");
        require(msg.sender == owner, "You aren't the owner");

        emit Withdrawal(address(this).balance, block.timestamp);

        owner.transfer(address(this).balance);
    }
}
```

This is a smart contract written in Solidity, a programming language used for creating smart contracts on the Ethereum blockchain. Let's go through the code and understand it step by step:

`contract CohortBank {`

This line declares a new contract called "CohortBank".

`uint256 public unlockTime;`

This line declares a public state variable called "unlockTime" of type uint256, which will hold the timestamp when the contract will be unlocked and withdrawals will be allowed.

`mapping(address => uint256) public balances;`

This line declares a public mapping called "balances" which will store the balances of each address that deposits funds into the contract.

`uint256 public totalCohortBalance;`

This line declares a public state variable called "totalCohortBalance" of type uint256, which will store the total amount of funds deposited by all addresses.

`address payable public owner;`

This line declares a public state variable called "owner" of type address payable, which will hold the address of the contract owner who can withdraw the funds.

`event Deposit(uint256 amount, uint256 when, address caller);`

This line declares an event called "Deposit", which will be emitted whenever someone deposits funds into the contract. It will contain the amount deposited, the timestamp of the deposit, and the address of the caller.

`event Withdrawal(uint256 amount, uint256 when);`

This line declares an event called "Withdrawal", which will be emitted whenever the owner withdraws funds from the contract. It will contain the amount withdrawn and the timestamp of the withdrawal.

`constructor(uint256 _unlockTime) payable {
    require(
block.timestamp < _unlockTime,
 "Unlock time should be in the future"
    );`

`unlockTime = _unlockTime;
owner = payable(msg.sender);
}`

This is the constructor function which is called when the contract is deployed. It takes an argument called "\_unlockTime" which will set the unlock time for the contract. It also checks that the unlock time is in the future, and sets the "unlockTime" and "owner" variables.

`function deposit() public payable {
    require(msg.value > 0, "cannot deposit 0 amount");
    balances[msg.sender] += msg.value;
    totalCohortBalance += msg.value;`

`emit Deposit(msg.value, block.timestamp, msg.sender);
}`

This function allows anyone to deposit funds into the contract. It requires that the deposited amount is greater than zero, updates the balances of the depositor and the total cohort balance, and emits a "Deposit" event.

`function withdraw() public {
    require(block.timestamp >= unlockTime, "You can't withdraw yet");
    require(msg.sender == owner, "You aren't the owner");`

`emit Withdrawal(address(this).balance, block.timestamp);`

` owner.transfer(address(this).balance);
}`

This function allows the owner to withdraw funds from the contract. It requires that the unlock time has passed and that the caller is the owner. It emits a "Withdrawal" event, and transfers the entire balance of the contract to the owner's address.

## Step 3: COMPILE AND DEPLOY SMART CONTRACT

Now that our smart contract is ready, we can compile and deploy it to the Celo network. To do this, we will use Hardhat, which is a development environment for building, testing, and deploying smart contracts.

a. Compile the Smart Contract

In your terminal, run the following command to compile the smart contract:

```solidity
        npx hardhat compile
```

This command will create a build directory containing the compiled smart contract. You can view the compiled smart contract in the build/contracts directory.

b. Deploy the Smart Contract

To deploy the smart contract, we need to configure Hardhat to connect to the Celo network. Create a .env file in the root directory of your project and add the following variables:

```
  PRIVATE_KEY=<your_private_key>
  CELO_RPC=<celo_rpc_endpoint>
```

Replace <your_private_key> with the private key you generated earlier, and <celo_rpc_endpoint> with the RPC endpoint of the Celo network you want to deploy to.

In this tutorial, we will use the alfajores test network, so the CELO_RPC variable should be set to:

```dotnetcli
 https://alfajores-forno.celo-testnet.org
```

Next, update the hardhat.config.js file to use the Celo network. Add the following code to the networks object:

```solidity
celo: {
  url: process.env.CELO_RPC,
  chainId: 44787,
  gas: 6000000,
  gasPrice: 20000000000, // 20 gwei
  accounts: [`0x${process.env.PRIVATE_KEY}`]
}
```

This code configures Hardhat to use the Celo network with the CELO_RPC endpoint and the PRIVATE_KEY variable.

Now, let's deploy the smart contract to the Celo network. In your terminal, run the following command:

```bash
npx hardhat run --network celo scripts/deploy.js
```

This command will deploy the smart contract to the Celo network and output the address of the deployed contract. Save this address as we will need it in the next step.

Now that the smart contract is deployed, we can interact with it using the Celo wallet.

a. Connect to the Celo Wallet

Open the Celo wallet on your browser and connect to the alfajores test network. You can do this by clicking on the network dropdown in the top-right corner of the wallet and selecting Alfajores.

b. Add the Smart Contract

Click on the Add Contract button on the left sidebar of the wallet. Enter the following details:

```
 Contract Address: The address of the deployed smart contract.
 Contract Name: CohortBank
 Contract ABI: The JSON ABI of the smart contract. You can find this in the build/contracts directory.
```

Click on Add Contract to add the smart contract to your wallet.

c. Interact with the Smart Contract

Now that the smart contract is added to your wallet, you can interact with it by clicking on the contract in the left sidebar.

You can deposit funds to the smart contract by clicking on the deposit button and entering the amount of funds you want to deposit.

You can also withdraw funds from the smart contract by clicking on the withdraw button. However, you can only withdraw funds after the unlock time has passed and you are the owner of the smart contract.

Congratulations! You have successfully built a smart contract using RainbowKit and deployed it to the Celo network.

## Step 4: CONCLUSION

RainbowKit-celo can assist developers in quickly and efficiently building and deploying decentralized applications on the Celo blockchain thanks to testing and development, a streamlined deployment process, and a suite of command-line tools.

RainbowKit-celo also offers a number of other features and integrations, such as:

Using the Celo Wallet integration will make managing your accounts simple.
backing for the on-chain governance structure used by Celo
Using IPFS integration for decentralized storage
Integration with OpenZeppelin library for the creation of safe smart contracts

For developers wishing to create decentralized applications just on Celo network, RainbowKit-celo is a strong tool overall.

For developers looking to create decentralized applications for the Celo network, RainbowKit-Celo is a potent tool. Developers may quickly and simply create dApps that take advantage of the capabilities of the Celo blockchain thanks to its pre-built templates, SDK, and interaction with the Celo Wallet.

The primary characteristics of RainbowKit-Celo, such as the pre-built templates, the Celo SDK, and the interface with the Celo Wallet, were discussed in this article. To help you understand how RainbowKit-Celo can be used to create a dApp on the Celo network, we have also supplied some code excerpts.

Check out RainbowKit-Celo if you're thinking about developing a dApp for the Celo network, and get to work on it right now!!
