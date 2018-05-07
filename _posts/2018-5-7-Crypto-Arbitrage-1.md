---
layout: post

title: Crypto-Arbitrage, Profit from inconsistency. Part 1, the Gist.

excerpt: A step-by-step cognitive guide to creating arbitrage engines.

published: true

---


During the Crypto Boom of 2018 there was a lot to be excited about if you were into bug-hunting, hacking, or just learning about new technology in general.

Everybody and their mother was trying to make an exchange, often to disastrous results. (But luckily, not THAT often).

As fun as an exchange hacking article would be, this is more about a technique that was sorely underutilized for almost all of 2017, arbitrage.

Nowadays, don't bother if you're not a professional, there are bots on everything, but arbitrage was at one point making over 30$/day for me, with a crappy architecture and one website.

tl;dr, and quick lessons learned: Don't bother with robot trading if you're not going to dedicate 40 hours a week to it.

Regardless, a ton of people are interested in this space. So lets take a programmatic and economic dive in.

## Note for prospective builders:
The architecture here is introductory. Don't follow along. Read every part AND part 4 (advanced concepts for profitability), or you will end up with a system that can't compete.

## Pre-requisites to read this:
Arbitrage ended up being a complicated beast. As such, I can do very little hand holding in this article, lest it be a 30 minute read.

This article is for people who already understand basic trading concepts such as order books, and exchanges.

This code concepts in this article are spoken for mid->senior level developers architecturally, but the core concepts should be approachable to beginners.

You'll need money to set aside to play with. At least 200$ USD for testing, at least 5,000$ USD(sorry) for long term usage.

## Introducton, and what is arbitrage?
Arbitrage is the free money of trading. When Bitcoin costs 500 dollars on exchange A, and 400 dollars on exchange B, simply buy a bitcoin on exchange B and sell on exchange A, and viola! you've evened out the market a bit, and made 100 bucks in the process.

A little deeper - Here's some log output from one of my bots :

*coin mgo ratios : {"btcBuy":1.0098327195601664,"ethBuy":0.9799523167228422} Mon May 7 2018 14:46:51 GMT-0400 (Eastern Daylight Time)*

In my system, this means, "The derivative change in USD (after conversions) for buying MGO with BTC and immediately selling with ETH is +0.9% of the current value without fees calculated in"

*Note: I no longer actively run arbitrage bots for tax reasons. My setup isn't profitable enough for the 10,000 trades to be worth the hassle.*

I've made a system that uses two base currencies, BTC and ETH. When I can buy MGO for 23 dollars using ethereum, and sell it for 24 dollars using bitcoin. That's where I take profit.

How much MGO do I buy and sell, exactly? That's covered in the Math section.
How do I do it, exactly? That's covered in the Code section.

This section is for understanding the trading system we are going to use as our use case. Then we'll break it down and make it work later.
You are free to come up with any system you like, this guide is going to be setting up a hypothetical machine between Binance and GDAX to trade Bitcoin, that's arguably the most popular arbitrage setup in the world. So if you want to make money, you'll have to use less common assets and less common markets.

Note : Expect me to reference my old system to help you through these articles, but don't expect it to be the same. Im applying new and better techniques for these articles.

##Part 1: Economic Concepts and Setbacks.

* Economic Decisions
    * How do I do it?
    * Are prices random?
    * What counts as non-random?
    * Fun facts about my failures.


#### How do I do it?

The best system is an inter-site system. This means you hold an asset (say, bitcoin), on multiple exchanges, and hook up to multiple apis. When the price differs on the two sites, you capitalize.
This system is the best for many reasons that I won't get into. My main love for it comes from the fact that it is the basic balancing force in markets.

Doing what I described in the introduction (check eth && btc price difference on the same website) is only possible if the other arbitrators (the ones who buy and sell between multiple websites) aren't doing their job, and on top of that it requires two trades, which doubles the fees.

So, in an abstract sense, inter-site trading puts you at the lowest level, there are many reasons to do it.

1. Extendability.
2. Fees.
3. Volume.

These are what you want in an arbitrage system, and you get the best bang for your buck when you do it with multiple sites on the same coin.

#### Are the prices random?

Do we consider the price of an asset random, or not?

In short : Nope.

There is really only one lesson to be learned here, I learned it the hard way. *Don't hold shitty assets or coins*.

The vast majority of companies fail. If you think it's cute to hold 100 bucks of every asset that you're intending to trade with, you will be unpleasantly surprised and probably a little confused when you are losing over a hundred dollars a day in depreciation.

An easy lesson to learn and an easy lesson to share. So other than that, yeah, they are random, or else we'd make a trading machine, not an arbitrage machine.

#### What can I hold then

It's up to you. Arbitrage in a hypervolatile asset space will only net you hyper-volatile assets. For this guide, our hypothetical robot will hold tether and bitcoin, extend it to anything you consider safe.

I recommend holding ethereum, bitcoin, and tether. If you want to actually be the person who finds an unbotted arbitrage pair, though, you'll have to hold more. (or implement complicated techniques involving many trades)

#### What is the price of an asset?

A great metric to use, especially for arbitrage, is the volume weighted average price. It's pretty straightforward from the name.
Big exchanges like Binance have more say, little exchanges like COSS have less say.

https://www.investopedia.com/terms/v/vwap.asp

It's fine to calculate it yourself, the top 3-4 exchanges take up a big majority of the market, so just use those. Along with that, there are no easy and free resources online for anything other than bitcoin.

bitcoinaverage.com is great, but a paid service. Arbitrage unfortunately requires far too many price refreshes to qualify for the free tier.

#### Sooo how is it really *really* gonna work?

Simply (on the surface)

We will hold a bitcoin and tether mix, ~1000$ of each, on two websites. Binance and GDAX.

When the difference in price reaches a certain threshhold (say .5%) we will sell exactly there, and exactly enough to put it below the threshold. The reasons for this will be explained in the math section.

Wallets do this thing where they tend to empty out. Once a wallet is emptied, it will be given a re-fill by the largest wallet, the formula for how much to give it will be in the math section.

The math section is by far the most complicated. I am not sure yet whether or not I'll include statistical analysis to find your optimal sell point.