---
title: Build your dapp using the message bridge
---

# Build your dapp using the message bridge

Use the ConsenSys zkEVM message bridge to bridge funds (in ETH) or arbitrary messages between Goerli and the ConsenSys
zkEVM network to enable your use case.

!!! important

    You can also just [bridge funds](../get-started/bridge-funds.md).

## Overview

The ConsenSys zkEVM message bridge operates using the following patterns.

### L1 --> L2

1. The developer sends a transaction to the L1 bridge contract (implementing the `IBridge.sol` interface) on
the `dispatchMessage` method.
1. The L1 bridge contract emits a `MessageDispatched` event.
1. The rollup `relayer` catches the event and sends an L2 transaction to the L2 bridge contract on the `deliverMessage`
function.
1. The L2 contract calls the contract defined in the `to` field. The L2 contract should authenticate the call by calling
the `sender()` method on the L2 bridge upon reception of the `deliverMessage` call, and verifying that this
corresponds to a known L1 address.
1. The L1 bridge contract verifies the message execution, and emits an event `MessageConfirmed`, once the next L2 block is
finalized and the ZK proof is verified.
1. Otherwise, the developer can drop a message after the `deadline`, effectively reimbursing the value that was sent.

### L2 --> L1

1. The four first steps are identical to [L1 --> L2](#l1----l2) but in the opposite direction.
1. The `relayer` embeds messages as a parameter when finalizing the block and verifying the ZK proof.

### Deployed contract addresses

The contracts are deployed at the following addresses:

* The L1 bridge, implementing the `IBridge.sol` and `IL1Bridge.sol` interfaces, is located at [`0xE87d317eB8dcc9afE24d9f63D6C760e52Bc18A40`](https://goerli.etherscan.io/address/0xe87d317eb8dcc9afe24d9f63d6c760e52bc18a40).
* The L2 bridge, implementing the `IBridge.sol` interface, is located at [`0xA59477f7742Ba7d51bb1E487a8540aB339d6801d`](https://explorer.goerli.zkevm.consensys.net/address/0xA59477f7742Ba7d51bb1E487a8540aB339d6801d).

## Interfaces

``` solidity linenums="1" title="IBridge.sol"
--8<-- "docs/developers/contracts/IBridge.sol"
```

``` solidity linenums="1" title="IL1Bridge.sol"
--8<-- "docs/developers/contracts/IL1Bridge.sol"
```
