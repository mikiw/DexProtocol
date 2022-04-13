# DexProtocol
This documentation is a first draft of building a decentralized exchange of knowledge economy on Arweave/SmartWeave with similar concepts as Wyvern Protocol has on Ethereum.

Firstly I'll explain the domain of asset exchanges, later I'll show the differences between Ethereum/Wyvern Protocol and Arweave/SmartWeave and finally I'll explain how to implement DEX using SmartWeave SDK v2.

## Centralized exchanges (CEX) vs decentralized exchanges (DEX).
Centralized exchanges (CEX) of assets like NYSE, Nasdaq or Binance are based on a trusted third party that helps conduct transactions for a known in advance fee. Clients of that exchanges submit orders generally with the price that they are willing to pay, the quantity of stocks/assets, type (market/limit), and expiration time. Later the matching algorithm based on the order book meets the buyer and seller.

It's a completely wrong way of doing things.

It's because this system works only on paper but the reality is completely different. The fundamental flaw of centralized exchanges is the lack of transparency and malversation.

In most cases you are not sending orders to the stock exchange, you use middleman brokers that have an advantage over you because they know what other client's orders look like.

This is just the beginning, here is the list of other malversations:
- Brokers can disable your option of buy/sell of the assets! Like GameStop/AMC case from Robinhood in from January and February 2021.
- Brokers can even close your positions if you are able to earn too much money on leverage positions like EXANTE or XTB (XTB - Polish Broker Forex & CFD)!
- You don't own your asset and there are different types of brokers so your assets can be less or more secure. It leads to other malversations this is why European Union has Directive 2014/65/EU, commonly known as MiFID 2.
- Information exploitation like Front-Running by brokers like Citadel Securities on its own clients between 2012 and 2014.
- Cancelation of short positions of hedge funds and investment banks on wheat/lithium that skyrockets after the war in Ukraine 24.2.2022. Cancelation of transactions is for investment banks, for you Mr. Smith there is a normal margin call!
- A hedge funds can loan money and use it for investment even if they can't cover this later.

Today we can build trustless decentralized exchanges (DEX) that are fully transparent and secure.

## CEX vs DEX Problems.
TODO: Write down core problems with moving from centralized to decentralized exchanges like:
- Orderbooks vs AAM
- If we will have an order book in DEX who will pay for gas? What if small order for 1$ will trigger a waterfall of other orders for 100000$, who will pay the gas fee?
- Off-chain vs on-chain and game theory behind that in both cases in case of market makers
- How arbitration of off-chain market makers will work in a decentralized world? Will it be worth competing from a game theory perspective?
- Read more about matching algorithms on exchanges.
- vs decentralized like Uniswap, dYdX.
- TODO: Read more about DeFi's, DEXs and AMMs and write down core concepts.

## Solutions.
- TODO: Brief introduction of solutions for given problems.
- TODO: What is Asset/Order data structure? What contracts are needed? Or how custom-made blockchain will look like?
- TODO: Orders with a lump sum fee for all like in a centralized world (NYSE/Nasdaq/Binance) that the client needs to pay?
- TODO: Competing market makers with a lump sum or dynamic fees?

## Wyvern Protocol.

Wyvern (https://wyvernprotocol.com/ https://github.com/wyvernprotocol/wyvern-v3/tree/master/contracts) is the most advanced decentralized exchange protocol designed to transfer Ethereum-based assets. Now you can exchange your Tokens, Assets, and CryptoKitties in a trustless way on Wyvern Exchange (https://exchange.projectwyvern.com/ [Currently not working]).

Currently, Wyvern supports ERC-20 Tokens, ERC-1155 Multi Tokens, and ERC-721 NFTs so it's enough for now. In the future new standards will be added according to Wyvern DAO.

Predicates in Wyvern are arbitrary and they can be any asset or combination of assets. Just try to swap AAPL and GOOGL from Nasdaq for VISA on NYSE, good luck with that!

So how it works?

Firstly users create orders with an order scheme (https://wyvernprotocol.com/docs#order-schema) and put them in the exchange registry and wait for a match and if so how much to fill it. Matching calldata can be constructed off-chain. Later, the first call is executed by the maker of the order through their proxy contract. The second call is executed by the counterparty.

Orders must always be authorized by the maker address, who owns the proxy contract which will perform the call. Authorization can be done in three ways: by signed message, by pre-approval, and by match-time approval. To optimize this process we can use the order book off-chain and if there is a match we can emit on-chain authorization.

In the end, a final order can be constructed and authorized by the maker of the order.

Wyvern documentation also mentions a special case with Ethereum which can only be sent from an account by a transaction.

The creators of the protocol don't mention too much about the matching algorithms and who will pay for its execution. We can only assume that sooner or later some application will be created to do so and people will run it locally to find a match for the orders. Also, maybe 3rd party paid services will be created to do so because they need to be incentivized somehow from a game theory perspective since they have no interest in doing it for free (or maybe it will be some charity project).

Questions:
- TODO: Is it true that ERC-20/ERC-721/ERC-1155 only?
- TODO: ERC-1271 - what is that?
- TODO: If I have 2 tokens on ERC-621 can I swap them in Wyvern?
- TODO: What if self-destruct will be executed?)

## SmartWeave SDK v2
Although Wyvern on Ethereum seems to be a really interesting protocol to exchange CryptoKitties it's very limited to build a highly scalable knowledge economy platform with homomorphic encryption around big data stored as permaweb (https://arwiki.wiki/#/en/the-permaweb) or IPFS (https://ipfs2arweave.com/).

It's mainly because of different requirements. We want to be able to store a variety of data with different sizes that only the owner has access to. Later we want to match clients with data processors that will process this data in a secure way. Finally, we want to build an economy around that process. This is why Arweave/SmartWeave SDK v2 (https://smartweave.docs.redstone.finance/) with permaweb concepts seems to be a good fit to cover use cases like this:
- There are clients that want to calculate taxes in Canada with a pretty easy tax system, but they don't want to reveal how much they earn. There is a digitalized accounting company that is writing its own solution to calculate tax. The processor is processing encrypted data for clients for payment.
- There is a data provider with satellite data and the provider just wants to monetize this data without revealing it publicly. There is an entity specialized in image processing of that homomorphic image encryption data that doesn't want to reveal its algorithm but it wants to just give output with a number of ships/crops/etc. Finally, there is a hedge fund that it's interested in processed data of cargo ships or crops in the current year.
- There is a medical company or patient that wants to analyze medical data(like NGS DNA sequence) without revealing it. There is a biotech startup with an algorithm to do so that wants to monetize it. On our marketplace, buyers can meet the seller.

## SmartWeave SDK v2
- TODO: Write down about proposed solution
- TODO: What about Homomorphic encryption while in proxy?

## Others
- TODO: How to simulate these custom made blockchain/exchanges/markets behaviors before launch? For example in Anylogic? Maybe Dexter (https://dexter-manual.readthedocs.io/en/latest/)?
