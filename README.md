# Module 3 - Types of Functions

This is a custom implementation of an ERC-20 token using the OpenZeppelin contracts library. The contract extends the functionality of a standard ERC-20 token by adding the ability to mint, burn, and perform custom transfers. The token can be deployed to an Ethereum-compatible blockchain, enabling users to create, send, and manage ERC-20 tokens.

## Table of Contents
1. [Overview](#overview)
2. [Contract Structure](#contract-structure)
3. [Functions](#functions)
4. [Deployment](#deployment)
5. [Usage](#usage)
6. [License](#license)

---

## Overview

The `Cust_ERC20` contract is a custom implementation of the ERC-20 standard. It inherits from OpenZeppelin's `ERC20` contract, and includes three key additional features:
1. **Minting**: Allows anyone to mint new tokens to their address.
2. **Burning**: Allows users to burn (destroy) their own tokens.
3. **Custom Transfer**: A custom implementation of token transfers, with additional checks and validation to prevent transfers to the zero address and ensure sufficient balance.

---

## Contract Structure

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract Cust_ERC20 is ERC20 {
    
    constructor(string memory name, string memory symbol) ERC20(name, symbol) {
    }

    function mint(uint256 amount) public {
        _mint(msg.sender, amount);
    }

    function burn(uint256 amount) public {
        _burn(msg.sender, amount);
    }

    function customTransfer(address to, uint256 amount) public returns (bool) {
        
        require(to != address(0), "Transfer to the zero address is not allowed");
        require(balanceOf(msg.sender) >= amount, "Insufficient balance");

        _transfer(msg.sender, to, amount);

        return true;
    }
}
```

### 1. **Inherits ERC20**
The contract inherits from OpenZeppelin's `ERC20` implementation, which provides the standard functions for an ERC-20 token, such as transferring tokens, checking balances, and approving transfers.

### 2. **Constructor**
The constructor accepts two parameters: `name` and `symbol`, which are passed to the `ERC20` constructor to initialize the token's name and symbol.

```solidity
constructor(string memory name, string memory symbol) ERC20(name, symbol)
```

### 3. **Mint Function**
This function allows the caller to mint a specified amount of new tokens to their own address.
```solidity
function mint(uint256 amount) public
```

### 4. **Burn Function**
This function allows the caller to burn (destroy) a specified amount of tokens from their own balance.
```solidity
function burn(uint256 amount) public
```

### 5. **Custom Transfer Function**
This custom transfer function allows a user to transfer tokens to another address, with additional checks to prevent transfers to the zero address and ensure the sender has enough balance.
```solidity
function customTransfer(address to, uint256 amount) public returns (bool)
```

---

## Functions

### 1. **mint**
```solidity
function mint(uint256 amount) public;
```
- **Description**: Mints a specified amount of tokens and assigns them to the caller's address (msg.sender).
- **Parameters**:
  - `amount`: The number of tokens to mint.
- **Requires**: The caller can mint an arbitrary number of tokens to their own account.

### 2. **burn**
```solidity
function burn(uint256 amount) public;
```
- **Description**: Burns a specified amount of tokens from the caller's account, effectively reducing the total supply.
- **Parameters**:
  - `amount`: The number of tokens to burn.
- **Requires**: The caller must have a sufficient balance of tokens to burn.

### 3. **customTransfer**
```solidity
function customTransfer(address to, uint256 amount) public returns (bool);
```
- **Description**: Transfers tokens from the caller's address to another address with additional validation.
- **Parameters**:
  - `to`: The address to transfer tokens to.
  - `amount`: The number of tokens to transfer.
- **Returns**: A boolean indicating whether the transfer was successful.
- **Requires**:
  - The `to` address cannot be the zero address (`0x0`).
  - The caller must have a sufficient balance of tokens to perform the transfer.

---

## Deployment

1. **Prerequisites**:
   - Ensure that you have a development environment that supports Solidity 0.8.26, such as [Remix](https://remix.ethereum.org/), [Truffle](https://www.trufflesuite.com/), or [Hardhat](https://hardhat.org/).
   - Install OpenZeppelin contracts if you're using a local development environment (e.g., Truffle or Hardhat):
     ```bash
     npm install @openzeppelin/contracts
     ```

2. **Deploy the Contract**:
   - In Remix, compile the contract and deploy it to the Ethereum network or a test network (such as Rinkeby or Goerli).
   - If using Truffle or Hardhat, ensure your deployment script points to the appropriate network and deploy the contract.

---

## Usage

### 1. **Mint Tokens**
To mint tokens, you can call the `mint` function, which will mint tokens to your address:
```solidity
mint(1000);
```
This will mint 1000 tokens to your address.

### 2. **Burn Tokens**
To burn tokens, simply call the `burn` function:
```solidity
burn(500);
```
This will burn 500 tokens from your address, reducing your balance and the total supply.

### 3. **Transfer Tokens**
To transfer tokens to another address, use the custom transfer function:
```solidity
customTransfer(address recipient, uint256 amount);
```
For example, transferring 100 tokens to `0x123...`:
```solidity
customTransfer(0x1234567890abcdef1234567890abcdef12345678, 100);
```

---

## License

This contract is released under the [MIT License](https://opensource.org/licenses/MIT). See the LICENSE file for more details.

---

## Disclaimer

This contract is intended for educational purposes and might require further testing, security audits, and optimizations before deploying it on the main Ethereum network or in production environments. Always review and audit the code thoroughly before using it with real funds.
