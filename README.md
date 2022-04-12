# DexProtocol
This documentation is a first draft of building a decentralized exchange of assets on Arweave/SmartWeave with similar concepts as Wyvern Protocol has on Ethereum.

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

Today we can build trustless decentralized exchanges (DEX) that are fully transparent and secure.

### CEX vs DEX Problems.
TODO: Write down core problems with moving from centralized to decentralized exchanges like:
- Orderbooks vs AAM
- If we will have an order book in DEX who will pay for gas? What if small order for 1$ will trigger a waterfall of other orders for 100000$, who will pay the gas fee?
- Off-chain vs on-chain and game theory behind that in both cases in case of market makers
- How arbitration of off-chain market makers will work in a decentralized world? Will it be worth competing from a game theory perspective?
- Read more about matching algorithms on exchanges.
- How to simulate these custom made blockchain/exchanges/makers behaviors before launch? For example in Anylogic? Maybe Dexter (https://dexter-manual.readthedocs.io/en/latest/)? 
- vs decentralized like Uniswap, dYdX.
- TODO: Read more about DeFi's, DEXs and AMMs and write down core concepts.

## Solutions.
- TODO: Brief introduction of solutions for given problems.
- TODO: What is Asset/Order data structure? What contracts are needed? Or how custom-made blockchain will look like?
- TODO: Orders with a lump sum fee for all like in a centralized world (NYSE/Nasdaq/Binance) that the client needs to pay?
- TODO: Competing market makers with a lump sum or dynamic fees?

### Wyvern Protocol.
- TODO: Brief introduction of Wyvern v3 on Ethereum (https://wyvernprotocol.com/ & https://wyvernprotocol.com/docs).
- TODO: Read implementation https://github.com/wyvernprotocol/wyvern-v3/tree/master/contracts.
- TODO: Check https://exchange.projectwyvern.com/.

### Similar protocol to Wyvern.
- TODO: Read more and write down core concepts about Etherdelta (https://github.com/etherdelta/smart_contract), 0x (https://github.com/0xProject/0x-monorepo), and Dexy (https://github.com/DexyProject/protocol)

### SmartWeave SDK v2
- TODO: Read what is SmartWeave SDK v2 https://smartweave.docs.redstone.finance/.
- TODO: Check codes https://github.com/ArweaveTeam/SmartWeave.
- TODO: What is Redstone https://docs.google.com/document/d/1_5uagYbOklSEERt1ZvfVki01yC9sQt7Y5BBsisejHKM/edit.

## Others
- TODO: Check https://sui.io/#community
- TODO: Check https://celestia.org/
- TODO: Write some codes?
