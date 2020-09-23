# binance-code-challenge

A code challenge about integrating an API in a concurrent way.

## Introduction

Imagine you work for a wealth manager. The customers trust you with investing their money to yield profits consistently over a long period of time. Of course, you invest it in crytpo currencies.

Your role is to create a system which receives buy and sell signals for the different crypto currencies ("coins") that are generated by strategies developed by the asset management department of the company. All the coins are deposited in the company's portfolio on Binance, [the largest](https://www.coingecko.com/en/exchanges) exchange for crypto currencies.

Right now, the company is just ramping up. You start by implementing the API integration to execute the buy and sell signals received. Luckily, Binance has a testnet for each of its variants (spot trading and futures trading) with a completely virtual portfolio of crypto currencies, so you don't put your customers' assets at risk just yet.

An intern already started implementing a few pieces, but she left and you can change the entire project as you think makes most sense.

Here we go.

## Challenge

Please separate the implementation of the individual parts by commit and/or tag.

### Part 1: Execute order for each signal

Please start by wiring the existing components together. A [market order](https://www.investopedia.com/terms/m/marketorder.asp) should be executed for each signal.

- The API endpoint for placing an order is already implemented: `CoinFlipper.Exchanges.BinanceFutures.create_market_order/2`
- Please leave the code for generating signals as is. Only rewrite the function to execute them: `CoinFlipper.Strategy.Random.execute_signal/2`

### Part 2: Batching orders

Please implement a mechanism that will execute orders in batches on the exchange.

- Each order should be executed after max. 10 seconds.
- Each batch should have as many orders as possible.

### Bonus: Calculate realized profits

It's always nice to know how profitable a strategy is. If you have some time left, please calculate and output the [realized profit or loss](https://www.thebalance.com/realized-profit-1031349) in USDT (the [stablecoin](https://blog.bybit.com/insights/focus/what-is-usdt-everything-you-need-to-know/) we're trading against) after each order. If you don't have time left for it, it's completely fine.

## Prerequisites

### Fork the repo

Fork [the repo](https://github.com/arnodirlam/binance-code-challenge), clone it and fire up your favorite dev environment.

### Get API Key

- Head over to the [Binance Futures Testnet](https://testnet.binancefuture.com) and "Register Now".
- Once signed in, a new tab "API Key" appears in the lower part of the page.
- Store the **API Key** and **Secret Key** displayed there in a new file `.env` in the project root:
  ```
  BINANCE_API_KEY=...
  BINANCE_API_SECRET=...
  ```

## API endpoints

### Execute order

Endpoint to execute an order on the exchange.

Docs: [New Order (TRADE)](https://binance-docs.github.io/apidocs/testnet/en/#new-order-trade)

This endpoint is already implemented in `CoinFlipper.Exchanges.BinanceFutures.create_market_order/2`.

We use only market orders, as they are executed fastest.
Also, we specify to wait for the order to be executed (instead of just acknowledged by the exchange) to get the result synchronously.

### Execute orders in batch

Endpoint to execute a batch of orders on the exchange.

Docs: [Place Multiple Orders (TRADE)](https://binance-docs.github.io/apidocs/testnet/en/#place-multiple-orders-trade)

## Closing open positions

_A word of caution:_ Once you start sending actual orders to the exchange, please make sure you close them manually after you're done.
To do this, go to the [Binance Futures Testnet](https://testnet.binancefuture.com), and under **Positions**, click the button **Market** next to the open position.
This will close the position with a market order.

If you ever get the error `User in liquidation mode now.`, it's too late and you need to re-create the account.

(We're using the [Futures](https://www.investopedia.com/terms/f/futurescontract.asp) variant of Binance, because its testnet is the only one that provides a web UI.)
