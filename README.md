# Quantum Algorithms Collect

A collection of quantum-computing problems, thought experiments, and explanations built around a habit I picked up while teaching myself quantum algorithms:

> **Take something familiar, change one of the assumptions that makes it work, and see what survives.**

A lot of the earlier problems here began when I was an undergraduate trying to learn quantum computing on my own.

Quantum algorithms often felt stranger to me than classical algorithms. With classical algorithms, I was used to pulling a problem apart: remove an operation, weaken an assumption, change the input model, and see where the original method stops working.

So I started doing the same thing with quantum algorithms.

Instead of only asking *how does this algorithm work?*, I would ask what happens when one of the primitives it normally gets for free suddenly disappears.

## Problems from when I was learning

These mostly started as attempts to teach myself familiar quantum algorithms by deliberately making their lives more difficult.

### [Can You Check a Palindrome When Equality Is Not Free?](Algorithms/How-to-Palindrome-When-its-Quantum.md)

Palindrome checking is easy when `a == b` is free. What happens when the symbols are quantum states that may not be perfectly distinguishable?

### [Deutsch–Jozsa but Realistic](Algorithms/DJ-Algorithm-but-Realistic.md)

Drop the constant-versus-balanced promise. Can the same basic machinery tell us something quantitative about a general Boolean function instead?

### [How Do You Search a Quantum Database?](Algorithms/How-to-Search-Quantum-Databases.md)

Suppose both the query and the database entries are quantum states. Comparing them is one problem; recovering the actual matching index is another.

### [How to Do Grover When You Don't Know What You Are Looking For](Algorithms/How-to-Grover-When-You-don't-know-What-to-Look-For.md)

Suppose an expensive oracle can identify the good branch, but you may use it only once. What, if anything, survives of Grover-style amplitude amplification?

The idea behind most of these was simple:

```text
familiar quantum algorithm
          ↓
remove something it normally gets for free
          ↓
what was that primitive actually doing?
```

## Problems that grew out of research

Years later, during my PhD, I found myself doing almost the reverse.

Now the starting point can be a problem coming directly from quantum-computing research. Instead of making a familiar algorithm stranger, I try to strip away enough quantum formalism that the underlying algorithmic problem becomes familiar again.

### [How Quantum Computing Gets You to a Nightclub](Algorithms/How-Quantum-Computing-Gets-You-To-A-Nightclub.md)

This one grew out of ideas I worked on during my PhD.

The exposition begins with a nightclub queue: creatures with badge codes, bouncers, masks, and rules about who may move past whom.

Underneath the story is a problem about dependencies, compatibility, rewriting operations, and deciding which parts of a quantum computation can actually matter to a chosen output.

The nightclub is not just an analogy pasted on top of the mathematics. The point is to expose a classical combinatorial problem that was already hiding inside the quantum one.

```text
quantum research problem
          ↓
strip away the unnecessary formalism
          ↓
what algorithmic structure is underneath?
```

I expect more of the newer problems in this repository to come from this direction.

## The common thread

Some entries here contain complete constructions. Some deliberately stop at the point where the interesting unresolved problem begins. Some are closer to puzzles than tutorials.

They span different stages of how I learned and worked with quantum computing, but the instinct behind them has stayed surprisingly constant:

> **Remove a primitive, weaken an assumption, or change the representation — and use what changes to understand the problem better.**

**The problems changed; the way I like to understand them did not.**
