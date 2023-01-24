### 9. Build an NFT Collection
* [ERC-721](https://docs.openzeppelin.com/contracts/3.x/api/token/erc721)
* #### Requirements:
	-   There should only exist 20 Crypto Dev NFT's and each one of them should be unique.
	-   User's should be able to mint only 1 NFT with one transaction.
	-   Whitelisted users, should have a 5 min presale period before the actual sale where they are guaranteed 1 NFT per transaction.
	-   There should be a website for your NFT Collection.
* [ERC721 Enumerable](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC721/extensions/ERC721Enumerable.sol): an extension of ERC721
	*  ERC721 Enumerable helps you to keep track of all the tokenIds in the contract and also the tokensIds held by an address for a given contract.

