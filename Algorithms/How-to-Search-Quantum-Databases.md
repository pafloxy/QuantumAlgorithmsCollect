
# How Do You Search When the Database Entries Are Not Perfectly Distinguishable?

**Assumed knowledge :** Basic idea of quantum algorithms, fidelity measures, QRAM architectures

[Series index](README.md) 

---

Ordinary database lookup is familiar:

```python
for i, item in enumerate(database):
    if item == query:
        return i
```

The interesting line is not the loop. It is `item == query`.

We usually treat equality as free. Now let the query and every database item be states that may overlap. One item may be an exact match while another is almost indistinguishable from it.

Can you compare all entries coherently and still recover the exact matching index?

```text
                         THE LOOKUP DESK

        QUERY                     STATE DATABASE
     +---------+                +----------------+
     |   phi   |                | 0 : psi_0      |
     +---------+                | 1 : psi_1      |
          \                     | 2 : psi_2      |
           \                    | 3 : psi_3      |
            \                   +----------------+
             \                         /
              +--------> (o_o) <-------+
                         /|\
                          |
                          v
                      index = ?

                    "Just use ==" is not a plan.
```

*These names label states for the reader; they are not classical descriptions supplied to the algorithm. The cabinet is a schematic, not a data-loading circuit.*

## A database of states

Let the query be $\ket{\phi}$ and the database contain

$$\mathcal S= \left\{ \ket{\psi_0}, \ket{\psi_1}, \ldots, \ket{\psi_{N-1}} \right\}.$$

The entries need not be mutually orthogonal. Two different entries can therefore have a large overlap with the query.

For intuition, imagine:

```text
             OPENING EXAMPLE: SIMILARITY TO QUERY

index     fidelity     schematic bar
  0         0.80       [####################-----]
  1         0.93       [#######################--]
  2         1.00       [#########################]  exact match
  3         0.12       [###----------------------]

Bars are rounded. The printed numbers are the stated fidelities.
These are example data for the reader, not free oracle outputs.
```

The numbers represent $|\braket{\phi|\psi_i}|^2$. The task is to recover index `2` without assuming the states arrive with useful classical labels.

## The access model

To focus the puzzle, assume the database has already been loaded coherently. You receive a normalized state of the form

$$\frac{1}{\sqrt N} \sum_{i=0}^{N-1}\ket{\psi_i}\ket{i}$$

and a copy of the query state $\ket{\phi}$.

This is a strong assumption. The puzzle does not hide or solve the data-loading cost. It asks what comparison-and-retrieval procedure follows after coherent indexed access is available.

```text
                  WHERE THE PUZZLE STARTS

DATA LOADING                GIVEN TO YOU              YOUR TASK
(not solved here)          coherent access

[ physical data ] ----> [ item + index registers ] ----> [ ????? ]
                            + query state                  |
                                                           v
                                                     classical index

               Access is assumed. Retrieval is not.
```

## The challenge

You are given:

- a query state $\ket{\phi}$;
- coherent indexed access to database states $\ket{\psi_i}$;
- no promise that the database states are mutually orthogonal.

Determine whether there is an index $i$ such that

$$\ket{\psi_i}=\ket{\phi},$$

and, if possible, recover that index.

You may coherently manipulate the query, item, index, and ancilla registers. You may not assume a free classical equality test for states.

Can you compare all candidates in a way that leaves enough usable information to retrieve the index corresponding to an exact match?





## Why "fidelity" is not the best approach. 


From the typical insight that we might gather from our quantum computing classese, you migh be inclided to use fidelity as a means to find the state, however recognise the two  difficulties that arise when we rely on fidelity: First it requires exponential number of measurements to estimate the fidelity, moreover once the fidelities are computed, the accumulated numerics migh prevent us to be certain of the state actually matching. 

Take four entries and suppose

$$\left|\braket{\phi|\psi_0}\right|^2=0.64, \qquad \left|\braket{\phi|\psi_1}\right|^2=0.92,$$

$$\left|\braket{\phi|\psi_2}\right|^2=1, \qquad \left|\braket{\phi|\psi_3}\right|^2=0.04.$$

```text
             THIS SECTION'S FOUR-ENTRY EXAMPLE

index     fidelity          25-cell scale
  0         0.64            [################---------]
  1         0.92            [#######################--]  near
  2         1.00            [#########################]  exact
  3         0.04            [#------------------------]

                 near match: 0.92
                exact match: 1.00
                        gap: 0.08

          What guarantee will separate the two?
```

Index $2$ is the exact match. Index $1$ is close enough that finite statistical evidence may be difficult to distinguish from equality.

How should an exact lookup procedure treat that gap? What changes if nonmatching states can approach the query arbitrarily closely?

*A schematic precision trap—not a measurement protocol:*

```text
TRUE FIDELITY             ROUNDED TO TWO DECIMAL PLACES

  0.999999  ---------------------->  1.00   not exact
  1.000000  ---------------------->  1.00   exact
                                     ^
                                     |
                           same displayed number;
                           different lookup answers
```



## A hint 

The main hint to make use of the indexed structure you receive, i.e : 

 $$\frac{1}{\sqrt N} \sum_{i=0}^{N-1}\ket{\psi_i}\ket{i}$$

Sicne fidelity only reveals approximate closeness, you would like something that reveal exact equality. This would have been difficult in general, but the indexed structure that we have here allows such a possibility. Though I will not give out an exact solutions try to see if you can prepare a state of the form : 

 $$\frac{1}{\sqrt N} \sum_{i=0}^{N-1}  \left( \ket{\psi_i} - \ket{\phi}   \right)   \ket{i}$$ 

Because, if you can then for an exact equality , say $\ket{\psi_i} = \ket{\phi}$, the superposition is guranteed to not have the index register in the state $\ket{i}$. But then it remains a challenge to see if you can identify that $i$ was missing from the superposition. 