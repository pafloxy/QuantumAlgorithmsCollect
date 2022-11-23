## Quantum Algorithms Collections
This repo is meant to be a collection of some of the simple yet useful algoirthms, that have been used to facilatte other more complicated algorithms in some of our projects. Feel free to post more problems or solutions to the mentioned probelms with appropriate documentation.

### **[Probing the nature of Boolean Functions !](https://github.com/pafloxy/QuantumAlgorithmsCollect/blob/main/Algorithms/probing_boolean_funciions.ipynb)**
The Deusth-Josza algorithm gives us an way of determining whether a given boolen fucntion $\mathtt{f}: \{0,1\}^n \to \{1,0\}$ is balanced or constant, within a limited circuit depth. However, simply knowing a function to be balanced or constatnt doesn't reveal much about the nature of the boolean function itself, for example one might be interested in knowing what are the number of inputs $\: \mathtt{\vec{x}} \:$ such that $\: \mathtt{f(\vec{x}) = 1 } \:$ or $\: \mathtt{f(\vec{x}) = 0 }\:$ for that matter. Moreover assuming the $\: \mathtt{f} \:$, to be either balanced or constant comprises the wide range of possible boolean functions.

To deal with this I have made a slight modification to or regular DJ algorithm, such that it allows us to probe into the nature of the boolean function by recasting necessary information into the amplitude of ancilla qubits. Once we done, we can read off the required iinformation from the probability distribution of the ancilla qubit itself. Below I give a brief overview of the algorithm and then move on to an example implementation

### **[Database lookup](https://github.com/pafloxy/QuantumAlgorithmsCollect/blob/main/Algorithms/database_lookup_algorithm.ipynb)**

Here our aim is to check whether a particular lookup ket say $\ket{\phi}$ exists within a set of provided set of kets say $\mathcal{S} = \{ \ket{\psi_1}, \ket{\psi_2}, .. . ,\ket{\psi_n }  \}$, and if so then what is the index of that particular ket in $\mathcal{S}$. For example, if $\ket{\phi} = \ket{\psi_3}$ we want to know the index $3$ using our algorithm.

In later generalisation we can try to compare two different sets , say $\mathcal{S_1}$ and $\mathcal{S_2}$ to find an estimate of the number of elements at the intersection of of two sets, and the idexto those elements too.

Note also that we want our algorithm to work even while the elements of $\mathcal{S}s$ are not all orthogonal to each other, as otherwise it would be of no real to use a quantum computer in the first place.geves


### **[Entanglement Preserving Partial Grover](https://github.com/pafloxy/QuantumAlgorithmsCollect/blob/main/Algorithms/entanglement_preserving%20_grover.ipynb)**

Assume that you have a state of form,
$\ket{\psi} \:=\: c_g \ket{g}\ket{f(g)} \:+\: c_b \ket{b}\ket{f(b)}$
where $\ket{g}$ is something we refer as `good` states and $\ket{b}$ as `bad` states and $f$ is a randomised black box function of whose action is not known to us i.e we do not know what $\ket{f(g)}$ or $\ket{f(b)}$ is, moreover it might yield different values upon every call, but it is somehow dependent on the ket to which it is entangeld to i.e either $\ket{g}$ or $\ket{b}$

Given this the challenge is to amplify the states corresponding to the `good` states $c_g$. Notice that regular `Grover Search` cannot be used in this case because we do not have sufficient information about the initial state $\ket{\psi}$ to construct the `diffuser`!


### **[Checking whether a given sequence is a Palindrome](https://github.com/pafloxy/QuantumAlgorithmsCollect/blob/main/Algorithms/quantum-palindrome-check.ipynb)**
To check whether a given sequence is a palindrome or not is a classic programming problem. Recently I came across a quantum version of it at [https://github.com/qosf/monthly-challenges}{qosf-monthly-challenges], where the challenge was to check whether a given sequence of integer is a palindrome or not by using a QRAM structure with added comparators. However, here I will show how we can check for a palindorme even if the alphabets choosen is quantum one, thus genralising the idea of palindrome checking! And it will be an efficient one ...




[![CC BY-NC-SA 4.0][cc-by-nc-sa-shield]][cc-by-nc-sa]

This work is licensed under a
[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License][cc-by-nc-sa].

[![CC BY-NC-SA 4.0][cc-by-nc-sa-image]][cc-by-nc-sa]

[cc-by-nc-sa]: http://creativecommons.org/licenses/by-nc-sa/4.0/
[cc-by-nc-sa-image]: https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png
[cc-by-nc-sa-shield]: https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg
