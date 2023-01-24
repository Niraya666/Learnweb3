### 10. Chainlink VRFs
>trusted random numbers cannot be generated natively in Solidity because randomness will be calculated on-chain which is public info to all the miners and the users.
>we can use some web2 technologies to generate the randomness and then use them on-chain.
https://zh.chain.link/vrf

[How can I securely generate a random number in my smart contract?](https://ethereum.stackexchange.com/questions/191/how-can-i-securely-generate-a-random-number-in-my-smart-contract)

[When can BLOCKHASH be safely used for a random number? When would it be unsafe?](https://ethereum.stackexchange.com/q/419/42)

[智能合约中的随机数](https://zhuanlan.zhihu.com/p/50219222)


https://docs.chain.link/docs/vrf/v2/introduction/


#### What is an oracle?

* An oracle sends data from the outside world to a blockchain's smart contract and vice-verca.
* Smart contract can then use this data to make a decision and change its state.
* They act as bridges between blockchains and the external world.
* However it is important to note that the blockchain oracle is not itself the data source but its job is to query, verify and authenticate the outside data and then futher pass it to the smart contract.

#### Intro
-   Chainlink VRF's are oracles which used to generate random values.
-   These values are verified using cryptographic proofs.
-   These proofs prove that the results weren't tampered or manipulated by oracle operators, users, miners etc.
-   Proofs are published on-chain so that they can be verified.
-   After there verification is successful they are used by smart contracts which requested randomness.
>Chainlink VRF (Verifiable Random Function) is a provably-fair and verifiable source of randomness designed for smart contracts. Smart contract developers can use Chainlink VRF as a tamper-proof random number generator (RNG) to build reliable smart contracts for any applications which rely on unpredictable outcomes.

#### How does it work?

-   Chainlink has two contracts that we are mostly concerned about [VRFConsumerBase.sol](https://github.com/smartcontractkit/chainlink/blob/master/contracts/src/v0.8/VRFConsumerBase.sol) and VRFCoordinator
-   VRFConsumerBase is the contract that will be calling the VRF Coordinator which is finally reponsible for publishing the randomness
- VRFConsumerBase using two functions from it:
	- requestRandomness, which makes the initial request for randomness.
	- fulfillRandomness, which is the function that receives and does something with verified randomness.
![](https://i.imgur.com/ssQTlkc.png)


## Requirements

-   We will build a lottery game today
-   Each game will have a max number of players and an entry fee
-   After max number of players have entered the game, one winner is chosen at random
-   The winner will get `maxplayers*entryfee` amount of ether for winning the game


