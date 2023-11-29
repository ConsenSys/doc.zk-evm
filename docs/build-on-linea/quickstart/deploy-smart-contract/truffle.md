---
title: Truffle
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

In this tutorial, we'll walk through creating a basic Truffle project and deploying a token contract.

Here's a video walkthrough:

<iframe
  width="560"
  height="315"
  src="https://www.youtube.com/embed/p0nFsVVvNvQ"
  title="YouTube video player"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
  allowfullscreen></iframe>

## Prerequisites

Before you begin, ensure you've:

1. [Set up your wallet](../../../use-mainnet/set-up-your-wallet.mdx)
2. [Funded your wallet with Linea ETH](../../../use-mainnet/fund.md) on either the testnet or mainnet
3. [Installed Truffle using the recommended installation procedure](https://trufflesuite.com/docs/truffle/how-to/install/).

## Create a Truffle project

To create an empty Truffle project, run:

```bash
truffle init linea-tutorial
```

Change into the new directory:

```bash
cd linea-tutorial
```

## Write the smart contract

Create your smart contract in the `contracts` directory by either creating a new file `Token.sol` or calling `truffle create contract Token`. Then, add the following code:

```sol
pragma solidity 0.8.17;

// SPDX-License-Identifier: MIT

contract Token {
  string public name = "My Token";
  string public symbol = "MTK";
  uint8 public decimals = 18;
  uint256 public totalSupply = 100000000;

  mapping (address => uint256) public balances;
  address public owner;

  constructor() {
    owner = msg.sender;
    balances[owner] = totalSupply;
  }

  function transfer(address recipient, uint256 amount) public {
    require(balances[msg.sender] >= amount, "Insufficient balance.");
    balances[msg.sender] -= amount;
    balances[recipient] += amount;
  }
}
```

You can check if it compiles by running `truffle compile` from the root folder.

## Write the migration script

To tell Truffle how we want to deploy our smart contracts, and in what order, we need to write a migration script.

Create `1_deploy_token.js` in the `migrations` directory, and add the following code:

```javascript
const Token = artifacts.require("Token");

module.exports = function (deployer) {
  deployer.deploy(Token);
};
```

## Deploy your contract

Truffle allows you to deploy through the [Truffle Dashboard](#truffle-dashboard) using your MetaMask wallet!

### Truffle Dashboard

[Truffle Dashboard](https://trufflesuite.com/docs/truffle/how-to/use-the-truffle-dashboard/) allows you to forgo saving your private keys locally, instead connecting to your MetaMask wallet for deployments. To deploy with Truffle Dashboard, you need to:

1. Run `truffle dashboard` in your terminal, which will open a window on port `24012`.
1. Navigate to `localhost:24012` in your browser. Please ensure that Dashboard is connected to the Linea testnet by connecting your MetaMask wallet to Linea. For reference, the Linea testnet network ID is `59140`.

<div class="center-container">
  <div class="img-large">
    <img
      src="/img/article_images/Build_on_Linea/Quickstart/Deploy_a_smart_contract/Truffle/Linea_deploy_smart_contract_Truffle_1.png"
      alt="Truffle dashboard confirm network"
    />
  </div>
</div>

3. Run `truffle migrate --network dashboard` in a separate terminal.
1. Navigate back to `localhost:24012`. You should see a prompt asking you to confirm the deployment. Click **Confirm**.

<div class="center-container">
  <div class="img-large">
    <img
      src="/img/article_images/Build_on_Linea/Quickstart/Deploy_a_smart_contract/Truffle/Linea_deploy_smart_contract_Truffle_2.png"
      alt="Truffle dashboard signature requests"
    />
  </div>
</div>

### truffle-config

You can deploy with Truffle using the command line, by specifying Linea testnet in `truffle-config.js`. To do so, you need to:

1. Create a `.env` file in the root folder with your wallet's mnemonic. This is your [secret recovery phrase](https://support.metamask.io/hc/en-us/articles/360015290032-How-to-reveal-your-Secret-Recovery-Phrase).

   ```
   MNEMONIC=<MNEMONIC>
   ```

   :::warning

   Please do not check your keys into source control. Please add `.env` to your `.gitignore`

   :::

1. Download `dotenv` and `@truffle/hdwallet-provider`
   ```
   npm i -D dotenv
   npm i -D @truffle/hdwallet-provider
   ```
1. Add the Linea testnet to your `truffle-config.js` file:

  <Tabs className="my-tabs">
    <TabItem value="Infura" label="Infura" default>

    To use Infura, you'll need to [get an API key](https://support.infura.io/hc/en-us/articles/15116941373979-Connecting-to-the-Linea-network). Add it to the `.env` file as follows:

    ```
    MNEMONIC=MNEMONIC
    INFURA_API_KEY=INFURA_API_KEY
    ```

    Then, modify `truffle-config.js` like so:

    ```javascript
    require("dotenv").config();
    const { MNEMONIC, INFURA_API_KEY } = process.env;

    const HDWalletProvider = require("@truffle/hdwallet-provider");

    module.exports = {
      networks: {
        linea_testnet: {
          provider: () => {
            return new HDWalletProvider(
              MNEMONIC,
              `https://linea-goerli.infura.io/v3/${INFURA_API_KEY}`,
            );
          },
          network_id: "59140",
        },
        linea_mainnet: {
          provider: () => {
            return new HDWalletProvider(
              MNEMONIC,
              `https://linea-mainnet.infura.io/v3/${INFURA_API_KEY}`,
            );
          },
          network_id: "59144",
        },
      },
      // ... rest of truffle-config.js
    };
    ```

    </TabItem>
    <TabItem value="Public Endpoint" label="Public Endpoint">

    The public endpoints are rate limited and not meant for production systems. However, you can use the public endpoints by modifying `truffle-config.js` as follows:

    ```javascript
    require("dotenv").config();
    const { MNEMONIC } = process.env;

    const HDWalletProvider = require("@truffle/hdwallet-provider");

    module.exports = {
      networks: {
        linea_testnet: {
          provider: () => {
            return new HDWalletProvider(
              MNEMONIC,
              `https://rpc.goerli.linea.build/`,
            );
          },
          network_id: "59140",
        },
        linea_mainnet: {
          provider: () => {
            return new HDWalletProvider(
              MNEMONIC,
              `https://rpc.linea.build/`,
            );
          },
          network_id: "59144",
        },
      },
      // ... rest of truffle-config.js
    };
    ```

  </TabItem>
  </Tabs>

1. Call `truffle migrate --network linea_testnet` or `truffle migrate --network linea_mainnet` from the CLI.

  Your output should look similar to the following:

   ````bash
   Compiling your contracts...
   ===========================
   > Everything is up to date, there is nothing to compile.

   Starting migrations...
   ======================
   > Network name:    'linea'
   > Network id:      59140
   > Block gas limit: 30000000 (0x1c9c380)

   1_deploy_token.js
   =================

     Deploying 'Token'
     -----------------
     > transaction hash:    0x412d58eaf4cc387fe1efa52b105f6fadac36db934b1617d04eaefc1947197525
     > Blocks: 0            Seconds: 0
     > contract address:    0x33b4D321Fc300E4f402820052EFA0958272D2AE5
     > block number:        143419
     > block timestamp:     1677366505
     > account:             YOUR_ACCOUNT_NUMBER
     > balance:             0.088400819995522296
     > gas used:            639672 (0x9c2b8)
     > gas price:           2.500000007 gwei
     > value sent:          0 ETH
     > total cost:          0.001599180004477704 ETH

     > Saving artifacts
     -------------------------------------
     > Total cost:     0.001599180004477704 ETH

   Summary
   =======
   > Total deployments:   1
   > Final cost:          0.001599180004477704 ETH
   ``` -->
   ````

Next, you can optionally [verify your contract on the network](docs/build-on-linea/quickstart/verify-smart-contract/truffle.md).

