<!--
Original publication metadata (retained from the MDX source):
---
title: "Can You Check a Palindrome When Equality Is Not Free?"
description: "A familiar palindrome problem, except the symbols can be different without being perfectly distinguishable. What survives when equality stops being a free primitive?"
date-display: "18th July 2021"
slug: "palindrome-without-free-equality"
order: 1
difficulty: "Medium"
status: "Challenge"
changedAssumption: "Equality is no longer free"
source: "https://github.com/pafloxy/QuantumAlgorithmsCollect/blob/main/Algorithms/quantum-palindrome-check.ipynb"
draft: false
---
-->

# Can You Check a Palindrome When Equality Is Not Free?


[Series index](README.md) 

*The familiar palindrome problem, except that strings are now quantum states, and equality checking isn't free.*




You probably know how to check a palindrome.

```text
                    TWO POINTERS, WALKING INWARD

position       0     1     2     3     4     5     6
symbol         R     A     C     E     C     A     R
               |     |     |           |     |     |
               |     |     +-----=-----+     |     |
               |     +-----------=-----------+     |
               +-----------------=-----------------+

                       every mirrored pair agrees
```

Two pointers walk inward. If every mirrored pair agrees, return `true`.

The algorithm is so simple that it barely feels like an algorithm. But it is quietly using a powerful primitive:

```text
                         THE FREE PRIMITIVE

                 a ----.   +----------------+
                       +-->|  equal(a, b)   |---> True / False
                 b ----'   +----------------+

                           small line of code;
                           large assumption
```

Now let's remove that primitive. What if the symbols themselves cannot always be perfectly distinguished? 

That turns the five-line programming exercise back into an algorithm-design problem.

## The line of code we usually ignore

For an ordinary string

```text
                      MOVE THE POINTERS INWARD

position           0     1     2     3     4
symbol             1     3     2     3     1
                   |     |           |     |
                   |     +-----=-----+     |
                   +-----------=-----------+

                   
```

we test `1 == 1`, then `3 == 3`, and we are done. More generally, for a sequence of length $N$, the palindrome condition is

$$
s_i=s_{N-1-i}
$$

for every mirrored position $i$.

Classically, this is uninteresting because exact comparison is built into the data model. So let us change the data model, and make it quantum :)

## An alphabet whose letters can overlap

Suppose a letter is a quantum state. Our alphabet might look schematically like this 

$$
\mathcal A=\left\{\ket{\phi_1},\ket{\phi_2},\ket{\phi_3},\ldots\right\}.
$$

where each $\ket{\phi_i}$ is an unknown quantum state. 

The main distinction with a ordinary alphabet story is as follows: two different letters do not have to be perfectly distinguishable from one another, which means you who is classical will not be able to tell one character different from another, and that is not my fault --- this comes from the fundamentatl properties of quantum states. 

Well actually, you can tell them differently under certain conditions, which is when the set of states making the alphabet make an **orthogonal-basis**, i.e 
$$ \braket{\phi_i | \phi_j} = \delta_{ij} $$
but that is not the case here/


The useful intuition is simpler:

```text
ORDINARY ALPHABET                  QUANTUM ALPHABET

    [ A ]    [ B ]                    [|phi_1>]    [|phi_3>]
       different                         different
           |                                 |
           v                                 v
   perfectly tellable apart          may be hard to distinguish

                                     equal(a, b) = ???
```

The operation “is this symbol exactly equal to that symbol?” is no longer free. If you ask a physicst, say my colleagues, they might refer you to heavy words like **Quantum State Tomography** or **Quantum State Certification**, and apparently determining if two quantum states are actually equal has so far required more than 1000 research paper, and yet noone has managed to solve it for all cases :) 

But don't worry, I got you covered ! The global question still makes sense.

## A four-symbol example

Consider

$$
\ket{\phi_2}\;\ket{\phi_1}\;\ket{\phi_3}\;\ket{\phi_2}.
$$

The outside pair matches, while

$$
\ket{\phi_1}\ne\ket{\phi_3}.
$$

So the sequence is not a palindrome. The catch is that $\ket{\phi_1}$ and $\ket{\phi_3}$ might be very close as quantum states. One measurement on each need not reveal their identities with certainty.

That is where the puzzle begins.

```text
                    THE SAME FOUR-SYMBOL EXAMPLE

position        0          1          2          3
state        |phi_2>    |phi_1>     |phi_3>    |phi_2>
                |          |            |        |
                |          +---- [?] ---+        |
                + ------------- [?] ------------ +

                  outside: equal     inside: different

                      NOT A PALINDROME
                      but how do we certify it?
```

*The state names are annotations for the reader, not classical identities that the testing algorithm is given.*

## The challenge

You are given access to a length-$N$ sequence of quantum states

$$
\ket{\phi_0},\ket{\phi_1},\ldots,\ket{\phi_{N-1}}.
$$

Decide whether every mirrored pair represents the same state:

$$
\ket{\phi_i}=\ket{\phi_{N-1-i}}
$$

for all relevant $i$.

You may assume that $N$ is a power of two and that the sequence is available coherently with an index register, for example in a normalized state proportional to

$$
\sum_i\ket{\phi_i}\ket{i}.
$$

where $\braket{i|j} = \delta_{ij}$

You may not assume that different alphabet states are orthogonal or that you possess a perfect classical equality oracle of some kind which will allow you to compute equality between two states. 

What test would you design, and what correctness guarantee is physically possible?






## How to think about the problem ? 


You can start by observing one key property of the indexed quantum state 
$$
\sum_i\ket{\phi_i}\ket{i}.
$$

Even though you cannot compare the equality of the states $\ket{\phi_i}$ themselves, the indices $\ket{i}$ are classically comparable i.e you can tell the index $\ket{1}$ and $\ket{2}$ different classically because how the encoding works. 

So any quantum algorithm that will help you must somehow condition on operations that base on this index $\ket{i}$, even though a direct comparions of the state at index $\ket{i}$ and $\ket{N-1-i}$ is not possible. 


One observations, even when the states are non-orthogonal the differnece of two states are still defined, and this difference goes to zero when the two state are the same. 

For instance if you are able to produce a state, which was composed of linear combinations of the form $$\sum_i \:\:   \left( \ket{\phi_{i}}  - \ket{\phi_{N-1-i}}   \right)   \ket{i}$$, then when $\ket{\phi_{i}}  = \ket{\phi_{N-1-i}}$ the corresponding term would have simply disappeared from the the superposition.And you would have only retained terms for which the $\ket{\phi_{i}}  \neq \ket{\phi_{N-1-i}}$. 

This would have been a siginificant step, cause a palindrome would have requires all such compariosns to vanish !



That's enough of hints, happy problem solving :)








```text
                      COMPARISON WORKBENCH

                              (o_o)
                              /|\

               [ mirrored pair ] ---> [ ????? ]
                                          |
                                          v
                                   mismatch evidence

                 The missing box is the algorithm.
```
