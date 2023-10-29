---
title: Fund your accounts
description: Get tokens in your accounts
sidebar_position: 2
---

## Getting tokens on Linea

For hackathon developers, the easiest way to get test tokens is by sending a message via https://xmtp.chat to an ENS name provided in the relevant hackathon prizes page.

Otherwise, there are several ways to get tokens on Linea:

- [Transferring from another account](https://support.metamask.io/hc/en-us/articles/360015488931-How-to-send-tokens-from-your-MetaMask-wallet) (on Linea mainnet or testnet)
- [Bridging between networks](./bridges-of-linea/index.mdx)
- [Faucets](#use-a-linea-faucet) (for testnet tokens on Linea Goerli)

## Use a Linea faucet

If you want to drip Goerli ETH directly to Linea testnet, the following faucets are available. Note that you will need to enter your actual address — ENS names will not work.

1. [Infura Linea faucet](https://infura.io/faucet/linea)
2. [Covalent Linea faucet](https://www.covalenthq.com/faucet/)
3. [FAUCETME faucet](https://linea.faucetme.pro/)
4. [LearnWeb3 Faucet](https://learnweb3.io/faucets/linea_goerli) 

If you want more ETH than the daily allotted amount or run into trouble with the above faucets, you can also [bridge ETH to Linea](/use-mainnet/bridges-of-linea/how-to-bridge-eth). Note that you'll need to [get test ETH on Goerli](#get-test-eth-on-goerli) in order to do so.

If you want to drip other tokens, you can find the [multi-token Linea faucet here](https://faucet.goerli.linea.build/), which lists the different tokens you can add to your wallet on the Goerli and Linea Goerli testnet.

## Get test ETH on Goerli

To get Goerli ETH, you'll need to:

1. Navigate to the [Linea faucet](https://faucet.goerli.linea.build/)
1. Connect your wallet and switch to the Goerli test network (make sure you are showing test networks) ![goerli eth card](../../static/img/docs/use-mainnet/goerlieth_faucet.png)
1. Navigate to the list of faucets linked through the Goerli card
1. Select a faucet and drip Goerli ETH

Transactions on Linea are much cheaper than Ethereum mainnet. 0.2 ETH is enough to execute a basic workflow, but feel free to get as much as you need!

## Get test ETH on Linea testnet

In order to interact with Linea, you can either:

1. [Drip Goerli ETH directly to Linea through the various faucets](#use-a-linea-faucet)
1. [Bridge Goerli ETH to Linea](/use-mainnet/bridges-of-linea/how-to-bridge-eth)

## Get other tokens on Goerli

1. Navigate to the [Linea faucet](https://faucet.goerli.linea.build/)
1. Connect your MetaMask wallet
1. Select the Goerli network in your MetaMask wallet
1. Select the desired token from the list of available tokens

If you want to get tokens that are not ETH or USDC, you'll need to lock ETH as a countermeasure for draining liquidity pools.

For example, if you wanted to get 1 USDT on Goerli, you would:

1. Navigate to the [Linea faucet](https://faucet.goerli.linea.build/)
1. Connect your MetaMask wallet
1. Select the Goerli network in your MetaMask wallet
1. Add the USDT token to MetaMask by selecting `Add to MetaMask` on the USDT card
1. Lock 0.0005 ETH by typing in 0.0005 on the USDT card, clicking claim, and confirming and paying the gas for the MetaMask transaction.

To get your ETH back, you would unwrap your USDT in the USDT card to receive ETH in a 2000:1 ratio. For example, to get back 0.0005 ETH, you would need to unwrap 1 USDT by typing in 1 on the USDT card, clicking unwrap, and confirming and paying the gas for the MetaMask transaction.

## Get other tokens on Linea

Selecting the Linea network on the [Linea faucet page](https://faucet.goerli.linea.build/) will show you the available tokens on Linea.

:::info

Not all available tokens will drip directly onto Linea. If they are, you will see a `CLAIM` button. Otherwise, you will need to navigate to that token's faucet to get it on Goerli and then bridge.

:::

Specific tokens require specific bridges. If you want to bridge from Goerli to Linea, you can find the tokens, contract addresses, and associated bridges [here](./info-contracts.md#token-contract-addresses-and-bridges).