
# ERC4626 Tokenized Vault

This project offers a foundational implementation of a tokenized vault using the ERC4626 standard.

## Overview

ERC4626 is an innovative Ethereum standard that introduces the concept of tokenized vaults. Building upon the well-established ERC20 token standard, ERC4626 aims to create a more structured, flexible, and composable interface specifically for vault operations on the Ethereum blockchain.

A tokenized vault, as proposed by this standard, serves as a secure container where assets (like tokens) can be deposited and, in exchange, users receive representative shares of the vault. The unique aspect of ERC4626 is its ability to handle various intricate operations related to these vaults, which can include functions such as depositing, withdrawing, and accounting for fees, among others.

The primary motivation behind ERC4626 is to ensure that different decentralized finance (DeFi) applications, ranging from lending markets to aggregators and tokens that inherently bear interest, can operate seamlessly with standardized vaults. This compatibility and composability ensure that developers can integrate and work with such vaults effortlessly, while users can trust in the uniformity of operations across different platforms.

## Implementing Fees in the Vault

Reference: [OpenZeppelin's Example](https://docs.openzeppelin.com/contracts/4.x/erc4626#fees)

ERC4626 vaults enable fee collection during either the deposit/mint or withdraw/redeem phases. For both scenarios, compliance with ERC4626's preview functions is imperative.

To illustrate:

- When invoking `deposit(100, receiver)`, the invoker must deposit a precise total of 100 underlying tokens, inclusive of fees. The receiver, in turn, should acquire shares equivalent to the output of `previewDeposit(100)`. In a similar vein, `previewMint` must account for the additional fees atop the share’s cost.

- The `Deposit` event, though ambiguously defined in the EIP specification, generally agrees upon including the entire asset count funded by the user, fees inclusive.

Conversely, during asset withdrawals:

- The specified amount by a user should be equal to the actual received assets. Any additional fees must be encompassed within the quote provided by `previewWithdraw`.

- The `Withdraw` event should detail the share count the user nullifies (inclusive of fees) and the post-fee-deduction asset amount the user receives.

As an outcome, both `Deposit` and `Withdraw` events will delineate two distinct exchange rates, with the gap between the acquisition and departure prices symbolizing the fees the vault accumulates.
