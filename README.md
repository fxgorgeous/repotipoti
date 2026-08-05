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

### Introduction to ERC721

ERC721 is the standard for non-fungible tokens (NFTs).

Each token is unique and has its own ID.

### NFT Transfer

```solidity
function transfer(address to, uint256 tokenId) public {
    require(ownerOf[tokenId] == msg.sender, "Not the owner");
    ownerOf[tokenId] = to;
    balanceOf[msg.sender] -= 1;
    balanceOf[to] += 1;
}

### NFT Approval

```solidity
mapping(uint256 => address) public getApproved;

function approve(address to, uint256 tokenId) public {
    require(ownerOf[tokenId] == msg.sender, "Not the owner");
    getApproved[tokenId] = to;
}

### Burn NFT

```solidity
function burn(uint256 tokenId) public {
    require(ownerOf[tokenId] == msg.sender, "Not the owner");
    delete ownerOf[tokenId];
    balanceOf[msg.sender] -= 1;
    delete getApproved[tokenId];
}

### Mint with Payment

```solidity
uint256 public mintPrice = 0.01 ether;

function mint() public payable {
    require(msg.value >= mintPrice, "Insufficient payment");
    ownerOf[nextTokenId] = msg.sender;
    balanceOf[msg.sender] += 1;
    nextTokenId++;
}

### Pause Functionality

```solidity
bool public paused = false;

modifier whenNotPaused() {
    require(!paused, "Contract is paused");
    _;
}

### Get Contract Balance

```solidity
function getContractBalance() public view returns (uint256) {
    return address(this).balance;
}

### Batch Mint

```solidity
function batchMint(uint256 quantity) public payable {
    require(quantity > 0, "Quantity must be greater than 0");
    require(nextTokenId + quantity <= maxSupply, "Exceeds max supply");
    require(msg.value >= mintPrice * quantity, "Insufficient payment");

    for (uint256 i = 0; i < quantity; i++) {
        ownerOf[nextTokenId] = msg.sender;
        balanceOf[msg.sender] += 1;
        nextTokenId++;
    }
}

### Token Ownership History Idea

```solidity
mapping(uint256 => address[]) public ownershipHistory;

function transfer(address to, uint256 tokenId) public {
    require(ownerOf[tokenId] == msg.sender, "Not the owner");
    ownershipHistory[tokenId].push(to);
    // rest of transfer logic
}

### Original Minter Tracking

```solidity
mapping(uint256 => address) public originalMinter;

function mint() public payable {
    // existing checks...
    originalMinter[nextTokenId] = msg.sender;
    ownerOf[nextTokenId] = msg.sender;
    balanceOf[msg.sender] += 1;
    nextTokenId++;
}

### Token Level / Rarity

```solidity
mapping(uint256 => uint256) public tokenLevel;

function mint() public payable {
    // existing checks...
    tokenLevel[nextTokenId] = 1; // starting level
    ownerOf[nextTokenId] = msg.sender;
    balanceOf[msg.sender] += 1;
    nextTokenId++;
}

### Experience Points

```solidity
mapping(uint256 => uint256) public experience;

function addExperience(uint256 tokenId, uint256 amount) public {
    require(ownerOf[tokenId] == msg.sender, "Not the owner");
    experience[tokenId] += amount;
}

### Base URI for Metadata

```solidity
string public baseURI;

function setBaseURI(string memory newBaseURI) public onlyOwner {
    baseURI = newBaseURI;
}

### supportsInterface (ERC165)

```solidity
function supportsInterface(bytes4 interfaceId) public pure returns (bool) {
    return interfaceId == 0x80ac58cd || // ERC721
           interfaceId == 0x5b5e139f || // ERC721Metadata
           interfaceId == 0x01ffc9a7;    // ERC165
}

### NFT Name and Symbol

```solidity
string public name = "Base NFT Collection";
string public symbol = "BNFT";

### Safer ownerOf

```solidity
function ownerOf(uint256 tokenId) public view returns (address) {
    address owner = ownerOf[tokenId];
    require(owner != address(0), "Token does not exist");
    return owner;
}

### Token Status Enum

```solidity
enum TokenStatus { Normal, Locked, Staked }

mapping(uint256 => TokenStatus) public tokenStatus;

### Unlock Token Function

```solidity
function unlockToken(uint256 tokenId) public {
    require(ownerOf[tokenId] == msg.sender, "Not the owner");
    require(tokenStatus[tokenId] == TokenStatus.Locked, "Token is not locked");
    tokenStatus[tokenId] = TokenStatus.Normal;
}

### Simple NFT Staking

```solidity
mapping(uint256 => bool) public isStaked;
mapping(uint256 => uint256) public stakedAt;

function stake(uint256 tokenId) public {
    require(ownerOf[tokenId] == msg.sender, "Not the owner");
    require(!isStaked[tokenId], "Already staked");
    
    isStaked[tokenId] = true;
    stakedAt[tokenId] = block.timestamp;
    tokenStatus[tokenId] = TokenStatus.Staked;
}

### Staking Rewards Idea

```solidity
uint256 public rewardPerDay = 0.001 ether;

function calculateReward(uint256 tokenId) public view returns (uint256) {
    if (!isStaked[tokenId]) return 0;
    uint256 timeStaked = block.timestamp - stakedAt[tokenId];
    return (timeStaked * rewardPerDay) / 1 days;
}

### Reward Multiplier by Level

```solidity
function calculateReward(uint256 tokenId) public view returns (uint256) {
    if (!isStaked[tokenId]) return 0;
    
    uint256 timeStaked = block.timestamp - stakedAt[tokenId];
    uint256 baseReward = (timeStaked * rewardPerDay) / 1 days;
    
    // Higher level = higher rewards
    return baseReward * tokenLevel[tokenId];
}

### Minimum Staking Period

```solidity
uint256 public minStakeTime = 1 days;

function unstake(uint256 tokenId) public {
    require(ownerOf[tokenId] == msg.sender, "Not the owner");
    require(isStaked[tokenId], "Not staked");
    require(block.timestamp >= stakedAt[tokenId] + minStakeTime, "Minimum stake time not reached");
    
    isStaked[tokenId] = false;
    tokenStatus[tokenId] = TokenStatus.Normal;
}

### Early Unstake Penalty

```solidity
uint256 public earlyUnstakePenalty = 20; // 20%

function unstake(uint256 tokenId) public {
    require(ownerOf[tokenId] == msg.sender, "Not the owner");
    require(isStaked[tokenId], "Not staked");

    uint256 reward = calculateReward(tokenId);
    
    if (block.timestamp < stakedAt[tokenId] + minStakeTime) {
        reward = reward * (100 - earlyUnstakePenalty) / 100;
    }

    isStaked[tokenId] = false;
    tokenStatus[tokenId] = TokenStatus.Normal;
    
    if (reward > 0) {
        payable(msg.sender).transfer(reward);
    }
}

### Max Staked Per User

```solidity
uint256 public maxStakedPerUser = 5;
mapping(address => uint256) public stakedCount;

function stake(uint256 tokenId) public {
    require(ownerOf[tokenId] == msg.sender, "Not the owner");
    require(!isStaked[tokenId], "Already staked");
    require(stakedCount[msg.sender] < maxStakedPerUser, "Max staked reached");

    isStaked[tokenId] = true;
    stakedAt[tokenId] = block.timestamp;
    stakedCount[msg.sender] += 1;
    tokenStatus[tokenId] = TokenStatus.Staked;
}

### Get User Staked Tokens (Idea)

Since Solidity does not support returning dynamic arrays of all staked tokens easily without extra storage, one common approach is to keep an array per user:

```solidity
mapping(address => uint256[]) public userStakedTokens;

### Total Staked Counter

```solidity
uint256 public totalStaked;

function stake(uint256 tokenId) public {
    // existing checks...
    isStaked[tokenId] = true;
    totalStaked += 1;
    stakedCount[msg.sender] += 1;
}

### Total Rewards Distributed

```solidity
uint256 public totalRewardsDistributed;

function claimReward(uint256 tokenId) public {
    // existing logic...
    uint256 reward = calculateReward(tokenId);
    totalRewardsDistributed += reward;
    payable(msg.sender).transfer(reward);
}
### Emergency Pause for Staking

```solidity
bool public stakingPaused = false;

function stake(uint256 tokenId) public {
    require(!stakingPaused, "Staking is paused");
    // existing stake logic
}

### Reentrancy Protection Idea

```solidity
bool private locked;

modifier noReentrant() {
    require(!locked, "No reentrancy");
    locked = true;
    _;
    locked = false;
}

### Current Contract Summary

Features implemented so far:

- ERC721 basic functionality  
- Minting with payment and max supply  
- Metadata (tokenURI)  
- Levels and experience  
- Staking system with rewards  
- Pause controls  
- Access control (onlyOwner)

### Simple Breeding Idea

```solidity
function breed(uint256 tokenId1, uint256 tokenId2) public {
    require(ownerOf[tokenId1] == msg.sender, "Not owner of first token");
    require(ownerOf[tokenId2] == msg.sender, "Not owner of second token");
    require(tokenLevel[tokenId1] >= 3 && tokenLevel[tokenId2] >= 3, "Level too low");

    // Mint a new NFT
    ownerOf[nextTokenId] = msg.sender;
    balanceOf[msg.sender] += 1;
    tokenLevel[nextTokenId] = 1;
    nextTokenId++;
}

### Breeding Cost

```solidity
uint256 public breedingCost = 0.01 ether;

function breed(uint256 tokenId1, uint256 tokenId2) public payable {
    require(msg.value >= breedingCost, "Insufficient payment");
    // existing breeding logic
}

### Simple Attribute System

```solidity
struct Attributes {
    uint8 strength;
    uint8 agility;
    uint8 intelligence;
}

mapping(uint256 => Attributes) public tokenAttributes;

### Attribute Boost on Level Up

```solidity
function levelUp(uint256 tokenId) public {
    require(ownerOf[tokenId] == msg.sender, "Not the owner");
    require(tokenLevel[tokenId] < maxLevel, "Max level reached");
    
    tokenLevel[tokenId] += 1;
    
    // Small boost to attributes
    tokenAttributes[tokenId].strength += 1;
    tokenAttributes[tokenId].agility += 1;
    tokenAttributes[tokenId].intelligence += 1;
}
### Rarity Calculation

```solidity
function getRarity(uint256 tokenId) public view returns (string memory) {
    uint256 power = getPowerScore(tokenId);
    
    if (power >= 40) return "Legendary";
    if (power >= 30) return "Epic";
    if (power >= 20) return "Rare";
    return "Common";
}
### Simple Leaderboard Idea

```solidity
address[] public topPlayers;
mapping(address => uint256) public playerScore;

function updateScore(address player, uint256 newScore) public {
    playerScore[player] = newScore;
    // In a real implementation you would insert into a sorted leaderboard
}
### Simple Marketplace Listing

```solidity
struct Listing {
    address seller;
    uint256 price;
    bool active;
}

mapping(uint256 => Listing) public listings;

function listToken(uint256 tokenId, uint256 price) public {
    require(ownerOf[tokenId] == msg.sender, "Not the owner");
    require(price > 0, "Price must be greater than 0");
    
    listings[tokenId] = Listing(msg.sender, price, true);
}
### Marketplace Fee

```solidity
uint256 public marketplaceFee = 2; // 2%

function buyToken(uint256 tokenId) public payable {
    Listing memory item = listings[tokenId];
    require(item.active, "Listing not active");
    require(msg.value >= item.price, "Insufficient payment");

    uint256 fee = (item.price * marketplaceFee) / 100;
    uint256 sellerAmount = item.price - fee;

    listings[tokenId].active = false;

    // Transfer ownership...
    payable(item.seller).transfer(sellerAmount);
    // fee stays in the contract
}
### Listing Event

```solidity
event TokenListed(
    uint256 indexed tokenId,
    address indexed seller,
    uint256 price
);

function listToken(uint256 tokenId, uint256 price) public {
    require(ownerOf[tokenId] == msg.sender, "Not the owner");
    require(price > 0, "Price must be greater than 0");

    listings[tokenId] = Listing(msg.sender, price, true);
    emit TokenListed(tokenId, msg.sender, price);
}
### Offer System Idea

```solidity
struct Offer {
    address buyer;
    uint256 price;
    bool active;
}

mapping(uint256 => Offer) public offers;

function makeOffer(uint256 tokenId) public payable {
    require(msg.value > 0, "Offer must be greater than 0");
    offers[tokenId] = Offer(msg.sender, msg.value, true);
}
### Offer Events

```solidity
event OfferMade(uint256 indexed tokenId, address indexed buyer, uint256 price);
event OfferAccepted(uint256 indexed tokenId, address indexed seller, address indexed buyer, uint256 price);
event OfferCancelled(uint256 indexed tokenId, address indexed buyer);

// Emit these events in the corresponding functions

### Royalty Idea (ERC2981 style)

```solidity
address public royaltyReceiver;
uint256 public royaltyPercentage = 5; // 5%

function setRoyalty(address receiver, uint256 percentage) public onlyOwner {
    require(percentage <= 10, "Royalty too high");
    royaltyReceiver = receiver;
    royaltyPercentage = percentage;
}

### ERC2981 Support Idea

```solidity
function royaltyInfo(uint256, uint256 salePrice)
    external
    view
    returns (address receiver, uint256 royaltyAmount)
{
    royaltyAmount = (salePrice * royaltyPercentage) / 100;
    receiver = royaltyReceiver;
}

### Contract Overview

This contract currently includes:

- ERC721 NFT functionality  
- Minting system with payments  
- Levels, experience and attributes  
- Staking with rewards  
- Basic marketplace (listings + offers)  
- Royalties  
- Access control and pause features  

### Using ERC721URIStorage

```solidity
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";

contract MyNFT is ERC721URIStorage, Ownable {
    constructor() ERC721("MyNFT", "MNFT") Ownable(msg.sender) {}
}

### Using Pausable

```solidity
import "@openzeppelin/contracts/utils/Pausable.sol";

contract MyNFT is ERC721URIStorage, Ownable, Pausable {
    // ...
}

### Adding ERC2981 Royalties

```solidity
import "@openzeppelin/contracts/token/common/ERC2981.sol";

contract MyNFT is ERC721URIStorage, ERC2981, Ownable {
    constructor() ERC721("MyNFT", "MNFT") Ownable(msg.sender) {
        _setDefaultRoyalty(msg.sender, 500); // 5%
    }
}

### Full OpenZeppelin NFT Skeleton

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/token/common/ERC2981.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Pausable.sol";
import "@openzeppelin/contracts/utils/ReentrancyGuard.sol";

contract BaseNFT is ERC721URIStorage, ERC2981, Ownable, Pausable, ReentrancyGuard {
    uint256 public nextTokenId;
    uint256 public mintPrice = 0.01 ether;
    uint256 public maxSupply = 1000;

    constructor() ERC721("BaseNFT", "BNFT") Ownable(msg.sender) {
        _setDefaultRoyalty(msg.sender, 500);
    }
}

### Withdraw Function

```solidity
function withdraw() public onlyOwner nonReentrant {
    uint256 balance = address(this).balance;
    require(balance > 0, "No funds");

    (bool success, ) = payable(owner()).call{value: balance}("");
    require(success, "Withdraw failed");
}

### Allowlist (Whitelist) Idea

```solidity
mapping(address => bool) public allowlist;
bool public allowlistEnabled = true;

function mint(string memory uri) public payable whenNotPaused nonReentrant {
    if (allowlistEnabled) {
        require(allowlist[msg.sender], "Not in allowlist");
    }
    // rest of mint logic
}

### Merkle Proof Allowlist Idea

Instead of storing every address on-chain, it is more efficient to use a Merkle root:

```solidity
bytes32 public merkleRoot;

function setMerkleRoot(bytes32 _root) public onlyOwner {
    merkleRoot = _root;
}

### Revealed / Unrevealed Metadata

```solidity
bool public revealed = false;
string public hiddenURI;

function tokenURI(uint256 tokenId) public view override returns (string memory) {
    if (!revealed) {
        return hiddenURI;
    }
    return super.tokenURI(tokenId);
}

### Reveal Event

```solidity
event CollectionRevealed(string newBaseURI);

function reveal(string memory newBaseURI) public onlyOwner {
    revealed = true;
    emit CollectionRevealed(newBaseURI);
}
