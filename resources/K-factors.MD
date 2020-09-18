# What are NLO, NNLO K-Factors?

### What is the meaning of Leading order terms?

Consider the equation 

y = x<sup>3</sup> + 5x + 0.1

here for different values of x there be different dominating terms (high-lighted)

| x 	          | 0.001 	    | 0.1 	| 0.5 	| 2 	| 10   |
| ------------- |:-----------:| :----:| :----:|:---:| ----:|
|  x<sup>3</sup>| 0.000000001 |	0.001 |	0.125 |	`8`   |	`1000` |
| 5x 	          |    0.005 	  |`0.5`  	| `2.5` 	| `10` 	| 50   |
| 0.1 	        |    `0.1` 	    | `0.1` 	| 0.1 	|0.1 	|0.1   | 
| y 	          |0.105000001 	|0.601 	|2.725 	|18.1 |1050.1|

depending on the value of x the leading order term changes, so a leading order term is the one that dominates the result of the equations
Which is `0.1` in first column, `0.1` & `5x` in second columm....

dropping terms other than the leading order terms will result in an equation called `leading order equation`, `leading order balance` or `dominant balance`

- When `x = 0.001`, the x<sup>3</sup> and 5x terms may be regarded as negligible, and dropped, along with any values in the third decimal places onwards in the two remaining terms.
This gives the leading-order balance `y = 0.1`. Thus the leading-order behaviour of this equation at `x=0.001` is that y is constant.

- Similarly, when `x = 10`, the 5x and 0.1 terms may be regarded as negligible, and dropped, along with any values in the third significant figure onwards in the two remaining terms. 
This gives the leading-order balance y = x3. Thus the leading-order behaviour of this equation at `x=10` is that y increases cubically with x.

![](https://upload.wikimedia.org/wikipedia/commons/6/65/LeadingOrderGraph.JPG)

y = x<sup>3</sup> + 5x + 0.1. The leading order, or main, behaviour at `x = 0.001` is that y is constant, and at `x = 10` is that y increases cubically with x.
