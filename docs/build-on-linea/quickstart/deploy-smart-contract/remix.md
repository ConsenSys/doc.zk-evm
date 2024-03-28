---
title: Remix
description: Deploy a smart contract using Remix.
image: /img/socialCards/remix.jpg
---

import Tabs from '@theme/Tabs'; import TabItem from '@theme/TabItem';

In this tutorial, we'll walk through creating a basic Remix project and deploying a sample contract.

## Prerequisites

Before you begin, ensure you:

1. [Set up your wallet](../../../use-mainnet/set-up-your-wallet.mdx)
2. [Fund your wallet with Linea ETH](../../../use-mainnet/fund.mdx) on either the testnet, or mainnet

## Explore the Remix workspace

To start using Remix, navigate to their [website](https://remix.ethereum.org/). The default project includes a
code sample with a configured smart contract.

<div class="center-container">
  <div class="img-large">
    <img
      src="/img/article_images/Build_on_Linea/Quickstart/Deploy_a_smart_contract/Remix/Linea_deploy_smart_contract_Remix_1.png"
      alt="Remix configured smart contract code"
    />
  </div>
</div>

## Compile a Remix contract

Navigate to the **Solidity compiler** tab on the left navigation and click **Compile contract**.

<div class="center-container">
  <div class="img-large">
    <img
      src="/img/article_images/Build_on_Linea/Quickstart/Deploy_a_smart_contract/Remix/Linea_deploy_smart_contract_Remix_2.png"
      alt="Remix compile contract"
    />
  </div>
</div>

## Deploy the contract

You can deploy a smart contract using the injected provider, meaning Remix can auto-detect the network you're
on and your account information. To do this, navigate to the **Deploy & run transactions** tab.

:::caution

The public endpoints are rate limited and not meant for production systems. To use Infura, you'll need to [get an API key](https://support.infura.io/hc/en-us/articles/15116941373979-Connecting-to-the-Linea-network). Then, [manually add a network to your MetaMask wallet](https://support.metamask.io/hc/en-us/articles/360043227612-How-to-add-a-custom-network-RPC#h_01G63GGJ83DGDRCS2ZWXM37CV5) with the information found
[here](../../../use-mainnet/info-contracts.mdx#network-information).

:::

Switch to the Linea network (mainnet or testnets) in your MetaMask wallet, and from the **ENVIRONMENT** drop down, select
**Injected provider - MetaMask**.

<div class="center-container">
  <div class="img-large">
    <img
      src="/img/article_images/Build_on_Linea/Quickstart/Deploy_a_smart_contract/Remix/Linea_deploy_smart_contract_Remix_3.png"
      alt="Remix deploy contract"
    />
  </div>
</div>

Then, select **Deploy** and confirm the transaction. This deploys the contract to the network selected in MetaMask.