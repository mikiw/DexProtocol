# DEX, Wyvern Protocol, SmartWeave Concepts Introduction
This documentation is a first draft of building a decentralized exchange of digital assets and knowledge economy on Arweave/SmartWeave with similar concepts as Wyvern Protocol has on Ethereum.

Firstly I'll explain the domain of digital asset exchanges, later I'll show the differences between Ethereum/Wyvern Protocol and Arweave/SmartWeave and finally I'll explain how to implement DEX using SmartWeave SDK v2.

## Centralized exchanges (CEX) vs decentralized exchanges (DEX)
Centralized exchanges (CEX) of assets like NYSE, Nasdaq or Binance are based on a trusted third party that helps conduct transactions for a known in advance fee. Clients of that exchanges submit orders generally with the price that they are willing to pay, the quantity of stocks/assets, type (market/limit), and expiration time. Later the matching algorithm based on the order book meets the buyer and seller.

It's a completely wrong way of doing things.

It's because this system works only on paper but the reality is completely different. The fundamental flaw of centralized exchanges is the lack of transparency and malversation.

In most cases you are not sending orders to the stock exchange, you use middleman brokers that have an advantage over you because they know what other client's orders look like.

This is just the beginning, here is the list of other malversations:
- Brokers can disable your option of buy/sell of the assets! Like GameStop/AMC case from Robinhood in from January and February 2021.
- Brokers can even close your positions if you are able to earn too much money on leverage positions like EXANTE or XTB (XTB - Polish Broker Forex & CFD)!
- You don't own your asset and there are different types of brokers so your assets can be less or more secure. It leads to other malversations this is why European Union has Directive 2014/65/EU, commonly known as MiFID 2.
- Information exploitation like Front-Running by brokers like Citadel Securities on its own clients between 2012 and 2014.
- Cancelation of short positions of hedge funds and investment banks on wheat/lithium that skyrockets after the war in Ukraine 24.2.2022! Cancelation of transactions is for investment banks, for you Mr. Smith there is a normal margin call!
- A hedge funds can loan money and use it for investment even if they can't cover this later.

Today we can build trustless decentralized exchanges (DEX) that are fully transparent and secure.

## CEX vs DEX problems and solutions
Decentralized exchanges based on old concepts like order books with only on-chain processing can't be efficient today. This can lead to a problem when a 1$ order can trigger a waterfall of gas costly orders worth 100$, which makes no sense. This is why we should move the matching algorithm off-chain and incentivize these 3rd party services somehow, maybe with a fee. With this approach on-chain logic contains only the swapping assets part of the overall exchange logic

We can also use a completely new way of automatic trading of digital assets which is automated market maker (AMM). An AMM is a way used to provide liquidity in decentralized finance (DeFi) which can replace a traditional concept of buyer and seller like in order book. Most common known protocol is [Uniswap v3](https://docs.uniswap.org/) but there is plenty of them.

It's a completely different way of creating a market of digital assets where prices of assets are determined by a constant mathematical formula (like XYK model). Now users are trading against the liquidity pool. Liquidity providers can be incentivized to add liquidity with part of the fee or with reward tokens. It sounds like a perfect way to get rich quickly, unfortunately, it's not because of phenomenon called impermanent loss.

## Wyvern Protocol
[Wyvern](https://wyvernprotocol.com/) is the most advanced decentralized exchange protocol designed to transfer Ethereum-based assets. Now you can exchange your Tokens, Assets, and CryptoKitties in a trustless way on [Wyvern Exchange](https://exchange.projectwyvern.com/ "Currently not working").

Currently, Wyvern supports:
- ERC-20 Tokens
- ERC-1155 Multi Tokens
- ERC-721 NFTs
- ERC-1271 off-chain signature validation assets
- All backward compatible tokens like ERC-621 and more

For now, it's enough but in the future new standards will be added according to Wyvern DAO. Also all predicates in Wyvern are arbitrary and they can be any asset or combination of assets. Just try to swap AAPL and GOOGL from Nasdaq for VISA on NYSE, good luck with that!

So how it works?
s
Firstly users create orders with an order scheme [order scheme](https://wyvernprotocol.com/docs#order-schema) and put them in the exchange registry and wait for a match and if so how much to fill it. Matching calldata can be constructed off-chain. Later, the first call is executed by the maker of the order through their proxy contract. The second call is executed by the counterparty.

Orders must always be authorized by the maker address, who owns the proxy contract which will perform the call. Authorization can be done in three ways: by signed message, by pre-approval, and by match-time approval. To optimize this process we can use the order book off-chain and if there is a match we can emit on-chain authorization.

In the end, a final order can be constructed and authorized by the maker of the order.

Wyvern documentation also mentions a special case with Ethereum which can only be sent from an account by a transaction.

The creators of the protocol don't mention too much about the matching algorithms and who will pay for its execution. We can only assume that sooner or later some application will be created to do so and people will run it locally to find a match for the orders. Also, maybe 3rd party paid services will be created to do so because they need to be incentivized somehow from a game theory perspective since they have no interest in doing it for free (or maybe it will be some charity project).

## SmartWeave
Although Wyvern on Ethereum seems to be a really interesting protocol to exchange ERC tokens like CryptoKitties but unfortunately it's very limited for building a highly scalable knowledge economy platform with homomorphic encryption around big data stored as [permaweb](https://arwiki.wiki/#/en/the-permaweb) or [IPFS](https://ipfs2arweave.com/).

It's mainly because of different requirements. We want to be able to store a variety of data with different sizes that only the owner has access to - not only standardized tokens (Wrapped AR can be used, more [here](https://medium.com/everfinance/what-is-wrapped-ar-c4b4375290b9)). Later we want to match clients with data processors that will process this data in a secure way. Finally, we want to build an economy around that process. This is why [Arweave](https://www.arweave.org/)/[SmartWeave SDK v2](https://smartweave.docs.redstone.finance/) with permaweb concepts seems to be a good fit to cover use cases like this:
- There are clients that want to calculate taxes in Canada with a pretty easy tax system, but they don't want to reveal how much they earn. There is a digitalized accounting company that is writing its own solution to calculate tax. The processor is processing encrypted data for clients for payment.
- There is a data provider with satellite data and the provider just wants to monetize this data without revealing it publicly. There is an entity specialized in image processing of that homomorphic image encryption data that doesn't want to reveal its algorithm but it wants to just give output with a number of ships/crops/etc. Finally, there is a hedge fund that it's interested in processed data of cargo ships or crops in the current year.
- There is a medical company or patient that wants to analyze medical data (like NGS DNA sequence) without revealing it. There is a biotech startup with an algorithm to do so that wants to monetize it. On our marketplace, buyers can meet the seller.

# SmartWeave Technical Insight
The purpose of that specification is to explain developers how to implement Wyvern like protocol on SmartWeave.

We will implement our DEX based on components listed below. They can be separate contracts or just state representation in one contract:
- `Owner` of DEX set in initial state, this is required for `Allowlist` tokens by DAO
- `Allowlist` of all possible ditial tokens assets that will be audited by DAO organization
- `Registry` of orders that entities want to perform
- `Deposits` of confirmed tokens transfers
- `Vault` as a proxy between maker and taker while they are performing asset exchange
- `Trades` that were executed

Our initial state will look like this:
```JSON
{
    "owner": "CONTRACT_CREATOR_ADDRESS",
    "allowlist": [],
    "registry": [],
    "deposits": [],
    "vault": [][],
    "trades": [],
}
```

Ecosystem can also include:
- Off-chain matching algorithm to inform entities about trade possibilities
- Fronted with Arweave wallet integration to perform actions in our ecosystem

## Assets
Although Arweave doesn't support ERC-20 like tokens we can assume that supported tokens must implement at least 
`transfer` and `balance` functions. We can assume that the implementation of that tokens will look like [this](https://github.com/ArweaveTeam/SmartWeave/blob/master/examples/token.js).

To execute these contracts functions in order to perform transfers from our vault contract we will need to audit them by the DAO organization and create a allowlist. We will provide two methods `AddToAllowlist(address: string)` and `RemoveFromAllowlist(address: string)` for storing and removing from `allowlist` contract in the vault contract state by vault contract owner (DAO). It's both a design and a security decision (We can also implement this DEX without DAO and `allowlist` if needed).

```Typescript
interface Allowlist {
    contracts: string[];
}
```

## Order type and Registry
To build decentralized order book exchange on SmartWeave smart contract protocol we need to specify our order schema.

|Name|Type|Purpose|
|----|-----|-------|
|maker|string|Address of order maker.|
|makerToken|string|Address of token to exchange by order maker.|
|makerTokenAmount|number|Amount of token to exchange by order maker.|
|takerToken|string|Address of token to exchange by taker.|
|takerTokenAmount|number|Amount of token to exchange by taker.|
|isActive|number|`True` or `False` on initialization. Order can be canceled in anytime by order maker.|
|isFilled|number|Order that was filled and it's inactive now.|
|expires|number|Order expiration in Unix Timestamp format (block.timestamp property).|
|txId|string|Transaction hash identifier to distinguish orders with same data. We can't add new `Order` if given txId is already in the registry.|

To define order structure we will use this `interface`:

```Typescript
interface Order {
    maker: string;
    makerToken: string;
    makerTokenAmount: number;
    takerToken: string;
    takerTokenAmount: number;
    isActive: boolean;
    isFilled: boolean;
    expires: number;
    txId: string;
}
```

`Registry` will be our on-chain state that will store all valid orders. We will need to implement function `RegisterOrder(order: Order)` to store order in `Registry`. Orders need to meet all requirements before adding like expiration time higher than the current time, tokens need to be on `allowlist`, `isFilled` must be false and two orders with the same `txId` can't exist. `ActivateOrder(orderTxId: string)` and `DeactivateOrder(orderTxId: string)` can be used to activate/deactivate order, keep in mind that you can't modify `isActive` anymore after order is filled.

```Typescript
interface Registry {
    orders: Order[];
}
```

Any entity will be able to read `Registry` to find matches in orders. Our clients will need to do it manually, run a matching algorithm off-chain or use 3rd party services to find matching orders.

To read our registry we can use this code snippet from SmartWeave SDK v2:
```Typescript
const contractId = "CONTRACT_ID";

// using SmartWeaveNodeFactory to quickly obtain fully configured, mem-cacheable SmartWeave instance
// see custom-client-example.ts for a more detailed explanation of all the core modules of the SmartWeave instance.
const smartweave = SmartWeaveNodeFactory.memCached(arweave);

// connecting to a given contract
const contract = smartweave.contract(contractId);
const { state, validity } = await cxyzContract.readState();
```

## Off-chain matching algorithm
Off-chain matching algorithm is not the main purpose of that specification but I advise to check [Machine Learning for Trading](https://www.udacity.com/course/machine-learning-for-trading--ud501) to understand how matching algorithms work especially on [this video](https://www.youtube.com/watch?v=lEBiyNojTqY).

## Vault
Vault will be our proxy for holding assets during an exchange. Firstly entity needs to send tokens to a contract vault, later it can perform a `DepositAsset(arweaveTxId: string)` function to increase its vault balance based on transaction Id. Our vault contract function will read transaction details and will add the balance of a specific token if it's on allowlist. After that, we will register that this deposit was already processed in `state.deposits` by adding there arweaveTxId. We can't prevent sending to vault address tokens that are not on allowlist so tokens like this will be lost forever and its design decision. We also need to agree on the number of confirmations that are needed to trust transaction.

If an entity will change its mind `WithdrawAsset(tokenAddress: string, walletAddress: string, amount: number)` function can be called and the vault contract will decrease the balance of that token and will transfer it to the indicated wallet address with `Contract.writeInteraction` function. This feature requires cross-contract calls, more about implementation can be found [here](https://github.com/redstone-finance/redstone-smartcontracts/tree/main/src/__tests__/integration/internal-writes). Implementation will need to perform multiple checks for example if the caller can withdraw the balance or if tokens are still on the allowlist. In rare cases, token contracts can be delisted for security reasons, in that case, we will need to wait for them to be on allowlist again, or ask DAO to handle that exception because we don't want to load malicious smart contracts into our contract memory during execution (this approach is recommended by SmartWeave team).

When all exchange criteria are met:
- Both orders are `active` and not `filled`
- Both orders are not in the `expired` state
- Both entities have enough funds in `vault`

We can perform an exchange of assets by executing `ExchangeAssets(callerTxId: string, takerTxId: string)` function by `maker` or `taker`. Now we will increase and decrease token balances of both entities in the vault and set `isFilled` in orders for `true`. After that we will add `Trade` record in `Trades`, first id which is `callerTxId` will be a caller `txId` of `ExchangeAssets()` method, second will be a `takerTxId`. After that action both entities can withdraw tokens with `WithdrawAsset(tokenAddress: string, walletAddress: string, amount: number)`.

```Typescript
interface Trade {
    callerTxId: string;
    takerTxId: string;
}

interface Trades {
    trades: Trade[];
}
```

```Typescript
Contract.writeInteraction
const token = smartweave.contract("TOKEN_CONTRACT_ADDRESS");

const result = await token.writeInteraction<any>({
    function: "transfer",
    data: {
        qty: QUANTITY,
    },
});
```

When the buyer meets the seller and both parties will authorize the transaction, successful trade can be performed in a decentralized trustless way. We can filter that filled orders using `isFilled` flag or by `Trades` for log proposuses.

## Useful links for Devs to read before implementing:
- [Software design by contract](https://en.wikipedia.org/wiki/Design_by_contract)
- [Introducing of SmartWeave](https://arweave.medium.com/introducing-smartweave-building-smart-contracts-with-arweave-1fc85cb3b632)
- [Building on SmartWeave v1 part #1](https://cedriking.medium.com/lets-buidl-smartweave-contracts-6353d22c4561)
- [Building on SmartWeave v1 part #2](https://cedriking.medium.com/lets-buidl-smartweave-contracts-2-16c904a8692d)
- [RedStone SmartContracts SDK](https://github.com/redstone-finance/redstone-smartcontracts)
- [Contract Writing Guide](https://github.com/ArweaveTeam/SmartWeave/blob/master/CONTRACT-GUIDE.md)
- [SmartWeave Protocol](https://github.com/redstone-finance/redstone-smartcontracts/blob/main/docs/SMARTWEAVE_PROTOCOL.md)
- [SmartWeave v2 SDK documentation](https://smartweave.docs.redstone.finance/#smartweave-sdk-v2)
- [Code examples for SmartWeave v2 SDK](https://github.com/redstone-finance/redstone-smartcontracts-examples)
- [Implementation tutorial for loot contract on SmartWeave SDK v2](https://github.com/redstone-finance/smartweave-loot/blob/main/docs/LOOT_CONTRACT_TUTORIAL.md)

## Smart Contract code snippet
This is code snippet based on this documentation:

```Typescript
export async function handle(state, action) {

    // state
    const owner = state.owner
    const allowlist = state.allowlist
    const registry = state.registry
    const deposits = state.registry
    const vault = state.vault
    const trades = state.trades

    switch (action.input.function) {

        case "AddToAllowlist": {
            const address = action.input.target

            // TODO: Implement required checks (add if not exists and only by owner).
            // TODO: If check will fail throw new ContractError.
            // TODO: Add to token contract address to allowlist.

            return { state };
        }

        case "RemoveFromAllowlist": {
            const address = action.input.target

            // TODO: Implement required checks (remove only if exists and only by owner).
            // TODO: If check will fail throw new ContractError.
            // TODO: Remove token contract address from allowlist.

            return { state };
        }

        case "RegisterOrder": {
            const order = action.input.target

            // TODO: Implement required checks like expiration time is higher than the current time
            // tokens need to be on allowlist, isFilled must be false and txId needs to be unique.
            // TODO: If check will fail throw new ContractError.
            // TODO: Add order to registry.

            return { state };
        }

        case "ActivateOrder": {
            const orderTxId = action.input.target

            // TODO: Implement required checks (find order by transaction id and check action.caller).
            // TODO: Check if order is not filled.
            // TODO: If check will fail throw new ContractError.
            // TODO: Set isActive to true.

            return { state };
        }

        case "DeactivateOrder": {
            const orderTxId = action.input.target

            // TODO: Implement required checks (find order by transaction id and check action.caller).
            // TODO: Check if order is not filled.
            // TODO: If check will fail throw new ContractError.
            // TODO: Set isActive to false.

            return { state };
        }

        case "DepositAsset": {
            const txId = action.input.target

            // TODO: Implement required checks for example check if token contract
            // is on allowlist from transaction details.
            // TODO: Read transaction details from blockchain and check confirmation number.
            // TODO: If check will fail throw new ContractError.
            // TODO: Increase the vault balance of digital asset for a given action.caller.
            // TODO: Update asset deposit registry with txId to avoid double deposit.

            return { state };
        }

        case "WithdrawAsset": {
            const withdrawDetails = action.input.target

            // TODO: Implement required checks (check if token contract is on allowlist).
            // TODO: Check if action.caller has enough balance of that asset.
            // TODO: If check will fail throw new ContractError.
            // TODO: Decrease the vault balance of digital asset for a given action.caller.

            return { state };
        }

        case "ExchangeAssets": {
            const exchange = action.input.target

            // TODO: Implement required checks for example check if orders are active
            // and not filled. Check if both orders are not in the expired state.
            // TODO: Check if entities have enough funds in vault.
            // TODO: Check if orders are complementary like 1A:10B and 10B:1A.
            // TODO: Change state of vault balances for both entities.
            // TODO: Add record to Trades array and set isFilled in orders.

            return { state };
        }

        default: {
            throw new ContractError(
            `Unsupported contract function: ${functionName}`);
        }
  }
}
```

## State example
This is state transition example for entities X, Y, Z and tokens A, B.

### Part 1 token distribution
```Typescript
Token A contract state:
state.balances[X] = 1000

Later actions:
transfer 10 token A from X to Y
transfer 10 token A from X to Z

New state of token A contact:
state.balances[X] = 980
state.balances[Y] = 10
state.balances[Z] = 10

Token B contract state:
state.balances[X] = 100

Later actions:
transfer 1 token B from X to Y
transfer 1 token B from X to Z

New state of token B contact:
state.balances[X] = 98
state.balances[Y] = 1
state.balances[Z] = 1
```

### Part 2 token orders in registry and vault deposit
```Typescript
Later actions:
entity Y register order to exchange 10x token A for 1x token B
entity Z register order to exchange 1x token B for 10 token A
off chain matching algorithm found a match!

Y sends 10 token A to V contract so state of token A contract looks like this:
state.balances[X] = 980
state.balances[Y] = 0
state.balances[Z] = 10
state.balances[V] = 10

Z sends 1 token B to V contract so state of token B contract looks like this:
state.balances[X] = 98
state.balances[Y] = 1
state.balances[Z] = 0
state.balances[V] = 1

Later actions:
entity Y register valut deposit balance of token A (DepositAsset(arweaveTxId: string))
entity Z register valut deposit balance of token B (DepositAsset(arweaveTxId: string))
```

```Typescript
Later actions for exchange scenario:
Vault state of V contract:
state.vault[token A][Y] = 10
state.vault[token B][Z] = 1
```

```Typescript
Later actions for withdrawing scenario after performing withdraw action by both 
entities wtih WithdrawAsset(tokenAddress: string, walletAddress: string, amount: number):

Token A contract will look like this:
state.balances[X] = 980
state.balances[Y] = 10
state.balances[Z] = 10
state.balances[V] = 0

Token B contract will look like this:
state.balances[X] = 98
state.balances[Y] = 1
state.balances[Z] = 1
state.balances[V] = 0

Vault state of V contract:
state.vault[token A][Y] = 0
state.vault[token B][Z] = 0
```

### Part 4 exchange scenario
```Typescript

Vault state of V contract after exchange:
state.vault[token B][Y] = 1
state.vault[token A][Z] = 10

Later state of tokens after withdraw of both entities:

Token A contract will look like this:
state.balances[X] = 980
state.balances[Y] = 0
state.balances[Z] = 20
state.balances[V] = 0

Token B contract will look like this:
state.balances[X] = 98
state.balances[Y] = 2
state.balances[Z] = 0
state.balances[V] = 0
```

## Others for future:
- TODO: Implement smart contract functions and tests it.
- TODO: Since SmartWeave smart contract system doesn't provide events and efficient filtering like in Ethereum, we need to run performance tests for high numbers of data like orders allowlist/registry/deposits/vault/trades and test if flags and soft delete is efficient enough.
- TODO: How to simulate these custom made exchanges/markets behaviors before launch? For example in Anylogic? Maybe [Dexter](https://dexter-manual.readthedocs.io/en/latest/)?
- TODO: Add more information about gas fees and who will pay for what.
- TODO: Add possibility to trade AR for tokens.
- TODO: Research possibility of a SmartWeave/Ethereum bridge.
- TODO: Research safe math in SmartWeave (buffer overflow scenarios etc.).
- TODO: Add predicate version of Order that can support multiply tokens types like on wyvern protocol.
- TODO: Extend this solution with AAM version for assets.
- TODO: Extend this solution with homomorphic encryption.
