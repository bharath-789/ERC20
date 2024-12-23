# README: DeFi Empire - A Simple DeFi Kingdom Clone on Avalanche

## Project Overview

Welcome to the **DeFi Empire** project! This is a step-by-step guide to creating a simplified DeFi Kingdom clone using the Avalanche blockchain. By leveraging Avalanche's EVM Subnet capabilities, you'll create a decentralized finance (DeFi) gaming platform that allows players to engage in activities such as battling, exploring, and trading while earning rewards in the form of tokens.

This project focuses on **custom EVM subnet development** and deploying foundational smart contracts like ERC20 tokens and a Vault. Let’s build a thriving DeFi Empire!

---

## Features
- **Custom EVM Subnet**: Deploy your own blockchain on Avalanche with low fees and customizable features.
- **In-Game Currency**: Implement your unique ERC20 token as the backbone of your gaming ecosystem.
- **Vault Contract**: Securely manage token deposits and withdrawals.
- **DeFi Gameplay Elements**: Explore the possibilities of battling, trading, and building liquidity pools.
- **Metamask Integration**: Seamlessly interact with your custom blockchain and smart contracts.

---

## Tools and Prerequisites
To complete this project, you'll need:
- **Unix-based Computer** (MacOS or Linux)
- **Solidity** and **Remix IDE**
- **Avalanche CLI**
- **Metamask Wallet**
- **Web Browser**

---

## Step-by-Step Guide

### 1. Set Up Your EVM Subnet
1. Install and configure the **Avalanche CLI**.
2. Deploy a custom EVM subnet by following the official Avalanche [documentation](https://docs.avax.network/).
3. Define your native currency for in-game transactions.

### 2. Connect to Metamask
1. Add your custom subnet as a network in Metamask.
2. Ensure your custom subnet is selected.

### 3. Deploy Smart Contracts
#### a. **ERC20 Token Contract**
This contract creates your game's native token. Here’s an example implementation:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

contract ERC20 {
    string public name = "GameToken";
    string public symbol = "GTKN";
    uint8 public decimals = 18;
    uint public totalSupply;

    mapping(address => uint) public balanceOf;
    mapping(address => mapping(address => uint)) public allowance;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);

    function transfer(address recipient, uint amount) external returns (bool) {
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint amount) external returns (bool) {
        allowance[sender][msg.sender] -= amount;
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function mint(uint amount) external {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }

    function burn(uint amount) external {
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }
}
```

#### b. **Vault Contract**
The Vault allows secure deposits and withdrawals of tokens:
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;

interface IERC20 {
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
}

contract Vault {
    IERC20 public token;
    uint public totalSupply;
    mapping(address => uint) public balanceOf;

    constructor(address _token) {
        token = IERC20(_token);
    }

    function deposit(uint amount) external {
        uint shares = (totalSupply == 0) ? amount : (amount * totalSupply) / token.balanceOf(address(this));
        totalSupply += shares;
        balanceOf[msg.sender] += shares;
        token.transferFrom(msg.sender, address(this), amount);
    }

    function withdraw(uint shares) external {
        uint amount = (shares * token.balanceOf(address(this))) / totalSupply;
        totalSupply -= shares;
        balanceOf[msg.sender] -= shares;
        token.transfer(msg.sender, amount);
    }
}
```

### 4. Test and Interact
- Use **Remix IDE** to deploy and test your smart contracts.
- Interact with your contracts directly in Remix or through your custom EVM subnet.

---

## Submission Requirements

1. **GitHub Repository**
   - Add your project files to a public GitHub repository.
   - Include a `README.md` with an overview of your project.

2. **Video Walkthrough**
   - Record a 5-minute video explaining your code.
   - Include a live demo of deploying and interacting with your contracts.

3. **Transaction/Deployment Identifier**
   - Provide the transaction ID or deployment ID of your contracts on Avalanche.

---

## Additional Resources
- [Avalanche Documentation](https://docs.avax.network/)
- [Remix IDE](https://remix.ethereum.org/)
- [Metamask](https://metamask.io/)

---

## License
This project is licensed under the [MIT License](LICENSE).  

---

Unleash your creativity and take your first steps in building a DeFi Empire on Avalanche. Good luck! 🚀
