# How to do Grover's algorithm, when you don't know what you are looking for ?


I will assume you know the basic idea of Grover's algorithm. [3Blue1Brown's explainer](https://www.youtube.com/watch?v=RQWpF2Gb-gU) is a good place to start otherwise.

Below we will discuss a slightly modified version of the Grover's algorithm. In traditional grover, at least what you might have learnt in class, you are usually told the bitstring you are looking for based on which you design the oracle and the diffuser and an iterative applicaiton of the oracle and the diffuser unitaries is supposed to enhance the amplitude of the bitstring we are looking for. 

However this particular approach hides a lot of the assumptions that are implicit in the Grover's algorithm, and the aim here is to bring this assumptions to light by slightly modifying the underlying primitives. 

I will start by breifly reminding you about the ususal primitives of the Grover's algorithm for small example and then introduce the altered version --- ofcourese by practically motivating it --- and we will examine how things change. Spoilier alert, I will restrict the number of times you can call the oracle to be just one, and show how a naive workaround --- that though seem very Grover-like will simply fail to amplify the good-state. 



## First, an ordinary two-qubit search

Let's start with the two-qubit case, the computational basis in this case is spanned by $\ket{00}$, $\ket{01}$, $\ket{10}$, and $\ket{11}$. And let's imagine $\ket{w}=\ket{10}$ is the good state we are looking for. 


By convention, we define the oracle unitary as $U_w=I_4-2\ket{w}\bra{w}$. For our chosen $w$, we can construct it explicitly as :
$$U_{10} = (I_2\otimes X)\,\mathrm{CZ}\,(I_2\otimes X).$$

And the diffuser unitary is defined as  
$D_s=2\ket{s}\bra{s}-I_4$.





We start with the equal superposition of all the computational basis states :
$$\ket{s} = \frac{1}{2} \left( \ket{00}+\ket{01}+\ket{10}+\ket{11} \right).$$


As known, the action of the oracle unitary
$$U_{10}\ket{s} = \frac{1}{2} \left( \ket{00}+\ket{01}-\ket{10}+\ket{11} \right).$$ flips the sign of the bistring that we are looking for. 

And in this case you can see that applying the diffuser follwed by the diffuser on $\ket{s}$ maps it to $w$
$$D_sU_{10}\ket{s}=\ket{10}.$$
Which is exactly what you would expect with Grover's algorithm. No surprises as of now. 




## Now, imagine a more general and nuances situation


So Grover wasn't wrong, the algorithm indeed amplifies the state we were looking for, good for him :) But what happens when calling we don't know what state (i.e the $\ket{w}$ in the above example) we want to look for and using oracle's that can identify the state for us is expensive ?  



Imagine that you were trying to solve an optimization problem using a quantum computer (that's really an application people think about, and it was indeed the attempt to solve one that lead me to this question :). You are handed an unknown n-qubit state 

$$\ket{\psi} = \sum_{x \in \{0,1\}^{n}} \: c_x \ket{x}$$ 
  
and you are told that there is one computational basis state $\ket{w}$ that is a solution to the optimization problem and your job is to amplify that probability of this state. 


Now ofcourse you have no idea what $\ket{w}$ is, if you did you would have already solved the optimization in the first place. Now if you compare what we did in the previous section, we built the oracle unitary $U_{w}$ with explcit information of the $\ket{w} = \ket{01}$ which was known to us, but here we cannot do that. So how do we build the oracle ? 


Well those of you paid real attention might complain, the actual construction of Grover's algorithm does not require us to know how the oracle unitary is constructedm, it assumes that **there simply exsits an oracle unitary** that flips the sign! Let me tell you guys, this is exactly how they fool us --- with smart assumptions --- without 

So in principle you can still proceed with Grover's algorithm by simply assuming that such an $U_w$ exists. But here I introduce a twist that will make things difficult: **The oracle $U_{w}$ is so expensive that you could only use it once.** --- imagine being an underpaid PhD student :/

This is a real problem if you think about it, the general Grover's algorithm requires applying the oracle unitary and diffuser unitary many times before the solution could be reached. So, what can you do now ? 


Well, there is one idea the "oracle" seller gives you. He has a modified version of the oracle unitary $U'_{w}$ which can instead of flipping the sign of $\ket{w}$ can identify it on an ancilla. I.e you start with the state $\ket{\Psi} = \ket{\psi} \ket{0}$, applying this oracle does the following :

$$ \ket{\Psi'} =  U'_{w} \ket{\psi}\ket{0} = \sum_{x \neq w} c_x \ket{x} \ket{0}  + c_w \ket{w}\ket{1}  $$

So this allows you to store about the right state on a separate qubit. But again this oracle is also as expensive, and you have the money to apply it only once. 


So the question now is, can you do somehow still use some version of the Grover's algorithm to amplify the state $\ket{w}$ ? 





[Restate the question, bit formally for furhter use downstream use] 


> **Problem statement.**
>
> You are given an unknown $n$-qubit state
>
> $$\ket{\psi} = \sum_{x\in\{0,1\}^n}c_x\ket{x},$$
>
> and there exists one unknown computational-basis state $\ket{w}$ that we call the good state.
>
> You are allowed exactly **one** call to an expensive oracle $U'_w$, which writes whether a basis state is $w$ into a one-qubit ancilla:
>
> $$U'_w\ket{\psi}\ket{0} = \sum_{x\neq w}c_x\ket{x}\ket{0} + c_w\ket{w}\ket{1} = \ket{\Psi'}.$$
>
> After this call you cannot use $U'_w$, or another oracle that knows $w$, again.
>
> Starting only from $\ket{\Psi'}$ and operations that do not require knowing $w$, can you increase
>
> $$\Pr(\text{main register}=w) = |c_w|^2$$
>
> in a Grover-like way?
>
> In particular, can the ancilla—which now tells us which branch is good—be used as a cheap replacement for repeatedly calling the original oracle?




## Question: Grover on the Ancilla space ?


You are back in your lab with $\ket{\Psi'}$. And you come make the following observations about the conventional Grover : 


1. The initial state you are starting from is always the equal superposition of all bitstrings $\ket{s} = \sum_{x} \ket{x}$.
2. The oracle unitary can be applied as many times as possible 


In your case, first, you dont have an oracle unitary in the usual sense. Second, since the state $\ket{\psi}$ is unknown you cannot really use it to build the diffuser either. So what do yo do ? 


Well, there is an observation you can make. Even though on the actual register i.e where $\ket{\Psi'}$ lives doing Grover seems complicated, on the ancilla register things seem rather intersting.

This is because even though you dont know what $\ket{w}$ is on the main register, the entangled ancilla must be in $\ket{1}$ whenever the main register is in $\ket{w}$. So instead of flipping the sign of the amplitude of state with $\ket{w}$ on the main register, flipping the sign of the state with $\ket{1}$ on the ancilla register will have the effect of : "flipping the ket which has the solution you are looking for". 

And since you know the bit-string, the oracle unitary can be constructed simply as $$U_{1} = I_{main} \otimes \left(   I - 2 \ket{1}\bra{1} \right)_{ancilla}$$.


Its easy to verify that $$U_{1} \ket{\Psi'} = \sum_{x \neq w} c_x \ket{x} \ket{0}  - c_w \ket{w}\ket{1}$$. So it has almost the intended effect. 

**But how do we construct the diffuser ? And can we still recover the usal Grover's algorithm in this case ?**

Unfortunately I will stop here, the challenge for you is to see if you can recover the Grover like amplitude amplification in this case. 

[restate the question]

> **The challenge, in one line:** starting from
>
> $$\ket{\Psi'} = \sum_{x\neq w}c_x\ket{x}\ket{0} + c_w\ket{w}\ket{1},$$
>
> with no more calls to the expensive oracle, can you construct a $$w$$-independent Grover-like iteration that makes
>
> $$\Pr(\text{main register}=w)$$
>
> larger than $$|c_w|^2$$?
>
> The ancilla gives you a cheap way to **mark** the good branch. The question is whether there is also a cheap **diffuser** that actually amplifies $\ket{w}$ rather than merely changing the ancilla.



Below is an example where I show you what possible erros might occur, and what that tells us about being the Grovers algorithm. 

[insert the same exact example]

### A tempting attempt — Grover only the ancilla

Let us return to exactly the two-qubit example from the beginning.

Take

$$\ket{\psi} = \ket{s} = \frac{1}{2} \left( \ket{00} + \ket{01} + \ket{10} + \ket{11} \right),$$

and, unknown to the algorithm, let $w=10$.

After spending our single expensive call to $U'_w$, we have

$$\ket{\Psi'} = \frac{1}{2} \left( \ket{00}\ket{0} + \ket{01}\ket{0} + \ket{10}\ket{1} + \ket{11}\ket{0} \right).$$

The ancilla perfectly identifies the good branch.

We already know how to mark ancilla state $\ket{1}$:

$$U_1 = I_{\mathrm{main}} \otimes \left( I-2\ket{1}\bra{1} \right).$$

Therefore

$$U_1\ket{\Psi'} = \frac{1}{2} \left( \ket{00}\ket{0} + \ket{01}\ket{0} - \ket{10}\ket{1} + \ket{11}\ket{0} \right).$$

Now we need something playing the role of the diffuser.

Since the ancilla is only one qubit, a tempting choice is

$$D_{\mathrm{anc}} = I_{\mathrm{main}} \otimes \left( 2\ket{+}\bra{+}-I \right).$$

For one qubit,

$$2\ket{+}\bra{+}-I=X,$$

so

$$D_{\mathrm{anc}} = I_{\mathrm{main}}\otimes X.$$

Applying it gives

$$D_{\mathrm{anc}}U_1\ket{\Psi'} = \frac{1}{2} \left( \ket{00}\ket{1} + \ket{01}\ket{1} - \ket{10}\ket{0} + \ket{11}\ket{1} \right).$$

Now something suspicious has happened.

Before the iteration, $\Pr(\text{ancilla}=1)=\frac14$. After it, $\Pr(\text{ancilla}=1)=\frac34$.

If we watched only the ancilla, this would look like amplification.

But the probability that the **main register actually contains the solution** is still $\Pr(\text{main}=10)=\frac14$.

Nothing has amplified $\ket{10}$.

In fact, the branches carrying ancilla $1$ are now

$$\ket{00}\ket{1}, \qquad \ket{01}\ket{1}, \qquad \ket{11}\ket{1},$$

which are precisely the three wrong answers.

The actual solution is sitting under ancilla $0$:

$$-\ket{10}\ket{0}.$$

So $\Pr(\text{main}=10\mid\text{ancilla}=1)=0$.

We produced more $1$s by making the ancilla stop telling the truth.

What happens if we repeat the procedure?

Let $Q=D_{\mathrm{anc}}U_1$. Since on the ancilla this is just $XZ$, $Q^2=-I$. Therefore, up to global phase, $Q^2\ket{\Psi'}=\ket{\Psi'}$.

So the process merely oscillates:

| Number of rounds | $\Pr(\mathrm{ancilla}=1)$ | $\Pr(\mathrm{main}=10)$ |
|---|---:|---:|
| $0$ | $1/4$ | $1/4$ |
| $1$ | $3/4$ | $1/4$ |
| $2$ | $1/4$ | $1/4$ |
| $3$ | $3/4$ | $1/4$ |

There is no progressive concentration of amplitude on $$\ket{10}$$.

The candidate probabilities never changed. Only the correlation between the candidate and its flag changed.

> **Amplifying the probability that the ancilla says "good" is not the same thing as amplifying the probability of the good state itself.**

So the easy part was constructing the new oracle.

The difficult part was the diffuser.