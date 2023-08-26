# Degen N Token
This token contract program tells about the transaction of tokens by burning, adding or transferring tokens using various functions and events like mint, burn, transfer, Total_Supply, balanceOf, etc.
## Description

This program is a simple contract for creating and mint of token, providing the details of token such as its name(i.e. my_token), total_Supply, balance, getting the results accordingly. It contains three events that are Transfer(for transferring the tokens), Burn (for burning the tokens) and Mint (for adding the tokens). Then there is a constructor with two parameters that are Nikhils_Token and TotalSupply, which are getting stored in my_token and total_Supply, also the value of total_Supply get stored in balances. After that there are six functions, Total_Supply() for returning the total_Supply value, balanceOf for returning balance of account, transfer for transferring tokens, burn for burning the amount of token and then updating the current value after burning of token from account and mint function for adding the tokens and accordingly updating the total_Supply and balances.

## Getting Started
### Executing program
       
```javascript
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function mint(address account, uint256 amount) external returns (bool);
    function burn(uint256 amount) external returns (bool);
    function redeem() external returns (bool);
}

contract Degen_N_Token is IERC20 {
    string public name;
    string public symbol;
 
    uint256 private _totalSupply;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    address private _owner;
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Mint(address indexed account, uint256 amount);
    event Burn(address indexed account, uint256 amount);
    event Redeem(address indexed account, uint256 amount);

    modifier onlyOwner() {
        require(msg.sender == _owner, "Only the owner can perform this action");
        _;
    }

    constructor(string memory NtokenName, string memory NtokenSymbol, uint256 initialSupply) {
        name = NtokenName;
        symbol = NtokenSymbol;
    
        _totalSupply = initialSupply;
        _balances[msg.sender] = _totalSupply;
        _owner = msg.sender;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    function totalSupply() external view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) external override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function allowance(address owner, address spender) external view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) external override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override returns (bool) {
        _transfer(sender, recipient, amount);
        uint256 currentAllowance = _allowances[sender][msg.sender];
        require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
        _approve(sender, msg.sender, currentAllowance - amount);
        return true;
    }

    function mint(address account, uint256 amount) external override onlyOwner returns (bool) {
        require(account != address(0), "ERC20: mint to the zero address");
        _totalSupply += amount;
        _balances[account] += amount;
        emit Mint(account, amount);
        emit Transfer(address(0), account, amount);
        return true;
    }

    function burn(uint256 amount) external override returns (bool) {
        require(amount <= _balances[msg.sender], "ERC20: burn amount exceeds balance");
        _balances[msg.sender] -= amount;
        _totalSupply -= amount;
        emit Burn(msg.sender, amount);
        emit Transfer(msg.sender, address(0), amount);
        return true;
    }

    function redeem() external override returns (bool) {
        uint256 amount = _balances[msg.sender];
        require(amount > 0, "ERC20: redeem amount is zero");
        _balances[msg.sender] = 0;
        _totalSupply -= amount;
        emit Redeem(msg.sender, amount);
        emit Transfer(msg.sender, address(0), amount);
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(amount <= _balances[sender], "ERC20: transfer amount exceeds balance");

        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
}                          
```
To compile the code, click on the "Solidity Compiler" tab in the left-hand sidebar. Make sure the "Compiler" option is set to "0.8.4" (or another compatible version), and then click on the "Compile M_m3p3.sol" button.

Once the code is compiled, you can deploy the contract by clicking on the "Deploy & Run Transactions" tab in the left-hand sidebar. Select the "Niks_Token" contract from the dropdown menu, and then click on the "Deploy" button. 

Once the contract is deployed, you can interact with it by clicking on various functions. Click on the "Niks_Token" contract in the left-hand sidebar, and then click on the functions for transactions, getting the results accordingly as burning or adding of tokens.

## Authors
Nikhil Upadhyay

## License
This project is licensed under the MIT License
