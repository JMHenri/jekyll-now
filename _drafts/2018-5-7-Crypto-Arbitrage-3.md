---
layout: post

title: Crypto-Arbitrage: Profit from inconsistency. Part 3, the Code.

excerpt: A step-by-step hackdown into the most lucrative and least secured sites we've ever seen.

published: false

---
During the Crypto Boom of 2018 there was a lot to be excited about if you were into bug-hunting, hacking, or just learning about new technology in general.

Everybody and their mother was trying to make an exchange, often to disastrous results. (But luckily, not THAT often).

As fun as an exchange hacking article would be, this is more about a technique that was sorely underutilized for almost all of 2017, arbitrage.

Nowadays, don't bother if you're not a professional, there are bots on everything, but arbitrage was at one point making over 30$/day for me, with a crappy architecture and one website!

tl;dr, and quick lessons learned: Don't bother with robot trading if you're not going to dedicate 40 hours a week to it.

Regardless, a ton of people are interested in this space. So lets take a programmatic and economic dive in.

## Pre-requisites to read this:
Arbitrage ended up being a complicated beast. As such, I can do very little hand holding in this article, lest it be a 30 minute read.

This article is for people who already understand basic trading concepts such as order books, and exchanges.

This code concepts in this article are spoken for mid->senior level developers architecturally, but the core concepts should be approachable to beginners.


## What is arbitrage
Arbitrage is the free money of trading. When Bitcoin costs 500 dollars on exchange A, and 400 dollars on exchange B, simply buy a bitcoin on exchange B and sell on exchange A.

"simply"

And viola! you've evened out the market a bit, and made 100 bucks in the process.

##Economic creatures lurking in the shadows

* Economic Decisions
    * Random Market or not?
    * What counts as non-random?
    * Fun facts about my failures.
* Mathy Decisions
    * I "see" an arbitrage opportunity. How do I determine how to take advantage of it quantitatively?
    * When do I start the process?
    * When do I stop the process?
* Programming Decisions
    * Does it really use THAT much processing power?
    * Does it really need to be THAT fast?
    * Any recommendations?
    