#Title
# DegenToken
Simple Overview
DegenToken is an ERC20 token deployed on the Avalanche network for Degen Gaming. This token facilitates in-game rewards, item exchanges, and token management for players.

# Description
DegenToken is a custom ERC20 token created for Degen Gaming on the Avalanche network. The smart contract allows for minting, transferring, redeeming, checking balances, and burning tokens. Only the owner can mint new tokens, ensuring controlled distribution for rewards. Players can exchange tokens for in-game items, transfer tokens to other players, and manage their token balances. The contract also provides a method for burning tokens that are no longer needed

# Installing
To run this program, you will need to use Remix, an online Solidity IDE. 
Follow these steps to get started:
1.Visit Remix: Go to Remix.
2.Create a New File: Click on the "+" icon in the left-hand sidebar to create a new file.
3.Save the File: Save the file with a .sol extension (e.g., degen.sol).
4.Copy and Paste Code: Copy and paste the following code into the new file:

# Programe
/*
Challenge
Your task is to create a ERC20 token and deploy it on the Avalanche network for Degen Gaming. The smart contract should have the following functionality:
1. Minting new tokens: The platform should be able to create new tokens and distribute them to players as rewards. Only the owner can mint tokens.
2. Transferring tokens: Players should be able to transfer their tokens to others.
3. Redeeming tokens: Players should be able to redeem their tokens for items in the in-game store.
4. Checking token balance: Players should be able to check their token balance at any time.
5. Burning tokens: Anyone should be able to burn tokens, that they own, that are no longer needed.
*/
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.23;
pragma solidity ^0.8.24;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";

contract DegenToken is ERC20, Ownable {

    uint256 public constant EXCHANGE_RATE = 100;
contract TokenDegen is ERC20, Ownable, ERC20Burnable {

    mapping(address => uint256) public shieldsOwned;
    constructor() ERC20("Degen", "DGN") Ownable(msg.sender) {}

    constructor() ERC20("Degen", "DGN") Ownable(msg.sender) {
        _mint(msg.sender, 10 * (10 ** uint256(decimals())));
    }
    // Enum for collectible items
    enum Collectibles {Common, Uncommon, Rare, UltraRare, Legendary}

    function exchangeShield(uint256 amount) public {
        uint256 totalCost = EXCHANGE_RATE * amount;
        require(balanceOf(msg.sender) >= totalCost, "Insufficient tokens to exchange for a shield");
    struct Buyer {
        address buyerAddress;
        uint quantity;
    }
    // Queue for buyers wanting to purchase TokenDegen
    Buyer[] public buyerQueue;

        shieldsOwned[msg.sender] += amount;
        _burn(msg.sender, totalCost);
    struct UserCollectibles {
        uint common;
        uint uncommon;
        uint rare;
        uint ultraRare;
        uint legend;
    }

    function viewShieldsOwned(address holder) public view returns (uint256) {
        return shieldsOwned[holder];
    // Mapping to store redeemed collectibles
    mapping(address => UserCollectibles) public userCollectibles;

    function purchaseTokens(address _buyerAddress, uint _quantity) public {
        buyerQueue.push(Buyer({buyerAddress: _buyerAddress, quantity: _quantity}));
    }

    function generateTokens(address recipient, uint256 quantity) public onlyOwner {
        _mint(recipient, quantity);
    // Mint tokens for buyers in the queue
    function mintTokens() public onlyOwner {
        // Loop to mint tokens for buyers in the queue
        while (buyerQueue.length != 0) {
            uint index = buyerQueue.length - 1;
            if (buyerQueue[index].buyerAddress != address(0)) { // Check for non-zero address
                _mint(buyerQueue[index].buyerAddress, buyerQueue[index].quantity);
                buyerQueue.pop();
            }
        }
    }

    // Transfer tokens to another user
    function transferTokens(address _recipient, uint _quantity) public {
        require(_quantity <= balanceOf(msg.sender), "Insufficient balance");
        _transfer(msg.sender, _recipient, _quantity);
    }

    function viewBalance(address holder) public view returns (uint256) {
        return balanceOf(holder);
    // Redeem different collectibles
    function redeemCollectibles(Collectibles _collectible) public {
        if (_collectible == Collectibles.Common) {
            require(balanceOf(msg.sender) >= 15, "Insufficient balance");
            userCollectibles[msg.sender].common += 1;
            burn(15);
        } else if (_collectible == Collectibles.Uncommon) {
            require(balanceOf(msg.sender) >= 25, "Insufficient balance");
            userCollectibles[msg.sender].uncommon += 1;
            burn(25);
        } else if (_collectible == Collectibles.Rare) {
            require(balanceOf(msg.sender) >= 35, "Insufficient balance");
            userCollectibles[msg.sender].rare += 1;
            burn(35);
        } else if (_collectible == Collectibles.UltraRare) {
            require(balanceOf(msg.sender) >= 45, "Insufficient balance");
            userCollectibles[msg.sender].ultraRare += 1;
            burn(45);
        } else if (_collectible == Collectibles.Legendary) {
            require(balanceOf(msg.sender) >= 60, "Insufficient balance");
            userCollectibles[msg.sender].legend += 1;
            burn(60);
        } else {
            revert("Invalid collectible selected");
        }
    }

    function destroyTokens(uint256 quantity) public {
        require(balanceOf(msg.sender) >= quantity, "Insufficient tokens to burn");
        _burn(msg.sender, quantity);
    // Function to burn tokens
    function burnTokens(address _holder, uint _quantity) public {
        _burn(_holder, _quantity);
    }

    function transferHoldings(address recipient, uint256 quantity) public {
        require(recipient != address(0), "Invalid recipient address");
        require(balanceOf(msg.sender) >= quantity, "Insufficient tokens to transfer");
        _transfer(msg.sender, recipient, quantity);
    // Function to check the balance of tokens
    function getBalance() public view returns (uint) {
        return balanceOf(msg.sender);
    }
}
# Help
Common Issues:
Insufficient Tokens for Exchange or Transfer: Ensure you have enough tokens in your balance before performing exchange or transfer operations. Use checkYourBalance to check your balance.
Invalid Recipient Address: When transferring tokens, make sure the recipient address is valid and not the zero address.
# Authors
Arpit https://github.com/Arpit554

# License
This project is licensed under the MIT License - see the LICENSE file for details.
