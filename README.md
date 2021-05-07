# Polygon Matic Box

- [Requirements](#requirements)
- [Installation](#installation)
- [Setup](#setup)
- [Overview](#overview)
- [Matic PoS](#matic-pos-chain)
  * [Compiling](#compiling)
  * [Migrating](#migrating)
  * [Paying For Migrations](#paying-for-migrations)
  * [Testing](#testing)
  * [New Configuration File](#new-configuration-file)
  * [New File Structure for Artifacts](#new-file-structure-for-artifacts)
  * [Communication Between Ethereum and Matic PoS Chains](#communication-between-ethereum-and-matic-pos-chains)
- [Support](#support)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


This Truffle Polygon Matic box provides you with the boilerplate structure necessary to start coding for Polygon's Ethereum L2 solution, the Matic PoS chain. For detailed information on how Matic works, please see their documentation [here](https://docs.matic.network/docs/develop/getting-started).

As a starting point, this box contains only the SimpleStorage Solidity contract. Including minimal code was a conscious decision as this box is meant to provide the initial building blocks needed to get to work on Matic without pushing developers to write any particular sort of application. With this box, you will be able to compile, migrate, and test Solidity code against several instances of Matic networks.

Matic's L2 solution is fully compatible with the EVM. This means you will not need a new compiler to deploy Solidity contracts. The main difference developers will encounter is in accessing and interacting with the Matic PoS network. Additionally, Polygon offers multiple ways for dapp developers to implement communication between Ethereum ("Layer 1") and Matic. Further information about how to enable Ethereum-Matic communication can be found in the Matic documentation [here](https://docs.matic.network/docs/develop/ethereum-matic/getting-started).

## Requirements

The Polygon Matic box has the following requirements:

- [Node.js](https://nodejs.org/) 10.x or later
- [NPM](https://docs.npmjs.com/cli/) version 5.2 or later
- Windows, Linux or MacOS

## Installation

> Note that this installation command will only work once the box is published (in the interim you can use `truffle unbox https://github.com/truffle-box/polygonMatic-box`).

```bash
$ truffle unbox polygonMatic
```

## Setup

You will need at least one mnemonic to use with an optimistic network. A `.env` file has been provided for you, and you should populate it with the appropriate mnemonic. The `truffle-config.matic.js` file expects a `MNEMONIC` value to exist in `.env` for running migrations on the networks listed in `truffle-config.matic.js`


## Overview

The code here will allow you to compile, migrate, and test your code against a Matic network instance. The following commands can be run (more details on each can be found in the next section):

 To compile:
 ```
 npm run compile:matic
 ```

 To migrate:
 ```
 npm run migrate:matic --network=(mit | mim | mt| mm)
 ```

 To test:
 ```
 npm run test:matic --network=(mit | mim | mt | mm)
 ```


## Matic PoS Chain


### Compiling

You do not need to add any new compilers or settings to compile your contracts for the Matic PoS chain, as it is fully EVM compatible. However, you may wish to distinguish between contracts that will be deployed to the Matic chain and those you wish to deploy to Ethereum. With this in mind, the Polygon Matic box has distinguished two contracts directories. The `truffle-config.matic.js` configuration file indicates the contract and build paths for Matic-destined contracts.

If you are compiling contracts specifically for the Matic network, use the following command, which indicates the appropriate configuration file:

```
npm run compile:matic
```

If you would like to recompile previously compiled contracts, you can manually run this command with
`truffle compile --config=truffle-config.matic.js` and add the `--all` flag.

### Migrating

To migrate on a Matic L2 network, run `npm run migrate:matic --network=(mit | mim | mt | mm)` (remember to choose a network from these options!).

You have several Matic L2 networks to choose from:

1) Infura networks. Infura is running a testnet node as well as a mainnet node for the Matic PoS chain. Deployment to these networks requires that you sign up for an Infura account and initiate a project. See the Infura website for [details](https://infura.io/). In the example network configuration, we expect you public Infura project key, which you should indicate in your `.env` file. The following Infura networks are indicated in the `truffle-config.matic.js` file:

  - `mit`: This is the Infura Matic testnet.
  - `mim`: This is the Infura Matic mainnet. Caution! If you deploy to this network using a connected wallet, the fees are charged in mainnet ETH.

2) Matic networks. Matic has provided RPC endpoints for testing and mainnet deployments.

  - `mt`: This is the Matic testnet, called "Mumbai". It is goerli-based.
  - `mm`: This is the Matic mainnet. Caution! Deployment requires mainnet ETH.   


If you would like to migrate previously migrated contracts on the same network, you can run `truffle migrate --config truffle-config.matic.js --network= (mit | mim | mt | mm)` and add the `--reset` flag.


### Paying for Migrations

To pay for your deployments, you will need to connect a wallet to at least one of the networks above. For testing, this means you will want to connect a wallet to the `mit` or `mt` networks. We recommend using [MetaMask](https://metamask.io/).

Documentation for how to set up MetaMask custom networks with Matic's testnet and mainnet can be found [here](https://docs.matic.network/docs/develop/metamask/config-matic). To set up a custom network for Infura's testnet and mainnet networks, follow the same steps but point the network to Infura's RPC endpoints (`"https://polygon-mainnet.infura.io/v3/" + infuraProjectId` and `"https://polygon-mumbai.infura.io/v3/" + infuraProjectId`). The `chainId` values are the same as those on the Matic networks.

To get testnet ETH to use, visit a faucet like https://goerli-faucet.slock.it/.

### Testing

Currently, this box only supports testing via Javascript/TypeScript tests. In order to run the test currently in the boilerplate, use the following command: `npm run test:matic --network=<(mit | mim | mt | mm)` (remember to choose a network!). The current test file just has some boilerplate tests to get you started. You will likely want to add network-specific tests to ensure your contracts are behaving as expected.

### New Configuration File

A new configuration file exists in this project: `truffle-config.ovm.js`. This file contains a reference to the new file location of the `contracts_build_directory` for Matic contracts and lists several networks that are running the Matic Layer 2 network instance.

Please note, the classic `truffle-config.js` configuration file is included here as well, because you will eventually want to deploy contracts to the EVM as well. All normal truffle commands (`truffle compile`, `truffle migrate`, etc.) will use this config file and save built files to `build/evm-contracts`.

### New File Structure for Artifacts

When you compile or migrate, the resulting `json` files for the OVM will be at `build/matic-contracts/`. This is to distinguish them from any EVM contracts you build, which will live in `build/evm-contracts `. As we have included the appropriate `contracts_build_directory` in each configuration file, Truffle will know which set of built files to reference!

### Communication Between Ethereum and Matic PoS Chains

The information above should allow you to deploy to the Matic PoS Layer 2 chain. This is only the first step! Once you are ready to deploy your own contracts to function on L1 using L2, you will need to be aware of the [ways in which Layer 1 and Layer 2 interact in the Polygon ecosystem](https://docs.matic.network/docs/develop/ethereum-matic/getting-started). Keep an eye out for additional Truffle tooling and examples that elucidate this second step to full Matic PoS L2 integration!

## Support

Support for this box is available via the Truffle community available [here](https://www.trufflesuite.com/community).
