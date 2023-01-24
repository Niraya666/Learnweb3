### 12. Diving into Decentralized Exchanges (Uniswap V1)

* Mt.Gox
* QuadrigaCX
#### Decentralized Exchanges
>The idea of a decentralized exchange is simple - allow users to trade their crypto directly on-chain through smart contracts without giving up control of their private keys.

* [Uniswap](https://uniswap.org/)
* Uniswap V1, V2, V3
	1. **V1**: it allowed only swaps between ether and a token; Chained swaps were also possible to allow token-token swaps(swap by first swapping one of them for ETH, and then swapping the ETH for the second token.).
	2. **V2**: allowed direct swaps between any ERC20 tokens, as well as chained swaps between any pairs
	3. **V3**: allowed liquidity providers to remove a bigger portion of their liquidity from pools and still keep getting the same rewards
*  Market Makers
	> **Market Makers** are entities that provide liquidity (assets) to trading markets.

	* A DEX must have enough liquidity to function and serve as an alternative to centralized exchanges.
	* **let anyone be a market maker**
* Functional Requirements
	**an automated market maker:**:
	* Anyone can add liquidity to become a liquidity provider
	* Liquidity providers can remove their liquidity and get back their crypto whenever they want
	* Users can swap between assets present in the trading pool, assuming there is enough liquidity
	* Users are charged a small trading fees, that gets distributed amongst the liquidity providers so they can earn for providing liquidity
$$XY = K$$
**x** = reserve balance of `ETH` in the trading pool

**y** = reserve balance of `LW3 Token` in the trading pool

**k** = a constant

We can simplify the above equation to solve for `Δy`, and we get the following formula:

`Δy = (y * Δx) / (x + Δx)`
```js
function calculateOutputAmount(uint inputAmount, uint inputReserve, uint outputReserve) private pure returns (uint) {

    uint outputAmount = (outputReserve * inputAmount) / (inputReserve + inputAmount);

    return outputAmount;

}
```
![](https://i.imgur.com/Lo9CAct.png)

* **Slippage**
>The bigger the amount of tokens being traded relative to their reserve values, the lower the price would be.
>**This also aligns with the law of supply and demand: the higher the demand relative to the supply, the more costly it is to buy that supply.**


* Who sets the initial price?
	* the first person adding liquidity to the pool gets to set a price
		>  **Adding liquidity involves adding tokens from both sides of the trading pair - you cannot add liquidity for just one side**.

	`addLiquidity`
	```js
	function addLiquidity(uint tokenAmount) public payable {

    // assuming a hypothetical function

    // that returns the balance of the

    // token in the contract

    if (getReserve() == 0) {

        IERC20 token = IERC20(tokenAddress);

        token.transferFrom(msg.sender, address(this), tokenAmount);

    } else {

        uint ethReserve = address(this).balance - msg.value;

        uint tokenReserve = getReserve();

        uint proportionalTokenAmount = (msg.value * tokenReserve) / ethReserve;

        require(tokenAmount >= proportionalTokenAmount, "incorrect ratio of tokens provided");

        IERC20 token = IERC20(tokenAddress);

        token.transferFrom(msg.sender, address(this), proportionalTokenAmount);

    }

}

```

* LP Tokens
>We need a way to reward the liquidity providers for their tokens, as without them other users would not have been able to perform swaps. Nobody would put tokens in a third-party contract if they are not getting something out of it.
>The only good solution for this is to collect a small fee on each token swap and distribute the fees amongst the liquidity providers, based on how much liquidity they provided.

**Liquidity Provider Tokens (LP Tokens)**
LP Tokens work as shares.

1.  You get LP-tokens in exchange for your liquidity
2.  Amount of tokens you get is proportional to your share of the liquidity in the pool
3.  Fees are distributed proportional to how many LP-tokens you own
4.  LP-tokens can be exchanged back for the liquidity + earned fees
**So, how do we calculate the amount of LP-tokens to be minted when liquidity is added?**
Uniswap V1 calculates the amount proportionate to the ETH reserve. The following equation shows how the amount of new LP-tokens is calculated depending on how much ETH is deposited:

`amountMinted = totalAmount * (ethDeposited / ethReserve)`

update `addLiquidity` function
```js
function addLiquidity(uint tokenAmount) public payable {

    if (getReserve() == 0) {

        ...

        uint liquidity = address(this).balance;

        _mint(msg.sender, liquidity);

    } else {

        ...

        uint liquidity = (totalSupply() * msg.value) / ethReserve;

        _mint(msg.sender, liquidity);

    }

}
```

* ### Fees
