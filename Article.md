{% include mathjax.html %}
# Beside the point, Jane Street November 2024 puzzle.
[Back to home page](README.md)


![Besides the point](/images/november-2024.png)

Two random points, one red and one blue, are chosen uniformly and independently from the interior of a square. To ten decimal places, what is the probability that there exists a point on the side of the square closest to the blue point that is equidistant to both the blue point and the red point?

## My solution

First, I observed that it is possible to generalize the problem by splitting the square into 8 equal parts and only looking at the bottom left one, since we can always rotate or mirror the square such that a random point will always be in the bottom left part. Any point is this area is closes to the bottom side. I arbitrarily place a blue point at coordinates (x,y) in this area, and consider where the red point has to be placed for there to exist an equidistant point on the bottom side. This area is obtained by making 2 circles from the bottom-left and bottom-right corners crossing the point (x,y). The area contained in both circles does not result in there being an equidistant point. Resulting in only the yellow area yielding valid results. 

![Besides the point](/images/plaatje 1.png)

We can calculate the yellow area for the point (x,y) by calculating the area of the 2 quarter-circles, and subtracting the overlapping area twice. The radii for the left- and right circles are $$\sqrt{x^2 + y^2}$$ and $$\sqrt{(1-x)^2 + y^2}$$ respectively. The overlapping area is calculated by calculating the sector, where I use that the angle of the sector is given by $$\arctan{\frac{x}{y}$$


$$ x^2 $$
