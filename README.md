# Poly-Proof-NFT-Bridge (Connecting Sepolia to Amoy Testnet)

This repository implements a bridge for an NFT collection, transferring it from the Ethereum Sepolia testnet to the Polygon Amoy testnet. The NFT collection is mapped to Polygon and transferred via the Polygon Bridge. Adding a unique twist, this project uses image generation tools like DALL-E 2 or Midjourney to create the artwork for the NFTs.

## Description

The aim of this project is to create and deploy an NFT collection on the Ethereum Sepolia testnet, then map it to the Polygon Amoy testnet and transfer the assets using the Polygon Bridge. The NFTs are created using AI tools like DALL-E 2 or Midjourney, stored on IPFS, and minted via smart contracts, utilizing Hardhat for deployment and transfer.

## Prerequisites

- [MetaMask](https://metamask.io/) browser extension
- [fxPortal Starter](https://github.com/Metacrafters/fxPortalStarter) for compiling and deploying smart contracts
- [Pinata Cloud](https://www.pinata.cloud/) for storing generated images on IPFS
- An AI image generation tool like DALL-E, Midjourney, Leonardo AI, or Lexica Art
- Visual Studio Code or any preferred development environment

## Getting Started

### Running the program

1. You can use either VS Code or GitPod to build and execute this project.
2. Click the "+" icon in the left-hand sidebar to create a new file.
3. Clone this repository.
4. Write and deploy an ERC721 or ERC1155 contract.

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import "erc721a/contracts/ERC721A.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract helloNFT is ERC721A, Ownable {
    uint256 public totalMinted;
    mapping(uint256 => string) private _nftMetadata;
    mapping(uint256 => string) private _nftPrompts;

    // Constructor initializes the NFT collection with a name and symbol
    constructor() ERC721A("hello", "hlo") Ownable(msg.sender) {
        totalMinted = 0;
    }

    // Function to mint multiple NFTs with metadata and descriptions
    function mintMultipleNFTs(string[] memory metadataURIs, string[] memory descriptions) external onlyOwner {
        require(metadataURIs.length == descriptions.length, "Both arrays must be of the same length");

        uint256 startId = totalMinted;
        uint256 numOfNFTs = metadataURIs.length;

        // Mint the NFTs to the owner of the contract
        _safeMint(owner(), numOfNFTs);

        // Assign metadata and prompts to each new NFT
        for (uint256 i = 0; i < numOfNFTs; i++) {
            _nftMetadata[startId + i] = metadataURIs[i];
            _nftPrompts[startId + i] = descriptions[i];
        }

        totalMinted += numOfNFTs;
    }

    // Function to return the description prompt of a given token ID
    function promptDescription(uint256 tokenId) external view returns (string memory) {
        require(_exists(tokenId), "Token does not exist");
        return _nftPrompts[tokenId];
    }

    // Function to return the metadata URI for a given token ID
    function tokenURI(uint256 tokenId) public view override returns (string memory) {
        require(_exists(tokenId), "Token does not exist");
        return _nftMetadata[tokenId];
    }
}
;
```

## Installation

1. Fork this repository and clone it to your local system.
2. Make sure to install Node.js before running the project.
3. Install all dependencies by running:
```bash
npm install
```
4. Configure your `.env` file with your RPC URLs and wallet private key.

### Running the program

1. Compile the contract:
```bash
npx hardhat compile
```

2. Deploy the contract on the Sepolia network:
```bash
npx hardhat run scripts/deploy.js --network sepolia
```

3. Mint the NFTs on the Sepolia network:
```bash
npx hardhat run scripts/mintNFT.js --network sepolia
```

4. Deposit the NFTs for bridging from Sepolia to Amoy using this command:
```bash
npx hardhat run scripts/approveDeposit.js --network sepolia
```

5. Wait 20-30 minutes for the bridging process, then copy the contract address on the Amoy network once the NFTs have been received.

6. After receiving the bridged NFTs, run `getBalance.js` on the Amoy network to verify the number of NFTs transferred:
```bash
npx hardhat run scripts/getBalance.js --network amoy
```

## Connecting MetaMask to the Sepolia Testnet

1. Open MetaMask and select the network dropdown.
2. Click "Add Network" and enter the following details:
    - **Network Name:** Sepolia Testnet
    - **New RPC URL:** (https://sepolia.infura.io/v3/)
    - **Chain ID:** 11155111
    - **Currency Symbol:** SepoliaETH
3. Save and switch to the Sepolia network.

## Connecting MetaMask to the Polygon Amoy Testnet

1. Open MetaMask and select the network dropdown.
2. Click "Add Network" and fill in the following details:
    - **Network Name:** Polygon Amoy Testnet
    - **New RPC URL:** (https://rpc-amoy.polygon.technology/)
    - **Chain ID:** 80002
    - **Currency Symbol:** MATIC
3. Save and switch to the Amoy network.

## Verifying Contract on PolygonScan

1. Visit Sepolia EtherScan(https://sepolia.etherscan.io/).
2. Search for your contract address and complete the verification.

## Obtain Bridged Wallet Address on Amoy PolygonScan

1. Visit PolygonScan(https://amoy.polygonscan.com/).
2. Search for your contract address and complete the verification.

## Faucet Links

- Sepolia Faucet: https://cloud.google.com/application/web3/faucet/ethereum/sepolia
- Amoy Faucet: https://faucet.polygon.technology/ (Polygon Discord membership required)

```
npx hardhat help
```

## Author

Parth Tiwari


Let me know if you'd like to adjust anything further!