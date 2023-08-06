# Token Contract 

## Description
The Token contract is an implementation of a standard ERC-20 token on the Ethereum blockchain. It allows users to create, transfer, and manage tokens within the contract. The contract is open-source and released under the MIT License.

## Prerequisites
To interact with the Token contract, you need the following:
- An Ethereum wallet with ETH to cover transaction fees (e.g., MetaMask).
- A web3-enabled browser or a web3 provider like Infura.

## Contract Details
The contract contains the following variables:

- `name`: A string representing the name of the token.
- `symbol`: A string representing the symbol or ticker of the token.
- `decimals`: An 8-bit unsigned integer representing the number of decimal places for token values.
- `totalSupply`: An unsigned integer representing the total supply of the token.
- `balanceOf`: A mapping that associates an Ethereum address with its token balance.
- `allowance`: A mapping that tracks the approved token allowance for one address to spend on behalf of another address.

The contract emits the following events:

- `Transfer`: Fired when tokens are transferred from one address to another.
- `Approval`: Fired when an allowance is approved for one address to spend tokens on behalf of another address.
- `Mint`: Fired when new tokens are created and added to an address.
- `Burn`: Fired when tokens are destroyed and removed from an address.
- `Donate`: Fired when tokens are transferred from one address to another as a donation.

## Constructor
The constructor function is executed when the contract is deployed and initializes the token's initial parameters. It takes the following parameters:
- `_name`: The name of the token.
- `_symbol`: The symbol or ticker of the token.
- `_decimals`: The number of decimal places for token values.
- `_initialSupply`: The initial total supply of tokens.

## Functions

1. `mint`: Allows the contract owner to create new tokens and assign them to a specified address. The function takes the `_to` address and the `_amount` of tokens to mint.

2. `burn`: Allows users to destroy their tokens, reducing the total supply. The function takes the `_amount` of tokens to burn.

3. `transfer`: Allows users to transfer tokens to another address. The function takes the `_to` address and the `_amount` of tokens to transfer.

4. `approve`: Allows users to approve another address to spend a specific amount of tokens on their behalf. The function takes the `_spender` address and the `_amount` of tokens to approve.

5. `donate`: Allows users to transfer tokens as a donation to another address. The function takes the `_to` address and the `_amount` of tokens to donate.

## code

    // SPDX-License-Identifier: MIT
    pragma solidity ^0.8.0;

    contract Token {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Mint(address indexed to, uint256 value);
    event Burn(address indexed from, uint256 value);
    event Donate(address indexed from, address indexed to, uint256 value);

    constructor(
        string memory _name,
        string memory _symbol,
        uint8 _decimals,
        uint256 _initialSupply
    ) {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _initialSupply * 10**uint256(_decimals);
        balanceOf[msg.sender] = totalSupply;
    }

    function mint(address _to, uint256 _amount) public {
        require(_to != address(0), "Invalid address");
        require(_amount > 0, "Invalid amount");
        require(balanceOf[msg.sender] >= _amount, "Not enough balance");
        balanceOf[msg.sender] -= _amount;
        balanceOf[_to] += _amount;
        emit Mint(_to, _amount);
        emit Transfer(msg.sender, _to, _amount);
    }

    function burn(uint256 _amount) public {
        require(_amount > 0, "Invalid amount");
        require(balanceOf[msg.sender] >= _amount, "Not enough balance");
        balanceOf[msg.sender] -= _amount;
        totalSupply -= _amount;
        emit Burn(msg.sender, _amount);
        emit Transfer(msg.sender, address(0), _amount);
    }

    function transfer(address _to, uint256 _amount) public returns (bool success) {
        require(_to != address(0), "Invalid address");
        require(balanceOf[msg.sender] >= _amount, "Not enough balance");
        balanceOf[msg.sender] -= _amount;
        balanceOf[_to] += _amount;
        emit Transfer(msg.sender, _to, _amount);
        return true;
    }

    function approve(address _spender, uint256 _amount) public returns (bool success) {
        allowance[msg.sender][_spender] = _amount;
        emit Approval(msg.sender, _spender, _amount);
        return true;
    }

    function donate(address _to, uint256 _amount) public returns (bool success) {
        require(_to != address(0), "Invalid address");
        require(balanceOf[msg.sender] >= _amount, "Not enough balance");
        balanceOf[msg.sender] -= _amount;
        balanceOf[_to] += _amount;
        emit Donate(msg.sender, _to, _amount);
        emit Transfer(msg.sender, _to, _amount);
        return true;
    }

    }


## Usage
To use the Token contract, follow these steps:
1. Deploy the contract with the desired initial parameters (name, symbol, decimals, initial supply).
2. Interact with the contract using the provided functions (`mint`, `burn`, `transfer`, `approve`, and `donate`) through a web3-enabled browser or web3 provider.

## Security Considerations
1. Ensure that the contract owner's private key is kept secure, as it has the ability to create new tokens (`mint` function).
2. Be cautious when transferring tokens to unknown or untrusted addresses.

## License
The contract is licensed under the MIT License
