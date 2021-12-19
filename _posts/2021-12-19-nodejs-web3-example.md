---
layout: post
title:  Nodejs Web3 ERC20 example
date:   2021-12-19 06:14:27 +0700
categories: [nodejs]
tags: [nodejs, web3]
---

This is NodeJS Web3 example

Register an <https://infura.io/> account, they provides endpoints to access to ETH Node for testing.

Dependencies

`yarn add web3`

To create a network instance: You can listen on transactions, events, of the whole chain.

```js
const Web3 = require('web3');

const { INFURA_WSS_ENDPOINT } = process.env;

const web3Instance = new Web3(INFURA_WSS_ENDPOINT);

web3Instance.eth.getBlockNumber()
    .then((blockNumber) => console.log("Current block number: ", blockNumber));

```


To create a contract instance - ERC20: You can listen events, call methods of a given contract.

```js

const Web3EthContract = require('web3-eth-contract');
const ContractABI = require('./ContractABI.json');

const { INFURA_WSS_ENDPOINT } = process.env;

Web3EthContract.setProvider(INFURA_WSS_ENDPOINT);

//https://goerli.etherscan.io/address/0xa05F7df24D81438CB2306A07d1204967b48f20d2
const CONTRACT_ADDRESS = "0xa05F7df24D81438CB2306A07d1204967b48f20d2";

const contractInstance = new Web3EthContract(
    ContractABI,
    CONTRACT_ADDRESS
);

contractInstance.events.Transfer().on('data', event => console.log("Transfer: ", event))
    .on('changed', changed => console.log("Changed: ", changed))
    .on('error', err => console.log('Error: ', err.message, err.stack))
    .on('connected', str => console.log("Connected: ", str))


```

Repository on github: <https://github.com/vanduc1102/samples/tree/master/nodejs/web3-nodejs>
