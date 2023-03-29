# Dibs Integration Guide for Uniswap V2 Style Decentralized Exchanges

To integrate Dibs into a Uniswap V2 style decentralized exchange, you need to fulfill certain requirements for each of the following contracts, which are typically included in a Uniswap V2 DEX:

- Router
- PairFactory

For the Dibs system to function properly, meet the following requirements for each contract:

## Router

The router must have the following views:

- `factory`, which points to the PairFactory contract
- `getAmountsOut(uint amountIn, route[] memory routes) public view returns (uint[] memory amounts)`, which, given a swap route, returns all amountOuts. Note that the "route" struct is defined as follows: `{address from; address to; bool stable;}`
- `weth()/wETH()`, which points to the wrapped native token of the chain. Please note that a ChainLink price feed for wETH must be available on the chain.

Additionally, the router contract must emit Swap events with the following data:

- `event Swap(address indexed sender, uint256 amount0In, address tokenIn, address indexed to, bool stable, uint256 feeAmount);`

where "feeAmount" is the total fees generated by the swap. Afterward, you should transfer `feeAmount * MAX_DIBS_FEE_PERCENTAGE` to the Dibs contract.

If you want to enable features such as rewards based on referrals for a specific pair, you must also include the route in the event:

- `event Swap(address indexed sender, uint amount0In, address _tokenIn, address indexed to, bool stable, uint256 feeAmount, route[] routes);`

## PairFactory

The PairFactory contract must emit a PairCreated event whenever a new pair is created:

- `event PairCreated(address indexed token0, address indexed token1, bool stable, address pair, uint256);`
