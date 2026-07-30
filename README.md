# Learning Base

This repository is my personal space to document everything I learn about Base, the Ethereum Layer 2 developed by Coinbase.

My goal is to understand how Base works and gradually build practical knowledge.

### What Makes Base Interesting

Base combines the security of Ethereum with much lower fees and faster confirmations.  

This makes it a great network for learning and experimenting.

### Understanding Layer 2

Base is a Layer 2 solution built on top of Ethereum.  

It processes transactions off the main chain while still benefiting from Ethereum’s security.

### Adding a Setter Function

```solidity
function setMessage(string memory newMessage) public {
    message = newMessage;
}

### Explicit Getter Function

```solidity
function getMessage() public view returns (string memory) {
    return message;
}

### Adding an Event

```solidity
event MessageUpdated(address indexed sender, string newMessage);

function setMessage(string memory newMessage) public {
    message = newMessage;
    emit MessageUpdated(msg.sender, newMessage);
}

### Adding a Constructor

```solidity
constructor(string memory initialMessage) {
    message = initialMessage;
}

### Adding Owner Variable

```solidity
address public owner;

constructor(string memory initialMessage) {
    message = initialMessage;
    owner = msg.sender;
}

### Transfer Ownership

```solidity
function transferOwnership(address newOwner) public onlyOwner {
    require(newOwner != address(0), "Invalid address");
    owner = newOwner;
}

### Simple Mapping

```solidity
mapping(address => uint256) public balances;

function deposit() public payable {
    balances[msg.sender] += msg.value;
}

### Deposit Event

```solidity
event Deposit(address indexed user, uint256 amount);

function deposit() public payable {
    balances[msg.sender] += msg.value;
    emit Deposit(msg.sender, msg.value);
}

### Adding Require Statements

```solidity
function withdraw(uint256 amount) public {
    require(amount > 0, "Amount must be greater than 0");
    require(balances[msg.sender] >= amount, "Insufficient balance");
    
    balances[msg.sender] -= amount;
    payable(msg.sender).transfer(amount);
}

### Creating a Modifier

```solidity
modifier onlyOwner() {
    require(msg.sender == owner, "Not the owner");
    _;
}

### Understanding msg.sender

`msg.sender` is the address that is calling the function.

It is one of the most important global variables in Solidity.

### Payable Functions

```solidity
function deposit() public payable {
    balances[msg.sender] += msg.value;
}

### Payable Functions

```solidity
function deposit() public payable {
    balances[msg.sender] += msg.value;
}

### Fallback Function

```solidity
fallback() external payable {
    // Executes when no other function matches
}

### The address Type

```solidity
address public user;

function setUser(address _user) public {
    user = _user;
}

### Simple Struct

```solidity
struct User {
    address addr;
    uint256 balance;
    bool active;
}

### Simple Array

```solidity
address[] public userList;

function addUser(address user) public {
    userList.push(user);
}

### Simple Loop Example

```solidity
function sum(uint256[] memory numbers) public pure returns (uint256) {
    uint256 total = 0;
    for (uint256 i = 0; i < numbers.length; i++) {
        total += numbers[i];
    }
    return total;
}

### If / Else Example

```solidity
function isOwner() public view returns (bool) {
    if (msg.sender == owner) {
        return true;
    } else {
        return false;
    }
}

### Simple Enum

```solidity
enum Status { Pending, Active, Completed }

Status public currentStatus;

### Basic Interface

```solidity
interface IExample {
    function getValue() external view returns (uint256);
}

### Introduction to ERC20

ERC20 is the standard interface for fungible tokens on Ethereum and Base.

Almost every token you see follows this standard.

### Transfer Event

```solidity
event Transfer(address indexed from, address indexed to, uint256 value);

function transfer(address to, uint256 amount) public returns (bool) {
    balanceOf[msg.sender] -= amount;
    balanceOf[to] += amount;
    emit Transfer(msg.sender, to, amount);
    return true;
}

### Burn Function

```solidity
function burn(uint256 amount) public {
    require(balanceOf[msg.sender] >= amount, "Insufficient balance");
    balanceOf[msg.sender] -= amount;
    totalSupply -= amount;
    emit Transfer(msg.sender, address(0), amount);
}

### Name and Symbol

```solidity
string public name = "MyBaseToken";
string public symbol = "MBT";
