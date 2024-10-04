# Quicksort Pivots

in the lectures I only briefly mentioned strategies for determining a good pivot
for quicksort. The implementation on the slides simply picks the leftmost
element in the part of the array that we consider as a pivot. I also mentioned a
few other ways of picking a good pivot, e.g. randomly.

Median-of-three is also a good way of picking a pivot -- inspect the first,
middle, and last elements of the part of the array under consideration and
choose the median value. Using the probabilities for picking a pivot in a
particular part of the array (in the same way as we did on slide 34), argue
whether this method is more or less (or equally) likely to pick a good pivot
compared to simply choosing the first element. Assume that all permutations are
equally likely, i.e. the input array is ordered randomly.

Your answer must derive probabilities for choosing a good pivot and
quantitatively reason with them.

Add your answer to this markdown file. [This
page](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions)
might help with the notation for mathematical expressions.



# My Quicksort Pivots Analysis

If we were just to randomly pick a pivot, this implies that we know nothing about the data and we are just arbitrarily picking a pivot. Calculating the median of three values is already much better because we are actually looking at the data and influencing our decision based on what we see (evidence down below). If picking a good pivot means we should try to pick a value that would be near the middle, then any method that gets us closer to that middle value is already a step in the right direction.

The goal is to pick a pivot value that fits within the middle half of the final array (the quarter on the left is too low, and the quarter on the right is too high). By how much would we have increased the likelihood that the median is a "good pivot"? 

Assume $p_{i}$ is the probability a random subset of 3 will generate a median that is within the bounds of this middle half of the array.

$$p_{i} = \frac{Partial}{Total}$$

There's 6 total ways for the three elements to be selected, so $\binom{3}{n} = \frac{n(n-1)(n-2)}{6}$. This will be put in the denominator of the $p_{i}$ equation. If the first value we pick is derived from $(i-1)$ and the third value is $(i+1), \ldots, n$, then to pick the second value would be $(i-1)(i+1)$. This will be the numerator of the $p_{i}$ equation.

Put together we get this formula:

$$p_{i} = \frac{6(i-1)(n-i)}{n(n-1)(n-2)}$$

If we consider a good pivot to be within the bounds of $\frac{n}{4} \le i \le \frac{3n}{4}$, we can use the following integral to determine how much better the median of the three values are.

$$\sum_{i=n/4}^{3n/4} p_{i} \approx \int_{n/4}^{3n/4} \frac{6*(-x^2 + nx + x - n)}{n(n-1)(n-2)} \; dx$$

#### Pull out non-x terms and rewrite integral portion:

$$\frac{6}{n(n-1)(n-2)} \int_{n/4}^{3n/4} (-x^2 + (n + 1)x - n) \; dx$$

#### Integrate:

$$\frac{6}{n(n-1)(n-2)} \left[-\frac{x^3}{3} + \frac{(n+1)x^2}{2} - nx\right]_{n/4}^{3n/4}$$

- Upper ($x=3n/4$): $-\frac{(3n/4)^3}{3} + \frac{(n+1)(3n/4)^2}{2} - n\left(\frac{3n}{4}\right)$
- Lower ($x=n/4$): $-\frac{(n/4)^3}{3} + \frac{(n+1)(n/4)^2}{2} - n\left(\frac{n}{4}\right)$

Simplify Upper & Lower:

- Upper (simplified): $-\frac{9n^3}{64} + \frac{9(n+1)n^2}{32} - \frac{3n^2}{4}$
- Lower (simplified): $-\frac{(n/4)^3}{3} + \frac{(n+1)(n/4)^2}{2} - \frac{n^2}{4}$

#### Plug in Upper and Lower:

$$\frac{6}{n(n-1)(n-2)} \left(Upper - Lower\right)$$

$$\frac{6}{n(n-1)(n-2)} \left(-\frac{9n^3}{64} + \frac{9(n+1)n^2}{32} - \frac{3n^2}{4} +\frac{(n/4)^3}{3} - \frac{(n+1)(n/4)^2}{2} + \frac{n^2}{4}\right)$$

#### Combine Like-Terms & Simplify:

$$\frac{6}{n(n-1)(n-2)} \left(-\frac{13n^3}{96} + \frac{(n+1)n^2}{4} + \frac{n^2}{2}\right)$$

$$\frac{6\left(-\frac{13n^3}{96} + \frac{(n+1)n^2}{4} + \frac{n^2}{2}\right)}{n(n-1)(n-2)}$$

### Analyzing what happens as $n$ approaches infinity:

$$\lim_{n \to \infty} \frac{6\left(-\frac{13n^3}{96} + \frac{(n+1)n^2}{4} + \frac{n^2}{2}\right)}{n(n-1)(n-2)} = \frac{11}{16}$$

As $n$ approaches infinity, we can see that it approaches a constant factor of $\frac{11}{16}$ which is greater than $\frac{1}{2}$. The reason why it is important that this constant is greater than $\frac{1}{2}$ is because it shows that the median-of-3 truly does have higher odds of being within a "good pivot" range (being within the $\frac{n}{4} \le i \le \frac{3n}{4}$ range of the array) than any randomly chosen element.


# Sources

- https://walkccc.me/CLRS/Chap07/Problems/7-5/: Helped in constructing   
                                the initial equations, finding what to look for, 
                                and the general direction in which to get me 
                                started.
- ChatGPT: For a refresher on how to math with limits and assistance in solving  
                my limit equation.

# Plagiarism Acknowledgement

I certify that I have listed all sources used to complete this exercise, including 
the use of any Large Language Models. All of the work is my own, except where 
stated otherwise. I am aware that plagiarism carries severe penalties and that if 
plagiarism is suspected, charges may be filed against me without prior notice.