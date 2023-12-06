When a random experiment is performed, we are not interested in all of the details. Only in one certain numerical quantity, for example we may be interested in the probability that the sum of two thrown dice will be seven, but not interested if it was a (1,6) or (6,1) combination. The numerical value we are interested in keeping track of in random experiments(basically each thrown dice in our example) is a **Random Variable**.

This is how we show this the probablity of a random variable where X is the sum of two thrown dice:

*P{X = 7} = {(1,6),(2,5),(3,4),(4,3),(5,2),(6,1)} = 6/36 = 1/6*

Random variables can either take a set of discrete variables which makes them **discrete random variables** or a continuous values which makes them **continuous random variables**.

## The Distribution Function

*The commulative distribution function* is defined for any real number x as:

*F(x) = P{X <= x}*

We can show different probabilities of random variables in distribution function form, for example:

*P{a < x <= b} = P{x <= b} - P{x <= a} = F(b) - F(a)*

* The sum of distribution functions of all valid random variables either discrete or continuous equals 1.

## Probability Functions

The **probability mass function** for a **discrete** random variable is defined by:

p(a) = P{X = a}

The sum of probability mass functions of all possible values of the random variable is 1.

**The commulative distribution function F(a) can be expressed as the sum of p(x) for all x <= a.**

## Probability Density Function

Assuming X is a continuous random variable, **The probability density function f(x)** which has the property that:

*P{X of a set } = the integral over that set of f(x)*

There's no need to mention that the probability of P over a set of all possible real numbers of x equals 1.

* The cummulative distribution function of a x in a certain range is the integral of the probability density function over that range.

*F(a) = P{X <= a} = integral from infinity to a of f(x)*










