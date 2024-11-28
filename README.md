# DegenToken Smart Contract

This Solidity program represents the `DegenToken` smart contract, which is an ERC20 token with additional features like minting, burning, and redeeming game rewards. The contract is designed to allow players to earn rewards in the form of in-game items by spending their tokens, with a minting function controlled by the contract owner.

## Description

The `DegenToken` contract is an implementation of an ERC20 token with extended functionality to mint, burn, and redeem tokens for various in-game rewards. The key features of this contract include:

- **Minting**: The contract owner can mint new tokens to specified addresses.
- **Burning**: Token holders can burn tokens from their own balance.
- **Rewards Store**: Users can redeem tokens for in-game items, including outfits, weapons, and shields.
- **Event Emission**: Events are emitted for important actions, such as minting, burning, and redeeming rewards.

### Key Features:
- **Minting**: Only the owner can mint new tokens, which are then transferred to the specified address.
- **Burning**: Token holders can burn tokens from their balance, reducing their total supply.
- **Redeem Rewards**: Players can redeem their tokens for specific in-game rewards (e.g., a strong sword or a big shield).
- **Event Logging**: The contract emits events for minting, burning, and redeeming rewards to track actions.

## Getting Started

### Executing Program

To deploy and interact with this contract, you can use **Remix**, an online Solidity IDE. Follow the steps below to get started:

1. Go to the [Remix website](https://remix.ethereum.org/).
2. In the left-hand sidebar, click the "+" icon to create a new file.
3. Save the file with a `.sol` extension (e.g., `DegenToken.sol`).
4. Copy and paste the following code into the file:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";

contract DegenToken is ERC20, Ownable, ERC20Burnable {
    mapping(address => bool) private _banned;

    event MintToken(address indexed to, uint256 value);
    event BurnToken(address indexed from, uint256 value);
    event RedeemGameReward(address indexed player, string itemName);

    constructor() ERC20("Degen", "DGN") {}

    function mint(address to, uint256 amount) external onlyOwner  {
        _mint(to, amount);
        emit MintToken(to, amount);
    }

    function burntokens(uint256 amount) external {
        require(balanceOf(msg.sender)>= amount, "You do not have enough Tokens in your account");
        _burn(msg.sender, amount);
        emit BurnToken(msg.sender, amount);
    }

    function RewardsStore() public pure returns(string memory) {
        return "1. Expensive Outfit (value = 500 Degen)\n2. Strong Sword (value = 300 Degen)\n3. Big Shield (value = 100 Degen)";
    }

    function redeem(uint256 choice) external {
        require(choice <= 3, "Invalid selection");

        if (choice == 1) {
            require(balanceOf(msg.sender) >= 500, "Insufficient balance");
            approve(msg.sender, 500);
            transferFrom(msg.sender, owner(), 500);
            emit RedeemGameReward(msg.sender, "Expensive Outfit");
        } else if (choice == 2) {
            require(balanceOf(msg.sender) >= 300, "Insufficient balance");
            approve(msg.sender, 300);
            transferFrom(msg.sender, owner(), 300);
            emit RedeemGameReward(msg.sender, "Strong Sword");
        } else if (choice == 3) {
            require(balanceOf(msg.sender) >= 100, "Insufficient balance");
            approve(msg.sender, 100);
            transferFrom(msg.sender, owner(), 100);
            emit RedeemGameReward(msg.sender, "Big Shield");
        }
    }

    function checkBalance() external view returns(uint){
        return balanceOf(msg.sender);
    }

    function transfer(address reciever, uint256 amount) public override returns (bool) {
        return super.transfer(reciever, amount);
    }

    function transferFrom(address sender, address reciever, uint256 amount) public override returns (bool) {
        approve(msg.sender, amount);
        return super.transferFrom(sender, reciever, amount);
    }
}
```

### Compilation and Deployment

1. To compile the code, click on the "Solidity Compiler" tab in the left-hand sidebar of Remix.
2. Ensure the compiler version is set to `0.8.0` (or another compatible version), and click "Compile DegenToken.sol."
3. Once the contract is compiled, go to the "Deploy & Run Transactions" tab in Remix.
4. Select the `DegenToken` contract from the dropdown menu and click the "Deploy" button.

### Interacting with the Contract

1. **Minting Tokens** (Owner Only):
   - To mint new tokens, call the `mint` function with the address to mint to and the amount of tokens.
   - Example: `mint(0xAddress, 1000)` to mint 1000 tokens to `0xAddress`.

2. **Burning Tokens**:
   - Token holders can burn tokens from their balance by calling the `burntokens` function, specifying the amount to burn.
   - Example: `burntokens(100)` will burn 100 tokens from the caller's account.

3. **Redeeming Rewards**:
   - Call the `redeem` function with the choice of item to redeem (1 = Expensive Outfit, 2 = Strong Sword, 3 = Big Shield).
   - Example: `redeem(1)` will attempt to redeem an "Expensive Outfit" if the caller has sufficient balance.

4. **Check Balance**:
   - Use the `checkBalance` function to check the caller's current token balance.
   - Example: `checkBalance()` will return the caller's current token balance.

5. **Reward Store**:
   - Use the `RewardsStore` function to view the available rewards and their costs.
   - Example: `RewardsStore()` will return a list of available items with their token costs.

6. **Token Transfers**:
   - The contract allows for token transfers between addresses using the `transfer` and `transferFrom` functions, standard ERC20 functionality.

## Events

The contract emits the following events:

- `MintToken`: Triggered when new tokens are minted, with the recipient and amount.
- `BurnToken`: Triggered when tokens are burned, with the sender and amount.
- `RedeemGameReward`: Triggered when a user redeems a reward, with the player's address and the reward item name.

## Conclusion

The `DegenToken` contract provides a customizable token for in-game economies, where players can earn rewards by spending tokens. The contract features minting, burning, and token redemption capabilities, as well as event tracking for transparency and security. It can be further enhanced with features like banning addresses, access control, or integration with a frontend application for user interaction.
