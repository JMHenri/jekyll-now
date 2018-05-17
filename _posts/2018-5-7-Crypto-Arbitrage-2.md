---
layout: post

title: Crypto-Arbitrage, Profit from inconsistency. Part 2.

excerpt: Picking a price to sell at.

published: true

---


## Hello
This is a short one. It's just here to give you a sense of how to look for a good sell-point. In fact, it's so short that after I make the next part, buying and selling through an orderbook, I'm going to merge it with this.

## What are we assuming?

1. The difference in price on exchanges, measured in percent, will follow a continuous random walk, like perlin noise, with a normal distribution centered around some constant c. c just represents the point at which two exchanges have an equal value for an asset.

2. There is always a trading fee.

## Wherein I ramble about choosing a price point to sell at.

Consider Difference (D) to be the % profit you will make during an arbitrage operation.
Volume Above D (V) is all of the volume available to you above your difference, or, the assets that you can capitalize on.
Profit over time = P/t is D*V /t... Sorta.

I'm not going to get into any of the equations that relate price, and volume, and distributions, because I'm really bad at math and you don't wanna see it. It's also far too complicated to be useful, instead I wrote some code to show you whats going on.


Here's where I put my graphic design skills to the test.

https://codepen.io/BunzoBunaldi/pen/erQBEx?editors=0011

Ok it kinda looks like an old GameMaker game.

Anyways. Look at this snippet, also click around on it. In this, you can set up a hypothetical selling point(the green line), and see how much money you'd made if that were what you actually chose. It's pretty close to what happens in real life.

*Y represents profitability, or D. positive Y values mean that an asset has a bid and an order which allow for positive arbitrage.

*X represents time.
 
*The Blue line, and height of the rectangles, represents how much money you'll make per unit traded. Blue = Height = Price Traded at

*The Red line, and width of the rectangles, represents the difference between your trading point, and a local high. Basically, this is supposed to represent how many dollars were thrown down, and that you traded on. Red = Width = Dollars traded.
 
*There's a nifty little profit function up top, the units are totally made up, but try moving the sell point around to see where you can make the most profit.


The point of this is to show you that if you trade for very very little, you will be able to trade many more dollars (wider boxes), but your profit will suffer because your profit is going to be volume * dollars traded.

To make the most money, you need to make the biggest boxes, basically. For less fancy functions, like something that went up and down over and over again, the best spot would be about halfway up.

For a normal distribution, the best spot is going to be about a third of the way up. This is just because large opportunities are less common than small opportunities.

You can test that here, you'll see that when you put the line about 1/3rd of the way to the highest peaks, you'll make the most money.

So here's how to apply this : if you look at pricing data, and are calculating and logging the potential profit you can make (be sure to use percents, e.g, .4% profit possible, rather than units) then you can get a good idea of where to trade at by going about 1/3rd of the way to your highest price points, from the point of profitability.

Example. Over the course of 3 hours, TRX arbitrage fluctuates between -1.1% profit and 1.1% profit, after fees, it transforms to -1.5 and .7% profit. A reasonable heuristic is to set your transformed trading price at 1/3 of .7% or .233%. Which is .633 without a transform.

##The End

Yes, it's the end of the article. It took me a really long time to make the javascript snippet so I'm gonna finish it later.