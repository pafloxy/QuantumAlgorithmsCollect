
# Deutsch-Josza but Realistic
**Assumption :** Deutsch-Josza algorithm 

[Series index](README.md) 

---

Suppose you receive a black-box Boolean function

$$f:\{0,1\}^n\to\{0,1\}.$$

There are $2^n$ possible inputs. You do not want the entire truth table. You want one global statistic:

> What fraction of the input space maps to 1?

For $n=4$, imagine that nine inputs return `0` and seven return `1`. Can a small coherent experiment produce a measured bit whose bias reflects $7/16$?

That is the problem.

## From a category to a quantity

Deutsch–Jozsa is usually presented with an extreme promise:

```text
                     THE PROMISED WORLDS

constant zero        [0000000000000000]    p = 0
perfectly balanced   [0000000011111111]    p = 1/2
constant one         [1111111111111111]    p = 1

                     AND THE WORLD WE NOW ALLOW

neither              [0000000001111111]    p = 7/16

Each position represents one input, not one experimental shot.
```

The task is to decide which world you are in. Most Boolean functions live in neither world.

So remove the promise and change the output. Define

$$n_1=\left|\{x:f(x)=1\}\right|, \qquad n_0=2^n-n_1.$$

The target quantity is

$$p=\frac{n_1}{2^n}.$$

Can $p$ appear directly as a measurement probability of one small register?

## A sixteen-input example

Take a four-bit function. Suppose

$$n_1=7, \qquad n_0=9.$$

```text
              ONE POSSIBLE SIXTEEN-INPUT FUNCTION

                x                f(x)
            ---------       ---------------
            0000-0011       [0] [0] [0] [0]
            0100-0111       [0] [0] [0] [0]
            1000-1011       [0] [1] [1] [1]
            1100-1111       [1] [1] [1] [1]

                       nine 0s, seven 1s
```

*This is an illustrative arrangement consistent with the stated counts; the truth table is not handed to the algorithm.*

The desired behaviour is

```text
                         PROBE TO DESIGN

                   equally weighted inputs
                         0000 ... 1111
                               |
                               v
                      +-----------------+
                      |   f-oracle      |
                      | + experiment ?  |
                      +--------+--------+
                               |
                               v
                         one output bit
                           /       \
                      b = 0         b = 1
                       9/16          7/16

                    One bit. Not a truth table.
```

One run does not reveal the number seven. It gives one Bernoulli sample. Repeated runs let you estimate the bias.

That distinction matters.

## The challenge

You are given coherent oracle access to a Boolean function

$$f:\{0,1\}^n\to\{0,1\}.$$

Design a small quantum experiment whose output bit $b$ satisfies

$$\Pr[b=1]=\frac{n_1}{2^n}, \qquad \Pr[b=0]=\frac{n_0}{2^n}.$$

You are not promised that $f$ is constant or balanced. A phase-oracle model is available:

$$\ket{x}\longmapsto(-1)^{f(x)}\ket{x}.$$

You may use a uniform superposition over the inputs and a small control register. What is the simplest coherent construction, and what information does one run provide compared with many runs?


## Wait — can this be done classically?

Yes.

Pick a uniformly random input $x$, evaluate $f(x)$, and repeat. The sample mean estimates the same fraction.

```text
                    THE CLASSICAL BASELINE

       random x ----> evaluate f(x) ----> record one bit
                                               |
                                               v
                                    repeat and average

                  Same target bias. No speedup claimed.
```

This page does not claim that producing the bias above establishes a quantum speedup. The design question is narrower:

> Can a global property of a coherently queried black box be reorganized so that one small register carries the statistic we care about?

That pattern is useful even when its first form is not complexity-theoretically superior.

## Why the usual story is too rigid

If Deutsch–Jozsa is remembered only as a fixed circuit, its more reusable structure is easy to miss.

The oracle stores $f(x)$ in a phase, while a computational-basis measurement reveals populations. The problem is therefore to design a route

```mermaid
flowchart LR
    P["Phase information"] --> I["Interference<br/>construction to design"]
    I --> B["Output-bit probabilities"]
    B --> M["Measure one bit"]
```

without reading the full truth table.

## State construction and estimation are separate

A LeetCode-style signature for the first half of the problem would be:

```python
def probe(f_oracle, n):
    """
    Return one bit b such that

        Pr[b == 1] = (# of x with f(x)=1) / 2**n
    """
```

Then comes a second question: how many calls to `probe` estimate $p$ within additive error $\varepsilon$?

Separating these tasks keeps a coherent encoding from being confused with a statistical estimate.

*Suppose the desired probe has been built. Here is a made-up sample history—not simulation output:*

```text
ONE RUN                         SIXTEEN FRESH RUNS

   [1]                     1 0 0 1 1 0 0 0 1 0 1 0 0 0 1 0
    |                                      |
    v                                      v
 one sample                           six observed 1s
 not the count                             |
                                           v
                                  sample mean = 6/16

                      target bias = 7/16
                      this sample estimate = 6/16

                  The sample mean need not equal the true bias.
```

## Constraints

Assume:

- $f$ is a Boolean function on $n$ bits;
- coherent phase-oracle access is available;
- a uniform superposition over all inputs can be prepared;
- a small control register is allowed;
- classical information is obtained by measuring at the end of a run;
- no constant-versus-balanced promise is given.

## Why this problem is worth thinking about

The general move is to replace

```text
           +-------------------------------+
           |   WHICH CATEGORY IS IT IN?    |
           |     constant / balanced       |
           +-------------------------------+
```

with

```text
           +-------------------------------+
           |   WHICH STATISTIC DO I NEED?  |
           |       fraction of ones        |
           +-------------------------------+
```

Algorithms, statistics, databases, and machine learning often do not need a complete object. They need one aggregate property. The art is to choose a representation that makes that property readable.

## Things to think about

1. What state over the domain makes every input contribute symmetrically?
2. How can phase information become a measurable bit value?
3. What does one measurement tell you, and what requires repeated sampling?
4. How does the estimator compare with ordinary random sampling?

> **Claim boundary**
>
> The source notebook gives a coherent encoding of the counts into measurement probabilities. This does not make one sample reveal the exact Hamming weight, and it does not by itself beat classical sampling. A complexity advantage would require a separate analysis against classical baselines and stronger quantum estimation methods.

## Your move

A Boolean function may hide an exponential-size truth table, while the answer you want is only one number.

Can you make that number leak out through one bit?

```text
               THE TWO BOXES STILL TO UNDERSTAND

       +-------------------+       +-------------------+
       | coherent encoding | ----> | statistical       |
       |                   |       | estimation        |
       | correct bit bias  |       | enough samples    |
       +-------------------+       +-------------------+

                  construction != estimation
```
