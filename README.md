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
