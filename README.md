PRAC-1

2.1 Some Standard Sample Spaces
The prob package has some functions to get one started. For example, consider the experiment
of tossing a coin. The outcomes are H and T. We can set up the sample space quicky with the
tosscoin() function:
> tosscoin(1)
toss1
1 H
2 T
The number 1 tells tosscoin() that we only want to toss the coin once. We could toss it three
times:
> tosscoin(3)
toss1 toss2 toss3
1 H H H
2 T H H
3 H T H
4 T T H
5 H H T
6 T H T
7 H T T
8 T T T
1
As an alternative, we could consider the experiment of rolling a fair die:
> rolldie(1)
X1
1 1
2 2
3 3
4 4
5 5
6 6
The rolldie() function defaults to a 6-sided die, but we can change it with the nsides
argument. Typing rolldie(3, nsides = 4) would be for rolling a 4-sided die three times.
Perhaps we would like to draw one card from a standard set of playing cards (it is a long data
frame):
> cards()
rank suit
1 2 Club
2 3 Club
3 4 Club
4 5 Club
5 6 Club
6 7 Club
7 8 Club
8 9 Club
9 10 Club
10 J Club
11 Q Club
12 K Club
13 A Club
14 2 Diamond
15 3 Diamond
16 4 Diamond
17 5 Diamond
18 6 Diamond
19 7 Diamond
20 8 Diamond
21 9 Diamond
22 10 Diamond
23 J Diamond
24 Q Diamond
25 K Diamond
26 A Diamond
27 2 Heart
28 3 Heart
29 4 Heart
30 5 Heart
31 6 Heart
32 7 Heart
33 8 Heart
34 9 Heart
35 10 Heart
2
36 J Heart
37 Q Heart
38 K Heart
39 A Heart
40 2 Spade
41 3 Spade
42 4 Spade
43 5 Spade
44 6 Spade
45 7 Spade
46 8 Spade
47 9 Spade
48 10 Spade
49 J Spade
50 Q Spade
51 K Spade
52 A Spade

2.2 Sampling from Urns

> urnsamples(1:3, size = 2, replace = TRUE, ordered = TRUE)
X1 X2
1 1 1
2 2 1
3 3 1
4 1 2
5 2 2
6 3 2
7 1 3
8 2 3
9 3 3

Ordered, Without Replacement
Here sampling is without replacement, so we may not observe the same number twice in any row.
Order is still important, however, so we expect to see the outcomes 1, 2 and 2, 1 somewhere in
our data frame as before.
> urnsamples(1:3, size = 2, replace = FALSE, ordered = TRUE)
X1 X2
1 1 2
2 2 1
3 1 3
4 3 1
5 2 3
6 3 2

Unordered, Without Replacement
Again, we may not observe the same outcome twice, but in this case, we will only keep those
outcomes which (when jumbled) would not duplicate earlier ones.
> urnsamples(1:3, size = 2, replace = FALSE, ordered = FALSE)
X1 X2
1 1 2
2 1 3
3 2 3

Unordered, With Replacement
The last possibility is perhaps the most interesting. We replace the balls after every draw, but we
do not remember the order in which the draws come.
> urnsamples(1:3, size = 2, replace = TRUE, ordered = FALSE)
X1 X2
1 1 1
2 1 2
3 1 3
4 2 2
5 2 3
6 3 3

3 Counting Tools

Examples
We will compute the number of outcomes for each of the four urnsamples() examples that we saw
in the last section. Recall that we took a sample of size two from an urn with three distinguishable
elements.
> nsamp(n=3, k=2, replace = TRUE, ordered = TRUE)
[1] 9
5
> nsamp(n=3, k=2, replace = FALSE, ordered = TRUE)
[1] 6
> nsamp(n=3, k=2, replace = FALSE, ordered = FALSE)
[1] 3
> nsamp(n=3, k=2, replace = TRUE, ordered = FALSE)
[1] 6

3.1 The Multiplication Principle

Example
Question: There are 11 artists who each submit a portfolio containing 7 paintings for competition
in an art exhibition. Unfortunately, the gallery director only has space in the winners' section
to accomodate 12 paintings in a row equally spread over three consecutive walls. The director
decides to give the _rst, second, and third place winners each a wall to display the work of their
choice. The walls boast 31 separate lighting options apiece. How many displays are possible?
Answer: The judges will pick 3 (ranked) winners out of 11 (with rep=FALSE, ord=TRUE). Each
artist will select 4 of his/her paintings from 7 for display in a row (rep=FALSE, ord=TRUE), and
lastly, each of the 3 walls has 31 lighting possibilities (rep=TRUE, ord=TRUE). These three numbers
can be calculated quickly with
> n <- c(11, 7, 31)
> k <- c(3, 4, 3)
> r <- c(FALSE,FALSE,TRUE)
> x <- nsamp(n, k, rep = r, ord = TRUE)
[1] 990 840 29791
(Notice that ordered is always TRUE; nsamp() will recycle ordered and replace to the appro-
priate length.) By the Multiplication Principle, the number of ways to complete the experiment
is the product of the entries of x:
> prod(x)
[1] 24774195600
Compare this with the some standard ways to compute this in R:
> (11*10*9)*(7*6*5*4)*31^3
[1] 24774195600
or alternatively
> prod(9:11)*prod(4:7)*31^3
6
[1] 24774195600
or even
> prod(factorial(c(11,7))/factorial(c(8,3)))*31^3
[1] 24774195600

4 De_ning a Probability Space

4.1 Examples
The Equally Likely Model
The equally likely model asserts that every outcome of the sample space has the same probability,
thus, if a sample space has n outcomes, then probs would be a vector of length n with identical
entries 1=n. The quickest way to generate probs is with the rep() function. We will start with
the experiment of rolling a die, so that n = 6. We will construct the sample space, generate the
probs vector, and put them together with probspace().
> outcomes <- rolldie(1)
X1
1 1
2 2
3 3
4 4
5 5
6 6
> p <- rep(1/6, times = 6)
7
[1] 0.1666667 0.1666667 0.1666667 0.1666667 0.1666667 0.1666667
> probspace(outcomes, probs = p)
X1 probs
1 1 0.1666667
2 2 0.1666667
3 3 0.1666667
4 4 0.1666667
5 5 0.1666667
6 6 0.1666667
The probspace() function is designed to save us some time in many of the most common
situations. For example, due to the especial simplicity of the sample space in this case, we could
have achieved the same result with simply (note the name change for the _rst column)
> probspace(1:6, probs = p)
x probs
1 1 0.1666667
2 2 0.1666667
3 3 0.1666667
4 4 0.1666667
5 5 0.1666667
6 6 0.1666667
Further, since the equally likely model plays such a fundamental role in the study of probability,
the probspace() function will assume that the equally model is desired if no probs are speci_ed.
Thus, we get the same answer with only
> probspace(1:6)
x probs
1 1 0.1666667
2 2 0.1666667
3 3 0.1666667
4 4 0.1666667
5 5 0.1666667
6 6 0.1666667
And _nally, since rolling dice is such a common experiment in probability classes, the rolldie()
function has an additional logical argument makespace that will add a column of equally likely
probs to the generated sample space:
> rolldie(1, makespace = TRUE)
X1 probs
1 1 0.1666667
2 2 0.1666667
3 3 0.1666667
4 4 0.1666667
5 5 0.1666667
6 6 0.1666667

4.2 Independent, Repeated Experiments

Example: An unbalanced coin continued
It was easy enough to set up the probability space for one toss, however, the situation becomes
more complicated when there are multiple tosses involved. Clearly, the outcome HHH should not
have the same probability as TTT, which should again not have the same probability as HTH.
At the same time, there is symmetry in the experiment in that the coin does not remember the
face it shows from toss to toss, and it is easy enough to toss the coin in a similar way repeatedly.
The iidspace() function was designed speci_cally for this situation. It has three arguments:
x which is a vector of outcomes, ntrials which is an integer telling how many times to repeat the
experiment, and probs to specify the probabilities of the outcomes of x in a single trial. We may
represent tossing our unbalanced coin three times with the following:
> iidspace(c("H","T"), ntrials = 3, probs = c(0.7, 0.3))
X1 X2 X3 probs
1 H H H 0.343
2 T H H 0.147
3 H T H 0.147
4 T T H 0.063
5 H H T 0.147
6 T H T 0.063
7 H T T 0.063
8 T T T 0.027
As expected, the outcome HHH has the largest probability, while TTT has the smallest.
(Since the trials are independent, IP(HHH) = 0:73 and IP(TTT) = 0:33, etc.) Note that the
result of the function call is a probability space, not a sample space (which we could construct
already with the tosscoin() or urnsamples() functions). The same procedure could be used to
represent an unbalanced die, or any experiment that can be represented with a vector of possible
outcomes.
Note that iidspace() will assume x has equally likely outcomes if no probs argument is
speci_ed. Also note that the argument x is a vector, not a data frame. Trying something like
iidspace(tosscoin(1),...) will give an error.

Denoted De_ned by elements Code
Union A [ B in A or B or both union(A,B)
Intersection A \ B in both A and B intersect(A,B)
Di_erence AnB in A but not in B setdiff(A,B)
Some examples follow.
> S <- cards()
> A <- subset(S, suit == "Heart")
> B <- subset(S, rank %in% 7:9)
We can now do some set algebra:
> union(A,B)
rank suit
6 7 Club
7 8 Club
8 9 Club
19 7 Diamond
20 8 Diamond
21 9 Diamond
27 2 Heart
28 3 Heart
29 4 Heart
30 5 Heart
31 6 Heart
32 7 Heart
33 8 Heart
34 9 Heart
35 10 Heart
36 J Heart
37 Q Heart
13
38 K Heart
39 A Heart
45 7 Spade
46 8 Spade
47 9 Spade
> intersect(A,B)
rank suit
32 7 Heart
33 8 Heart
34 9 Heart
> setdiff(A,B)
rank suit
27 2 Heart
28 3 Heart
29 4 Heart
30 5 Heart
31 6 Heart
35 10 Heart
36 J Heart
37 Q Heart
38 K Heart
39 A Heart
> setdiff(B,A)
rank suit
6 7 Club
7 8 Club
8 9 Club
19 7 Diamond
20 8 Diamond
21 9 Diamond
45 7 Spade
46 8 Spade
47 9 Spade

6 Calculating Probabilities

> S <- cards(makespace = TRUE)
> A <- subset(S, suit == "Heart")
> B <- subset(S, rank %in% 7:9)
Now it is easy to calculate
> Prob(A)
[1] 0.25
Note that we can get the same answer with
> Prob(S, suit == "Heart")
[1] 0.25

6.1 Conditional Probability

The conditional probability of the subset A given the event B is de_ned to be
IP(AjB) =
IP(A \ B)
IP(B)
; if IP(B) > 0:
We already have all of the machinery needed to compute conditional probability. All that is
necessary is to use the given argument to the Prob() function, which will accept input in the
form of a data frame or a logical expression. Using the above example with S=fdraw a cardg,
A =fsuit = "Heart"g, and B=frank is 7, 8, or 9g.
> Prob(A, given = B)
[1] 0.25
15
> Prob(S, suit=="Heart", given = rank %in% 7:9)
[1] 0.25
> Prob(B, given = A)
[1] 0.2307692
Of course, we know that given the event B has occurred (a 7, 8, 9 has been drawn), the
probability that a Heart has been drawn is 1/4. Similarly, given that the suit is "Heart", there
are only three out of the 13 Hearts that are a 7, 8, or 9, so IP(BjA) = 3=13. We can compute it
by going back to the de_nition of conditional probability:
> Prob( intersect(A,B) ) / Prob(B)
[1] 0.25
> Prob( union(A,B) )
[1] 0.4230769
> Prob(A) + Prob(B) - Prob(intersect(A,B))
[1] 0.4230769
Or perhaps the Multiplication Rule:
> Prob( intersect(A,B) )
[1] 0.05769231
> Prob(A) * Prob(B, given = A)
[1] 0.05769231
We could give evidence that consecutive trials of ipping a coin are independent:
> S <- tosscoin(2, makespace = TRUE)
> Prob(S, toss2 == "H")
[1] 0.5
> Prob(S, toss2 == "H", given = toss1=="H")
[1] 0.5

