{% include mathjax.html %}

# Beside the point, Jane Street November 2024 puzzle.
[Back to home page](README.md)



![Besides the point](/images/november-2024.png)

Two random points, one red and one blue, are chosen uniformly and independently from the interior of a square. To ten decimal places, what is the probability that there exists a point on the side of the square closest to the blue point that is equidistant to both the blue point and the red point?

## My solution

First, I observed that it is possible to generalize the problem by splitting the square into 8 equal parts and only looking at the bottom left one, since we can always rotate or mirror the square such that a random point will always be in the bottom left part. Any point is this area is closes to the bottom side. I arbitrarily place a blue point at coordinates (x,y) in this area, and consider where the red point has to be placed for there to exist an equidistant point on the bottom side. This area is obtained by making 2 circles from the bottom-left and bottom-right corners crossing the point (x,y). The area contained in both circles does not result in there being an equidistant point. Resulting in only the yellow area yielding valid results. 

{% include JS_figure.html %}


![Besides the point](/images/plaatje 1.png)

We can calculate the yellow area for the point (x,y) by calculating the area of the 2 quarter-circles, and subtracting the overlapping area twice. The radii for the left- and right circles are $$\sqrt{x^2 + y^2}$$ and $$\sqrt{(1-x)^2 + y^2}$$ respectively. The overlapping area is calculated by calculating the sector, where I use that the angle of the sector is given by $$\arctan{\frac{y}{x}}$$ and subtracting the triangle with base x and height y. Visually, I first calculate the red and green area together, and then subtract the green area to obtain half of the overlapping area. The other area is calculated by repeating the process for the right circle, where we substitute $$x$$ for $$(1-x)$$.


![Besides the point](/images/plaatje2.png)


Some algebra yields the following expression for the yellow area, where $$R_1$$ and $$R_2$$ are the squared radii $$(x^2 + y^2)$$ and $$((1-x)^2 + y^2)$$ of the left and right circle respectively:
        <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
         "HTML-CSS": { linebreaks: { automatic: true } },
                 SVG: { linebreaks: { automatic: true } }
        });
        </script>
$$\frac{1}{4} * \pi (x^2 + (1-x)^2 + 2y^2 ) - \arctan{\frac{y}{x}}R_1 - \arctan{\frac{y}{1-x}}R_2 + y $$

Now we can take the double integral over the red area from above. (calculated using python)

$$ \int_0^{\frac{1}{2}} \int_0^x \frac{1}{4} * \pi (R_1 + R_2) - \arctan{\frac{y}{x}}(R_1) - \arctan{\frac{y}{1-x}}(R_2) + y \,dy\,dx$$

Now we multiply it by 8 since we only calculated the probability for 1/8th of the square, this is approximately equal to $$\approx 0.49140757883$$ 

## Bonus solution
I wondered what would happen if we chose the side closest to either the blue or the red point. We still use the lower left triangle from the previous solution, adding that the point now has to be closer to the bottom side than the second point to any of the sides. Visually this can be obtained by drawing a square within the larger square with sides of $$1-2y$$. Now we need the part of the yellow area from the previous solution that is within this square. We calculate this red area, by taking the sector of the circle, using that the angle is given by $$ \frac{\pi}{4} - 2\arctan{\frac{y}{x}} $$  and removing the parts outside of the square by calculating the green area and the red area outside of the square combined, and then removing the green area. The green area is given by 2 triangles between the corner, the intersection of the circle and the square and the third point on the side of the triangle such that there are right angles in the triangles.

![Besides the point](/images/plaatje3.png)

We do this for the right circle aswell and add the two yellow areas together, again we take the double integral resulting in the following equation, where $$R_1$$ and $$R_2$$ are the squared radii $$(x^2 + y^2)$$ and $$((1-x)^2 + y^2)$$ of the left and right circle respectively:

$$ \int_0^{\frac{1}{2}} \int_0^x (\frac{\pi}{4} - \arctan{\frac{y}{x}})R_1 + (\frac{\pi}{4} - \arctan{\frac{y}{1-x}})R_2 - y + \frac{y^3}{x(1-x)} \,dy\,dx $$

Calculating in python and multiplying by 8 yields approximately $$\approx 0.21103527329$$. This is considerably smaller due to the yellow area also getting smaller.
