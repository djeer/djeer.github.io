---
layout: post
title: All Quiet on the Binance Front
---

This analysis continues the series of studies on the trading anomalies in cryptocurrency exchanges. We have decided to focus on the exchange that appeared only in 2017 but since then has been the leader — Binance.

TL;DR
=====

Although there are some examples of suspicious behaviour in Binance, we have not found enough evidence of systematic wash trading. The following makes one concerned:

a) In some symbols (EOS/BTC, BCH/BTC ADA/USDT, BNB/USDT, XLM/BTC) trades occur every 125, 200, 400 and 1000 ms. Is it trading bots making regular trades?  
b) STEEM/BTC and VIA/BTC have the bimodal distribution of trade amounts. Is it trading bots placing small orders to minimize market risk?  
c) VIA/BTC and ETH/BTC show uncorrelated delays between the trades and trading volumes. Is it bots making regular self-trades in addition to organic flow?  
d) The ratio of sell-to-buy trades fluctuates around 50% in ADA/USDT, BTC/USDT, ETH/USDT. Is it bots that bear no directional market risk and balance sell and buy trades?

The detailed research below will help you decide yourself.

Analysis
========

_Disclaimer: It’s not a comprehensive analysis across all exchanges and all available assets. It’s rather more like the hands-on analysis that could be reproduced on any PC with the help of Jupiter notebooks and some data analysis skills. We plan this article will be followed by further research papers._

As many newbies to the crypto community quite often rely on the most popular volume-based ranking, Binance, which is Top-1 exchange according to CoinMarketCap, is the popular choice among retail investors. Gigantic daily volumes amid bearish market trend place Binanance under scrutiny. Luckily, Binance allows downloading historical trades with millisecond timestamps via API, which makes our investigation easier. These two factors have made our mind when we contemplated our next research after finalizing [Is this real? Fake Volumes on Hitbtc and Binance](https://medium.com/crypto-integrity/is-this-real-fake-volumes-on-hitbtc-and-binance-c984279acb9f).

We divide our research into 5 parts. Each of the parts focuses on different aspects of the trade distribution:

1.  The delay between the consecutive trades
2.  The delay between trades vs number of trades
3.  Most popular values of delays between trades
4.  Average daily statistics
5.  Trade direction dominance

**1\. Delays in ms between the consecutive trades**
---------------------------------------------------

Delays in ms between the consecutive trades (selected symbols; first 10 ms are excluded; one bar is 1 ms; data from 25-Mar-2018 till 23-Jul-2018) show some anomalies around 125, 200, 400 and 1000 ms. May it be the case that either human beings or trading bots used to make trades (it’s not about placing an order, which a bot may do with a regular interval!) so punctually and orderly? We doubt so.

![]({{ site.baseurl }}/images/blog/binance-Gnuwe_UzW65_RRu2.png)

ADA/USDT on Binance

1.1 Here one can see an unexplainable high number of trades that are made every ~125 ms, 400ms, 1000 ms (i.e. 1 sec).

![]({{ site.baseurl }}/images/blog/binance-QEffAKUE0IYNISDU.png)

BNB/USDT on Binance

1.2 Here one can see an unexplainable high number of trades that are made every ~125 ms, 200ms, less distinctive 400 ms, and highly distinctive 1000 ms (i.e. 1 sec).

![]({{ site.baseurl }}/images/blog/binance-gqeutoYS5B1UP-2K.png)

XLM/BTC on Binance

1.3 Here one can see an unexplainable high number of trades that are made every 200ms, less distinctive 400 ms, and still distinctive 1000 ms (i.e. 1 sec).

**2\. Delays in ms vs number of trades**
----------------------------------------

Here we have drawn some charts showing delays in ms versus the number of trades by log10 volume (selected symbols; first 10 ms are excluded; one bar is 10 ms; data from 1-Mar-2018 till 9-Jun-2018). The charts show one more dimension — the size of trade amount, i.e. how many ETH a user bought or sold against BTC.

2.1 This symbol seems to be organic.

![]({{ site.baseurl }}/images/blog/binance-mAweVPLsjtzPMMTm.png)

ETH/BTC on Binance

2.2 This currency pair would also seem organic if there were no distinctive groups of trades made every second.

![]({{ site.baseurl }}/images/blog/binance-uylUcHiNj3NKmKdb.png)

EOS/BTC on Binance

![]({{ site.baseurl }}/images/blog/binance-KAuO1HNPiqt4eDd8.png)

EOS/BTC on Binance

2.2 This currency pair does not seem organic for two reasons: (1) there are two distinct groups of trades made every 400 and 1000 ms; (2) the dominance of trade sizes, which can be explained neither by min lot size, nor by min lot step, nor by favourite sizes of retail investors (such as 1, 0.5, 0.1, 0.01 etc.)

![]({{ site.baseurl }}/images/blog/binance-iOue44qFI75t69xU.png)

BCH/BTC on Binance

![]({{ site.baseurl }}/images/blog/binance-h3uAC2YI3Td0jKhi.png)

BCH/BTC on Binance

2.3 STEEM/BTC & VIA/BTC — These two symbols have the similar profile as if someone made a lot of small trades (as the bimodal distribution of trade amounts shows) quite often to produce the appearance of liquidity (with the delay of less than 100–150 ms).

![]({{ site.baseurl }}/images/blog/binance-FAbvcCo448SCFcqE.png)

STEEM/BTC on Binance

![]({{ site.baseurl }}/images/blog/binance-mJJ3JPVmela7Y202.png)

VIA/BTC on Binance

2.4 ENG/BTC symbol has quite an unusual pattern of delays between the trades: someone makes trades of random volume every 10–20 ms — these trades make up the majority of overall volumes.

![]({{ site.baseurl }}/images/blog/binance-k-t-EmDF5UUlpEhx.png)

ENG/BTC on Binance

The further research of ENG/BTC shows that these trades are made smoothly round the clock (see the chart below: _Y-axis_ is time during 24 hours, _X-axis_ is the delay between the trades). One of the possible explanations is the trading bots involved in wash-trade.

![]({{ site.baseurl }}/images/blog/binance-JIgyaoqcoaVPMtIX.png)

ENG/BTC on Binance

**3\. A share of delays between trades**
----------------------------------------

The table below summarizes the shares of most popular 20 delays between consecutive trades (in ms; data from 29-May-2018 till 21-Jul-2018). It might look suspicious that almost 2% BTC/USDT trades occur every 17, another 1.5% — every 18, another 1.5% — every 19 milliseconds. However, we believe there is no direct evidence of the wash trading in the data.

![](https://miro.medium.com/v2/resize:fit:1304/1*9IhOOAorS3-pcsAyx09kKg.png)

A share of delays between trades

**4\. An average daily delay between trades**
---------------------------------------------

The following figures show the average daily delay between consecutive trades (in ms; the period is indicated in charts) vs daily volume. Theoretically, there must be no patterns or regularities that cannot be explained by fundamental factors (such as news).

4.1 ETC/BTC: the flow seems to be organic.

![]({{ site.baseurl }}/images/blog/binance-Zahvb_2Vr_V25y6y.png)

ETC/BTC on Binance

4.2 VIA/BTC: the permanent delays in April-May do not seem so organic.

![]({{ site.baseurl }}/images/blog/binance-RwKJv6DY_Rmi0ekE.png)

VIA/BTC on Binance

4.3 ETH/BTC: one can see three periods — the delays were more or less constant around 300 ms, then around 250 ms, then around 400 ms.

![]({{ site.baseurl }}/images/blog/binance-r-ovxZZ36ueCGLCr.png)

ETH/BTC on Binance

5\. Trade direction dominance
-----------------------------

The hypothesis is that equal share of both buy and sell trades (i.e., trades caused by a hitting buy order or a sell order respectively) indicates the activity of wash trading bots, which enter into a transaction without economical reason. In contrast, market making bots usually place resting two-side (i.e., buy and sell) limit orders while the hitting retail flow is usually random but trendy.

Although there are some examples of suspicious behaviour in Binance, the hypothesis of wash trading evidenced by trade direction dominance does not seem to be regular and sustainable.

5.1 BNB/USD: the flow seems to be organic.

![]({{ site.baseurl }}/images/blog/binance-hX5ZWs0jJO5PICMJ.png)

![]({{ site.baseurl }}/images/blog/binance-yVC5uMzJeZ6sUqVN.png)

5.2 ADA/USDT: the buy or sell dominance is not obvious and sticks to 50%.

![]({{ site.baseurl }}/images/blog/binance-TQsuqFJQd78lMSO9.png)

5.3 BTC/USDT: the same pattern.

![]({{ site.baseurl }}/images/blog/binance-m5pdcTBGL40pCmTp.png)

5.4 ETH/USDT: the same pattern.

![]({{ site.baseurl }}/images/blog/binance-923woGeKRkXXuJDr.png)

Conclusion
==========

Binance has been the leading exchange for quite a while. Ranked by the 30-day adjusted traded volume (as per CoinMarketCap data), Binance has left OKex, Huobi as well as Bitfinex far behind. Are these volumes the result of the organic trading flow? Does the exchange really have an enormous number of clients who continue actively trading cryptocurrencies on a daily basis? Our research has shown that there are some anomalies in trading statistics that may cause concerns.

1\. Namely, we have detected a recurring pattern in delays between the consecutive trades — in some symbols (EOS/BTC, BCH/BTC ADA/USDT, BNB/USDT, XLM/BTC, to mention a few) trades are likely to occur every 125, 200, 400 and 1000 ms. Although trading bots are likely to place or replace orders at a given interval, trades are unlikely to happen with such regularity.

2\. The analysis of delays between consecutive trades shows there are certain popular values. Although there might be some technical explanation why the matching engine of Binance is likely to match trades every X milliseconds, one can suspect inflated volumes when trading bot trade with each other with the single purpose of increasing reported trading volume.

The two symbols — STEEM/BTC and VIA/BTC — have drawn our attention because of a similar profile. It appears that someone makes a lot of small trades (as the bimodal distribution of trade amounts shows). It may be explained as wash trading when bots are trading small lots in order to minimize the risk of their order being matched against non-related participants.

3\. The analysis of top-20 delay values between consecutive trades shows that around 5% of BTC/USDT trades occur every 17, 18 and 19 milliseconds, which is weird. However, we cannot claim there is direct evidence of the wash trading in the data.

4\. Theoretically, there must be no patterns or regularities in average daily delay between trades, which are not correlated to the volumes. This means lower volumes are likely to implicate fewer trades and, thus, longer time between the consequent trades. The symbol VIA/BTC had almost the same daily volume in April-June, 2018, while the delays were uncorrelated. It may be explained by bots making random trading with the daily volume target. The symbol ETH/BTC had three different levels of delays during the observed period from February till June. The delays were not correlated to the volumes. It may be explained by bots making self-trades at regular intervals in addition to organic flow.

5\. When the share of volumes generated by bots, which are involved in self-trading, high, the ratio of sell-to-buy trades fluctuates around 50%. The wash trading bots usually bear no directional market risk and close the position as soon as possible. Although there are some examples of suspicious behaviour in Binance (e.g., ADA/USDT, BTC/USDT, ETH/USDT), we do not find enough evidence of wash trading evidenced by trade direction dominance.

_The article was written in October 2018. Its aim is bringing transparency and efficiency to cryptomarkets and cryptoeconomy._

*Disclaimer: I did the research for this article and wrote the initial draft, which was edited by a professional publisher later.*
*Enough time has now passed to republish it on my personal blog*
