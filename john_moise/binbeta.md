# Binomial with Beta Prior

## Motivating Example

Before we dive into the mathematics, let us consider a motivation example:

---

One day while walking downtown, you stop briefly to watch a street-performer - a
magician - who is doing various sleight-of-hand tricks, much to the amazement of
the crowd. As a part of his next trick, he shows the crowd a standard-appearing
coin with both head and tails sides. Somewhat abruptly the magician singles you
out from the crowd and offers you a deal: if he flips the coin 10 times and it
lands on heads 7 or less times, he will pay you $100, but if it lands on heads 8
or more times, you owe him $100. The question: do you take the bet?

---

Naturally this proposition sounds like a wonderful deal (with an expected value
of $89) if the coin is fair; however, how do you know whether the coin is fair?
Better yet, how would you quantify your uncertainty about the fairness of the
coin? Suppose, in this hypothetical (although unlikely in reality) that the
magician allows you to flip the coin a few times prior to taking the bet. How
could you use the data that you collect to inform your decision by updating your
uncertainty?

## Parameterizing the Situation

To begin, we model the coin as a random variable which describes the number of
successes \\(s\\) and failures \\(f\\) in \\(n\\) Bernoulli trials with some unknown probability
\\(\theta\\) of success ("heads"), and a complementary probability
\\((1-\theta)\\) of failure ("tails").

The random variable will follow the binomial distribution with a probability
mass function of \\[ p(s,f \mid \theta)={n\choose s}\theta^{s}(1-\theta)^{f}\\]

The conjugate prior of the binomial distribtion is the beta distribution :

\\[ p(\theta)=\frac{\theta^{\alpha-1}(1-\theta)^{\beta-1}}{B(\alpha,\beta)}\\]

where parameters \\((\alpha,\beta)\\) encode existing belief about the
distribution of \\(\theta\\) and \\(B(\alpha,\beta)\\) serves as a normalising
constant.

Plugging the above equations into Bayes' theorem, simplifying, and
re-normalizing,

\\[ P(\theta \mid s, f)= \frac{P(s,f \mid \theta)P(\theta)}{\int P(s,f \mid \theta)P(\theta)d\theta} = \frac{\theta^{\alpha+s-1}(1-\theta)^{\beta+f-1}}{B(\alpha+s,\beta+f)}\\]

which is a Beta distribution with parameters \\((\alpha+s,\beta+f)\\). 

## Intuition of the Beta parameters
The \\(\alpha\\) and \\(\beta\\) parameters of the Beta prior can seem somewhat
cryptic at first, until one recognizes that they represent pseudo-observations.
A Beta prior with parameters \\((1,1)\\) is uninformative as it represents a
coin where one has only witnessed a single result for both heads and tails.
Meanwhile a Beta prior of \\(85,15\\) can be understood as a coin which we have
flipped 100 times, with 85 heads and 15 tails. Thus in the process of updating
our Beta prior using Bayes' theorem, it makes sense that the resulting posterior
distribution would have parameters \\((\alpha+s,\beta+f)\\).

##Application
Revisting the motivating example, suppose you test the coin by flipping it 50
times and it lands on heads exactly 42 times and tails 8 times. Assuming an
uninformative likelihood of \\(Beta(1,1)\\), the posterior would have the form
\\(Beta(1+42,1+8)=Beta(43,9)\\).

The posterior predictive distribution is of the beta binomial family and its
probability mass function takes the form
\\[ p(s)={n \choose x}\frac{B(\alpha+s,\beta+f)}{B(\alpha,\beta)}\\]

Summing over the values that would net gain in the game,

\\[ \sum_{s=0}^{7}p(s)=0.253\\]

Meanwhile summing over the values that would net loss in the game,

\\[ \sum_{s=8}^{10}p(s)=0.747\\]

We can thus calculate our expected return in playing the game:

\\[ E($)=(0.253\cdot 100)-(0.747\cdot 100)=-$49.40\\]

That is, we would expect to lose an average of $49.40 every time we played the
game.


