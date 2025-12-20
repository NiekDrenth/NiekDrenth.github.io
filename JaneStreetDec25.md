{% include mathjax.html %}

# Robot Javelin, Jane Street December 2025 puzzle.
[Back to home page](README.md)

## Puzzle Description
It’s coming to the end of the year, which can only mean one thing: time for this year’s Robot Javelin finals! Whoa wait, you’ve never heard of Robot Javelin? Well then! Allow me to explain the rules:

- It’s head-to-head. Each of two robots makes their first throw, whose distance is a real number drawn uniformly from [0, 1].
- Then, without knowledge of their competitor’s result, each robot decides whether to keep their current distance or erase it and go for a second throw, whose distance they must keep (it is   also drawn uniformly from [0, 1]).
- The robot with the larger final distance wins.

This year’s finals pits your robot, Java-lin, against the challenger, Spears Robot. Now, robots have been competing honorably for years and have settled into the Nash equilibrium for this game. However, you have just learned that Spears Robot has found and exploited a leak in the protocol of the game. They can receive a single bit of information telling them whether their opponent’s first throw (distance) was above or below some threshold d of their choosing before deciding whether to go for a second throw. Spears has presumably chosen d to maximize their chance of winning — no wonder they made it to the finals!

Spears Robot isn’t aware that you’ve learned this fact; they are assuming Java-lin is using the Nash equilibrium. If you were to adjust Java-lin’s strategy to maximize its odds of winning given this, what would be Java-lin’s updated probability of winning? Please give the answer in exact terms, or as a decimal rounded to 10 places.

## My Solution

From the puzzle description, we can make a clear plan on how to solve this puzzle:
1. Find the Nash equilibrium.
2. Figure out Spears Robot's strategy, how does it set d and how does that knowledge change its strategy.
3. Calculate the optimal response strategy for Java-Lin and the appropriate chance of winning

### Finding the Nash equilibrium


First, I started by trying to figure out a basic strategy. Recognizing that since we don't know anything about the throws of our opponent (before Spears Robot figured out the exploit), our decision to go for a second throw is solely based on the distance of our first throw. Furthermore, since all robots have equal skill, we cannot change our strategy depending on our opponent.
This leads to a strategy where we set a certain threshold, if our first throw is below this threshold, we go for a second throw, otherwise we keep our first throw.
Let's call this threshold $$X$$, our opponent uses the same logic to determine their strategy, let's call their threshold $$Y$$.
My first intuition for this threshold was to use 1/2, since this maximizes the expectation of the distance thrown. 
Then I wanted to calculate the optimal response to this strategy, so we set Y to 1/2 and calculate X such that it results in the highest probability of winning. 
<script type="text/x-mathjax-config">
        MathJax.Hub.Config({
         "HTML-CSS": { linebreaks: { automatic: true } },
                 SVG: { linebreaks: { automatic: true } }
        });
        </script>


The probability of winning is calculated by splitting it into 4 parts using the law of total probability:
- Both keep their first or second throw.
- We use first throw, opponent 2nd and vice versa.

This results in the following expression, where we take $$Y$$ as given:
<div style="overflow-x: auto;">
$$P(\text{x wins}|Y) = (1-X)\left((1-Y)(1-\frac{1-X}{2(1-Y)} + Y(1-\frac{1-X}{2})\right) + X\left((1-Y)(\frac{1-Y}{2}) + Y(\frac{1}{2})\right)$$
</div>
To calculate the optimal threshold $$X$$ when we know $$Y$$, we calculate the derivative with respect to $$X$$ and set it equal to 0.
This results in the following expression:  
$$X=\frac{(1-Y)^2 + 3Y}{2(Y+1)}$$  
For $$Y=0.5$$ the optimal response threshold is $$X = \frac{7}{12}$$.  
The Nash equilibrium is defined as the strategy for which no player can deviate to improve their probability of winning. 
Recognizing this, we set $$X=Y$$, resulting in the following:  
$$0=X^2 + X - 1 \rightarrow X = \frac{-1+\sqrt{5}}{2} \approx 0.618$$  
This is the only root in the interval [0,1].
We call this threshold $$\phi$$.

### Spears Robot's strategy with exploit

Spears Robot thinks we use threshold $$\phi$$, and will choose $$d=\phi$$. 
Case 1: our first throw is below $$\phi$$, they will choose to use threshold $$Y=0.5$$ since this maximizes chance to win when we throw again.
Case 2: our first throw above $$\phi$$, this is slightly more involved. Spears Robot thinks we keep our first throw, which is distributed 
uniformly over [phi,1]. We again calculate the expression for probability of winning, and derive with respect to $$Y$$:  
$$P(\text{y win}) = \frac{(1-Y)^2}{2(1-\phi)} + \frac{Y(1-\phi)}{2}$$  
Setting derivative equal to 0 yields:  
$$0=\frac{-1 + Y}{1-\phi} + \frac{1-\phi}{2} \rightarrow Y=\frac{1+\phi^2}{2} = T$$ 
We call this threshold $$T$$  

### Java-Lin's new strategy

Spears Robot doesn't know that we know of their exploit and will play thinking our threshold is $$\phi$$.
Since we don't know anything about the throws of Spears Robot, to amend our strategy we can only change our threshold $$X$$.  
Simulations in Python show that this new threshold should lower than $$\phi$$ and we will continue with this assumption.

Again we use the law of total probability to split up into multiple parts:
- first throw below $$X$$. $$Y = \frac{1}{2}$$
- first throw between $$X$$ and $$\phi$$. $$Y=\frac{1}{2}
- first throw above $$\phi$$. $$Y=T$$.

The above are split again depending on wether Spears Robot's first throw is above or below $$Y$$.  

This results in the probability of winning, for the last time we calculate $$X$$ by setting derivative with respect to $$X$$ equal to zero.
We plug the value for $$X$$ into the probability of winning.  
The probability of winning is:  
$$\frac{3}{8}X + (\phi - X)\left(\frac{3}{4}\phi + \frac{3}{4}X - \frac{1}{2}\right) + (1-\phi)\left(\frac{(1-T)^2}{2(1-\phi)} + \frac{T}{2} + \frac{\phi T}{2}\right)$$  
Derivative with respect to $$X$$:  
$$\frac{3}{8} + \frac{1}{2}-\frac{3}{4}X=0$$  
We find optimal threshold $$X=7/12$$  
This results in probability of winning of *0.4939370904*.

### Some thoughts

I found this to be an enjoyable puzzle. At first I found it difficult to intuitively understand why using a higher threshold than 0.5 could improve the chances of winning, since you lower the expected value of your distance thrown. However looking at it by considering values above 0.5 but below the new threshold phi, you can think about it this way: by keeping first throws of e.g. 0.51, you might make your expectation over 2 throws higher but when considering that you are playing an opponent with a similar strategy, the probability of winning with a distance of 0.51 is smaller than when you throw again.


