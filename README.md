# Grover's Attack Simulation on a Toy Learning-With-Error Instance (CRYSTALS-Kyber)

This repository explores the theoretical vulnerabilities of lattice-based cryptography by simulating a quantum exhaustive search (Grover's algorithm) against a miniaturized Learning With Errors (LWE) problem. 

This project serves as a follow-up to my previous work on **Breaking RSA using Shor's Algorithm**. While Shor's algorithm fundamentally breaks classical asymmetric encryption (like RSA) in polynomial time, Post-Quantum Cryptographic (PQC) schemes like CRYSTALS-Kyber rely on lattice problems (MLWE) which are widely considered resistant to both classical and quantum attacks. This repository tests that resistance by seeing how Grover's algorithm interacts with a simplified LWE framework.

## Theoretical Background
CRYSTALS-Kyber's security is anchored in the difficulty of recovering a secret vector **s** from a noisy linear system:  
**t = (A · s) + e mod q**

Where:
* **A** is a known public matrix.
* **t** is the public target vector.
* **e** is a small, random error vector (noise).
* **s** is the secret vector we want to find.

Without the noise (**e**), this would be a simple linear algebra problem. With the noise in a high-dimensional space, it becomes computationally intractable. 
## The Simulation
Instead of a full-scale Kyber implementation (which involves polynomial rings and hundreds of dimensions), this repository uses a "toy" LWE instance to make quantum simulation feasible on current classical hardware via Qiskit's AER Simulator.

### Toy Parameters
* Modulus `q = 4`
* 4 Qubits total (representing a 2-element secret vector `s` with 2 bits each)
* Tolerance `delta = 1`

### The Quantum Approach
Because the error vector **e** is unknown, a traditional exact-match Grover oracle won't work. Instead, we relax the verification step. The oracle in this simulation amplifies the amplitude of all candidate vectors **s** that generate an approximation of **t** within a fixed tolerance (`delta`). 

While this doesn't "break" the encryption, it successfully narrows down the search space to a subset of plausible pre-images. 

## Results and Conclusion
The simulation successfully outputs the most probable bitstrings that satisfy the noisy equation within the defined tolerance. 
**Disclaimer:** This is purely a proof-of-concept. For realistic parameter settings (like those used in NIST-standardized Kyber512, Kyber768, or Kyber1024), the combinatorial complexity of the multidimensional grid and the specific Gaussian distribution of the error make a practical application of this Grover-based attack unscalable. However, it provides valuable insight into how quantum search spaces can be manipulated for cryptanalysis.
