---
layout: post
title: CoinMarketCap begets inflated volumes
---

A bit of theory

A trading volume is the aggregate of trade amounts while a trade amount is a quantity (size) multiplied by price. Imagine, there is a stock exchange where there are only 2 participants A and B. When one buys 10 AAPL @ $200, the trade amount is $2,000. At the end of the trading session, each of the traders claims that this participant’s trading volume is $2000. Formally speaking, this is true. However, to avoid double counting, the exchange will report the daily trading volume of $2000 (not of $4000).

When one buys 1,000 EUR/USD @ 1.14, the trade amount is $1140, as the trade amount is calculated in the quote currency. This is the traditional approach to the calculation of volume, although one can claim that during this transaction 1000 EUR and 1140 USD have changed hands. Let us have a look at the methodology applied by prominent financial institutions operating in FX sphere. CLS International operates the world’s largest multi-currency cash settlement system, handling over 50% of global FX transaction volumes. The CLS Intraday Hourly FX spot volume database provides information on the executed trade volume that is submitted to the CLS Settlement and Aggregation services. The data is adjusted to follow the reporting conventions used by the [Bank for International Settlements](https://www.bis.org/) (BIS) and the semi-annual foreign exchange committee market reports. These surveys [only report the bought currency values, or one leg of the trade, to avoid double counting](https://www.quandl.com/databases/CLSRV/documentation) the total amount of trades.

What’s wrong with CoinMarketCap ?
=================================

In the EUR/USD example above, CoinMarketCap counts both 1000 EUR and 1140 USD when calculating overall trading volume across all markets. Let’s prove it. Unfortunately, either CoinMarketCap lacks a clear methodology supporting the numbers they show or it is really difficult to find it. Thus, one can only assume they do the following:

1.  Calculate a trading volume by currency pair (a.k.a symbol)
2.  Sum up the trading volume by currency pair to get trading volume by asset (regardless of whether an asset is a base or quote currency)
3.  Sum up the trading volume by asset to get the overall trading volume of the crypto market

Market manipulation?
====================

Let’s have a look at BZ token. There are four symbols with BZ: 3 symbols where BZ is a base currency and 1 symbol where BZ is a quote currency. Total Volume (24h) is the sum of the volume of all four symbols (see the chart below).

![]({{ site.baseurl }}/images/blog/cmc-volume-9LQ_Yff5CWdBp5zIOG7LFg.png)

Chart 1. BZ markets include SS/BZ where BZ is a quote currency

Let’s now have a look at SS token. It has 8 symbols where SS is always a base currency. The third by volume symbol is SS/BZ.

![]({{ site.baseurl }}/images/blog/cmc-volume-PZ1CJRxYf3pmibxuesJ_uw.png)

Chart 2. SS markets with SS/BZ symbol (#3)

What do these two charts show? They show that CoinMarketCap counts the volume of SS/BZ twice when calculating the overall market volume. While the volume of SS token seems to be fair (because SS across all symbols is a base currency), the volume of BZ token is inflated by 0.5% (=109k / 23.9M).

Not much? In this case, it is indeed not much. However, let’s turn to Tether.

Tether
======

CoinMarketCap website shows only top-400 by volume symbols. As of 25-Oct-2018, the total Volume (24h) was $1,830M. When we exclude all symbols where Tether is a price currency, we will get only either $54.7M or $4.6M if we follow the methodology of CoinMarketCap and exclude USDT/USD traded in Bitfinex (the alleged reason is that it is not available to the broad public).

![]({{ site.baseurl }}/images/blog/cmc-volume-4V-8J99rg-bo1-MpfBJkrQ.png)

Table 1. The symbols where Tether is a base currency

Wait a minute! Does it mean that more than at least $1.7 billion of the volume is double counted? Yes.

> At least 97% of Tether trading volume is double counted by CoinMarketCap. This is around 19% of the overall reported trading volume in cryptomarkets.

In contrast, we propose the following industry standards: Tether volume must count only symbols where USDT is a base currency.

Thank God, we have Bitcoin!
===========================

Tether is not the only case. If we scrutinize Bitcoin, Ethereum and other currencies that are both base and price currencies, we will find the same pattern. We also analyse only top-400 symbols (it seems to be enough because top BTC symbols constitute 89% of the overall volume) and focus on only BTC and ETH. All the symbols whose volume is excluded by CoinMarketCap are also excluded from our analysis.

In Bitcoin’s case, there are 277 symbols out of 400 where BTC is a quote currency and, thus, this volume is double counted. The aggregate volume of these 277 symbols is $1.4 billion, i.e., 41%.

> 41% of Bitcoin trading volume is double counted by CoinMarketCap. This is around 15% of the overall reported trading volume in cryptomarkets.

In Ethereum’s case, there are 248 symbols out of 400 where ETH is a quote currency and, thus, this volume is double counted. The aggregate volume of these 248 symbols is $332 million, i.e., 29%.

> 29% of Ethereum trading volume is double counted by CoinMarketCap. This is around 3.5% of the overall reported trading volume in cryptomarkets.

Conclusion
==========

The methodology is the central part of any endeavour related to accounting. When it is about numbers, one cannot be too scrupulous or too attentive to details. CoinMarketCap is the most popular resource in the web that publishes daily (and now monthly) trading data from cryptomarkets. It has API connections to hundreds of crypto exchanges and shows prices and volumes of 2000+ coins. Still, they do not have a clear methodology available to the public; many will agree this is the issue that CoinMarketCap must address.

Based on our research, CoinMarketCap has **double counted 37%** of the reported daily volume, as of October, 25th. It means the **actual trading volume is inflated by 1.6 times** (the table below shows the calculations; the source is [here](https://docs.google.com/spreadsheets/d/1oZh4LaVBOXD6hb2LfWJ8DuO33YzDToKa6iA8pRq72fU)).

![]({{ site.baseurl }}/images/blog/cmc-volume-NtqeEhL2p5oKMtku99BWvw.png)

Table 2. The summary of the double-counted volumes (it includes only BTC, ETH, and USDT)

_The article is written in October 2018. Its aim is bringing transparency and efficiency to cryptomarkets and cryptoeconomy._

*Disclaimer: I did the research for this article and wrote the initial draft, which was edited by a professional publisher later.*
*Enough time has now passed to republish it on my personal blog*
