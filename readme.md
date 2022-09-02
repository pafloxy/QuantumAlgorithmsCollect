**1.  Probing the nature of Boolean Functions !**

The Deusth-Josza algorithm gives us an way of determining whether a given boolen fucntion $ \mathtt{f}: \{0,1\}^n \to \{1,0\} $ is balanced or constant, within a limited circuit depth. However, simply knowing a function to be balanced or constatnt doesn't reveal much about the nature of the boolean function itself, for example one might be interested in knowing what are the number of inputs $\: \mathtt{\vec{x}} \:$ such that $\: \mathtt{f(\vec{x}) = 1 } \:$ or $\: \mathtt{f(\vec{x}) = 0 }\:$ for that matter. Moreover assuming the $\: \mathtt{f} \:$, to be either balanced or constant comprises the wide range of possible boolean functions.

To deal with this I have made a slight modification to or regular DJ algorithm, such that it allows us to probe into the nature of the boolean function by recasting necessary information into the amplitude of ancilla qubits. Once we done, we can read off the required iinformation from the probability distribution of the ancilla qubit itself. Below I give a brief overview of the algorithm and then move on to an example implementation

**2.  Database lookup**

Here our aim is to check whether a particular lookup ket say $\ket{\phi}$ exists within a set of provided set of kets say $ \mathcal{S} = \{ \ket{\psi_1}, \ket{\psi_2}, .. . ,\ket{\psi_n }  \}$, and if so then what is the index of that particular ket in $\mathcal{S}$. For example, if $\ket{\phi} = \ket{\psi_3}$ we want to know the index $3$ using our algorithm.

In later generalisation we can try to compare two different sets , say $\mathcal{S_1}$ and $\mathcal{S_2}$ to find an estimate of the number of elements at the intersection of of two sets, and the idexto those elements too.

Note also that we want our algorithm to work even while the elements of $\mathcal{S}$ are not all orthogonal to each other, as otherwise it would be of no real to use a quantum computer in the first place.geves








Shield: [![CC BY-NC-SA 4.0][cc-by-nc-sa-shield]][cc-by-nc-sa]

This work is licensed under a
[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License][cc-by-nc-sa].

[![CC BY-NC-SA 4.0][cc-by-nc-sa-image]][cc-by-nc-sa]

[cc-by-nc-sa]: http://creativecommons.org/licenses/by-nc-sa/4.0/
[cc-by-nc-sa-image]: https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png
[cc-by-nc-sa-shield]: https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg