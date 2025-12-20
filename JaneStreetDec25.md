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
Let's call this threshold 'X', our opponent uses the same logic to determine their strategy, let's call their threshold 'Y'.


