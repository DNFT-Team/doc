# DNFT project introduction document

## introduce

# Agreement overview

## What is dnft

DNFT is a cross chain decentralized NFT asset network based on substance, which is committed to building a new generation of NFT protocol, especially for big data and practical NFT assets; We integrate the functions of NFT asset creation, NFT decentralized transaction / auction, NFT maintenance tax, NFT recovery mechanism, and dnft governance; Dnft governance maintains dnft Dao mechanism through chain governance, Oracle, storage mechanism management, recycling mechanism management and system protocol management development.

 

Our goal is to develop a cross chain infrastructure in the field of NFT. Dnft agreement is the ninth batch of official sponsored projects of Web3.0 foundation. It has also been officially sponsored by BSC, and has won many awards in hacksong competitions. The main body of the team is from Australia. At present, it has been invested by dozens of institutions such as leader capital, candaq and shuidi capital.

 

##  The ecological architecture of DNFT

DNFT system is composed of five important parts: data storage, data tax, data exchange, Dao governance and artificial intelligence toolkit. The data is generated, stored, exchanged, shared and disseminated by the whole system. Through data ETL and data tag annotation, dnft is developing a data framework based on personal data and artificial intelligence model.

 

The algorithm stable token module of dnft protocol obtains dusd (dnft protocol ecological stable token) through Polkadot ecological token. Dusd and dnf (dnft protocol eco token) will be used as entry tokens for NFT, games and other dnft protocol eco applications.

 

The algorithm governance module of dnft protocol manages the ecological application of dnft protocol in the way of dnft Dao governance

 

DNFT encourages NFT and game projects to join the dnft protocol ecosystem. When the main network is officially launched, the NFT and game funding incentive program will go online to fund and develop NFT and game projects and enrich the dnft protocol ecosystem.

## Governance agreement of DNFT

The ultimate goal of NFT is dnft governance. First, the project team selects NFT protocol parameters and restoration governance objects, such as rate of return, payment cycle change, etc. In the future, these systems will be upgraded iteratively through community management mode. Community governance needs to be carried out through dnft Council (holding dnft agreement token DNF).

 

The DNFT Council supervises the whole process, and only the dnft Council has the right to decide on NFT agreement governance, NFT & Game Awards, ecological governance and other functions. After the initial test and implementation of tnft version test network, knft will be released online and added to Kusama ecosystem. After knft runs for a period of time, the main network of knft will be started. The formulation of dnft protocol agreement is subject to the dnft Council, and any improvement will be decided by the DNFT Council.

 

 

 

## Security

Audit and formal verification

Between January 8 and April 30, a team of six engineers reviewed and formally validated the key components of the dnft smart contract. The work includes the development of smart contract and formal verification of multi mortgage Dai.

The scope of work includes:

Formal verification of core smart contract

Code review of core smart contract

Numerical error analysis

Code review of peripheral smart contract (under development)

The report also has a "design opinion" part, and we strongly recommend that we get an in-depth technical understanding of some people's choice in DNFT.

 

Problems to be considered when building on dnft

When dnft is integrated into another chain system, special attention must be paid to avoid security vulnerabilities, operational approaches and potential financial losses.

As a preliminary note: Smart contract integration can be performed at two levels: directly using a pair of contracts, or through a router. Direct interaction provides the most flexibility, but requires the most work to get it right. Mediated interactions provide more limited capabilities, but stronger security guarantees.

There are two main categories of risk with DNFT. The first involves so-called "static" errors. These may include sending too many tokens (or requesting too few tokens) during the exchange, or allowing the transaction to stay in the memory pool long enough so that the sender’s price expectations are no longer accurate.

These errors can be resolved by fairly simple logical checks. Performing these logical checks is the main purpose of the router. People who interact directly with the pair must perform these checks themselves (in the library).

The second category is "dynamic" risk, which involves runtime pricing. Because Ethereum transactions take place in an adversarial environment, smart contracts written naively can and will be used for profit. For example, suppose a smart contract checks the ratio of assets in the DNFT pool and trades with it at runtime. Assume that the ratio represents the "fair" or "market" price of these assets. In this case, it is very susceptible to manipulation. A malicious actor can, for example, trivially insert naive transactions before and after the transaction (so-called "sandwich" attacks), resulting in smart contract transactions at a very bad price, profiting from the dealer’s fees, and then Return the contract to their original state, all at low cost. (An important caveat is that these types of attacks can be mitigated by trading in high-liquidity pools or trading in low-value.)

The best way to prevent these attacks is to introduce price predictors. Oracle is any device that returns the required information, in this case, the spot price of a pair. The best "oracle" is simply an off-chain observation of the current price by the trader, which can be passed into the transaction as a security check. This strategy is most suitable for retail trading venues, where users initiate transactions in their own name. However, in general, a credible price observation is not available (for example, in a multi-step, programmatic interaction involving DNFT). Without price oracles, these interactions would be forced to trade on DNFT at any (potentially manipulated) interest rate.

 

## Transaction

The SDK cannot perform transactions or send transactions on your behalf. Instead, it provides utility classes and functions that make it easy to calculate the data needed to safely interact with DNFT. Almost all the secure processing you need with DNFT is done by trading entities. However, you are responsible for using this data to send transactions in any context that makes sense to your application.

This guide will specifically focus on sending transactions to the currently recommended DNFT.

So, we have built a transaction entity, but how do we use it to send transactions? We still have a few things to do.

Before continuing, we should explore how ETH works in a trading environment. In fact, the SDK uses WETH because all DNFT V2 subs use WE TH under the hood. However, as an end user, it is entirely possible for you to use ETH and rely on the router to handle the conversion to WETH. So, let's use ETH.

The first step is to select the appropriate router function. The name of the router function is self-explanatory; in this example, we want to exchange execution tokens because we need to exchange a certain amount of ETH for tokens.

Sliding tolerance coding How much price fluctuations we are willing to tolerate before our transaction fails. Since Ethereum transactions are broadcast and confirmed in a confrontational environment, this tolerance is the best we can do to protect ourselves from prices The impact of volatility. We use this sliding tolerance to calculate the amount of Minum we must receive before our trade resumes, thanks to the smallest mountain. Note that this code calculates the worst case result assuming the current price, which is the middle of the route The price is fair (usually a good assumption because of arbitrage).

The path is just an ordered list of token addresses that we are trading, in our case, Weth and DAI (note that we use Weth addresses, even though we use ETH). The recipient address is the address that will receive DAI.

The deadline is a Unix timestamp, after which the transaction will fail to protect our case, our transaction takes a long time to confirm, and we wish to cancel our trade. This value must be used as msg.value in our transaction.

 

# Developer's Guide

## Interface integration

## Use API

In this guide, we will create a web interface that consumes and displays data from the DNFT submap. The goal is to provide a quick overview of the settings that you can extend to create your own user interface and analysis around DNFT data.

Many different libraries can be used to create interfaces and connections to the subgraph graph graphql endpoints, but in this guide, we will use React for the interface and Apollo Client to send queries. We will also use yarn for dependency management.

Setup and installation

We need to create a basic skeleton for the application. We will use create-react-app for this. We will also add the dependencies we need. Navigate to the root location in the command line and run:

```

```

 

In the browser, you should see the default React application running. In a text editor, open App.js in src and replace the content with this peeled template. We add as we go.

 

Graphql client

We need to set up some middleware to make requests to the DNFT subgraph and receive data. For this, we will use Apollo and create a Graphql client to handle this problem.

Add the import shown in the figure below and instantiate a new client instance. Please note how we use the link to the DNFT subgraph here.

 

We also need to add a context so that Apollo can process the request correctly. Import the appropriate provider in the index.js file and wrap the root directory like this:

 

Write query

Next we will build our query and get the data. For this example, we will get some data about the Dai token on DNFT. We will get the current price and total liquidity of all pairs. In this query, we will use the ?? generation address as the ID. We will also get the USD price of ETH to help create USD conversions for Dai data.

First we need to define the query itself. We will use gql to parse the query string into the GraphQL AST standard. Import the gql assistant into the application and use it to create queries. Add the following to your App.js file:

```

```

 

 

# Front-end integration

## SDK

The following webpage contains the technical reference SDK for DNFT. Looking for a quick start instead? You may also want to jump into a wizard, which provides a more friendly introduction to the SDK!

The SDK is written in TypeScript, has a robust test suite, performs arbitrary-precision algorithms, and supports rounding to significant digits or fixed decimal places. The main export SDK is an entity: a class that contains initialization and verification checks, necessary data fields and helper functions.

An important concept in the SDK is fractions. Because Solidity performs integer math, attention must be paid to non-EVM environments to faithfully replicate the actual calculations performed on the chain. The first issue here is to ensure the use of safe overflow integer implementations. Ideally, the SDK can use native Great Indias. However, before the support becomes more widespread, SBI objects are used, with the idea that once BigInts surges, this dependency can be compiled away. The second problem is the loss of precision, for example, chain price ratio calculations. In order to solve this problem, all mathematical operations are performed in the form of fractional operations to ensure arbitrary precision upwards. Until the value is rounded for display purposes, or truncated to fit a fixed bit width point. The SDK is applicable to all factories on the chain that have been deployed.

Code

```

```

The source code can be found on GitHub.

Dependency

The SDK declares its dependencies as peer dependencies. There are two reasons for this:

Prevent installation of unused dependencies (for example: @ethersproject/providers and @ethersproject/contracts, only for Fetcher)

Prevent repeated @ethersproject version conflict dependencies

However, this means that these dependencies must be installed in the SDK if you haven't installed them yet.

 

## Smart contract

DNFT is a binary smart contract system. The core contract provides basic security guarantees for all parties interacting with DNFT. Edge contracts interact with one or more core contracts, but they themselves are not part of the core.

Core source code

The core consists of a singleton factory and many pairs, the factory is responsible for creation and indexing. These contracts are very small, even cruel. The rationale for this is that a contract with a smaller surface area is easier to reason about, is less prone to bugs, and is more elegant in function. Perhaps the biggest benefit of this design is that many expected attributes of the system can be asserted directly in the code, leaving almost no room for error. However, one disadvantage is that the core contract is somewhat unfriendly to users. In fact, for most use cases, it is not recommended to directly interact with these contracts. Instead, peripheral contracts should be used.

Factory Reference Document

The universal bytecode held by the factory is responsible for the power supply pair. Its main job is to create one and only one smart contract for each unique token pair. It also contains the logic to open the protocol for charging.

reference document

Reference documents (20th expert review meeting)

The pair has two main purposes: as an automatic market maker and tracking pool token balances. They also publish data that can be used to construct decentralized price oracles.

Edge source code

The periphery is a set of smart contracts designed to support the interaction between a specific field and the core. Due to the unlicensed nature of DNFT, the contracts described below have no privileges and are in fact only a small part of possible peripheral contracts. However, they are useful examples of how to safely and effectively interact with DNFT.

 

Library reference files

The library provides various convenient functions for obtaining data and pricing.

 

Router Reference File

The router using this library fully supports all the basic requirements for the front-end to provide transaction and liquidity management functions. It is worth noting that it natively supports multiple pairs of transactions (such as x to y to z), treats ETH as a first-class citizen, and provides meta-transactions to eliminate liquidity.

 

Design decision

The following sections describe some notable design decisions made in DNFT. Unless you are interested in learning more about how DNFT works, or writing smart contract integration, you can skip these steps!

 

Send token

Generally, smart contracts that require tokens to perform certain functions require potential interactors to first approve the token contract and then call a function, which in turn calls the transfer From on the token contract. This is not how V2 accepts tokens. On the contrary, at the end of each interaction, at the beginning of the next interaction, the current balance is different from the stored value to determine the number of tokens sent by the current interactor. Saw the white paper to explain why this is the case, but the conclusion is that the token must be transferred to the pair before calling any method that requires a token (an exception to this rule is flash swap).

 

Weiss is different from DNFT pool, V2 pair does not directly support ETH, so ETH ERC-20 pair must be simulated together with WETH. The motivation behind this choice is to delete the ETH-specific code in the core, resulting in a more streamlined code base. However, by simply wrapping/unfolding ETH on the periphery, the end user can be completely unaware of this implementation detail.

The router fully supports interaction with any WETH pair via ETH.

 

Minimum liquidity

In order to improve the rounding error and increase the theoretical minimum quotation size provided by liquidity, the first minimum liquidity pool tokens are burned. For the vast majority of pairs, this will represent a negligible value. Combustion occurs automatically during the first liquidity supply, after which the total supply is always more boundary.

 

# our products

## Home

![img](/Users/fangzhihui/Documents/GitHub/doc/en/img/home.png)

Search query

Asset Management Center

Link wallet

Information display

Asset collection and trading

 

 

 

## Mining

![img](/Users/fangzhihui/Documents/GitHub/doc/en/img/mining.png)

Mining

Asset pledge 

## Bridge

![img](/Users/fangzhihui/Documents/GitHub/doc/en/img/bridge.png)

Cross-chain

 

 

## Market

![img](/Users/fangzhihui/Documents/GitHub/doc/en/img/market.png)

Art Management Center

Art collection

Art purchase 

## Data

![img](/Users/fangzhihui/Documents/GitHub/doc/en/img/data.jpg)Event Information Center

Smart data

 

# Resource address

## DNFT website

https://www.dnft.world

 

## white paper

 

## GitHub

https://github.com/DNFT-Team 

 

## Twitter

https://twitter.com/dnftprotocol

 

## Medium

https://medium.com/dnft-protocol

 

 

 

 

