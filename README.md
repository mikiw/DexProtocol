# DexProtocol
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

We can also use a completely new way of automatic trading of digital assets which is automated market maker (AMM). An AMM is a way used to provide liquidity in decentralized finance (DeFi) which can replace a traditional concept of buyer and seller like in order book. Most common known protocol is Uniswap v3 (https://docs.uniswap.org/) but there is plenty of them.

It's a completely different way of creating a market of digital assets where prices of assets are determined by a constant mathematical formula (like XYK model). Now users are trading against the liquidity pool. Liquidity providers can be incentivized to add liquidity with part of the fee or with reward tokens. It sounds like a perfect way to get rich quickly, unfortunately, it's not because of phenomenon called impermanent loss.

## Wyvern Protocol.

Wyvern (https://wyvernprotocol.com/ https://github.com/wyvernprotocol/wyvern-v3/tree/master/contracts) is the most advanced decentralized exchange protocol designed to transfer Ethereum-based assets. Now you can exchange your Tokens, Assets, and CryptoKitties in a trustless way on Wyvern Exchange (https://exchange.projectwyvern.com/ [Currently not working]).

Currently, Wyvern supports:
- ERC-20 Tokens
- ERC-1155 Multi Tokens
- ERC-721 NFTs
- ERC-1271 off-chain signature validation assets
- All backward compatible tokens like ERC-621 and more

For now, it's enough but in the future new standards will be added according to Wyvern DAO. Also all predicates in Wyvern are arbitrary and they can be any asset or combination of assets. Just try to swap AAPL and GOOGL from Nasdaq for VISA on NYSE, good luck with that!

So how it works?

Firstly users create orders with an order scheme (https://wyvernprotocol.com/docs#order-schema) and put them in the exchange registry and wait for a match and if so how much to fill it. Matching calldata can be constructed off-chain. Later, the first call is executed by the maker of the order through their proxy contract. The second call is executed by the counterparty.

Orders must always be authorized by the maker address, who owns the proxy contract which will perform the call. Authorization can be done in three ways: by signed message, by pre-approval, and by match-time approval. To optimize this process we can use the order book off-chain and if there is a match we can emit on-chain authorization.

In the end, a final order can be constructed and authorized by the maker of the order.

Wyvern documentation also mentions a special case with Ethereum which can only be sent from an account by a transaction.

The creators of the protocol don't mention too much about the matching algorithms and who will pay for its execution. We can only assume that sooner or later some application will be created to do so and people will run it locally to find a match for the orders. Also, maybe 3rd party paid services will be created to do so because they need to be incentivized somehow from a game theory perspective since they have no interest in doing it for free (or maybe it will be some charity project).

## SmartWeave
Although Wyvern on Ethereum seems to be a really interesting protocol to exchange ERC tokens like CryptoKitties but unfortunately it's very limited for building a highly scalable knowledge economy platform with homomorphic encryption around big data stored as permaweb (https://arwiki.wiki/#/en/the-permaweb) or IPFS (https://ipfs2arweave.com/).

It's mainly because of different requirements. We want to be able to store a variety of data with different sizes that only the owner has access to - not only standardized tokens (Wrapped AR can be used, more here https://medium.com/everfinance/what-is-wrapped-ar-c4b4375290b9). Later we want to match clients with data processors that will process this data in a secure way. Finally, we want to build an economy around that process. This is why Arweave/SmartWeave SDK v2 (https://smartweave.docs.redstone.finance/) with permaweb concepts seems to be a good fit to cover use cases like this:
- There are clients that want to calculate taxes in Canada with a pretty easy tax system, but they don't want to reveal how much they earn. There is a digitalized accounting company that is writing its own solution to calculate tax. The processor is processing encrypted data for clients for payment.
- There is a data provider with satellite data and the provider just wants to monetize this data without revealing it publicly. There is an entity specialized in image processing of that homomorphic image encryption data that doesn't want to reveal its algorithm but it wants to just give output with a number of ships/crops/etc. Finally, there is a hedge fund that it's interested in processed data of cargo ships or crops in the current year.
- There is a medical company or patient that wants to analyze medical data (like NGS DNA sequence) without revealing it. There is a biotech startup with an algorithm to do so that wants to monetize it. On our marketplace, buyers can meet the seller.

## SmartWeave Proposed solution
TODO: Write down the proposed solution but firstly we need to answer this question

The general question is what digital assets will be on this market and how they will be traded? Is it will be some standardized tokens or custom prepared results of homomorphic encryption execution or maybe both?

For now I can see these variants:
- Solution A) Economy market for assets based on the register and proxy smart contract on-chain and the off-chain matching algorithm.
- Solution B) Economy market for assets based on AAM.
- Solution C) Economy market for knowledge economy based on data and processing of it with the possibility of using homomorphic encryption concepts.
- Solution D) Any variation of A, B, or C.

## Others
- TODO: How to simulate these custom made blockchain/exchanges/markets behaviors before launch? For example in Anylogic? Maybe Dexter (https://dexter-manual.readthedocs.io/en/latest/)?
