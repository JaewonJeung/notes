## Components
- Qubit
	- Characterized by amplitudes, and these are actual physical components
- Amplitude
	- You can sort of think of it as probability (but it doesn't go from 0 to 1 since it's in the form of $a+bi$, using imaginary numbers). But a constraint is made on what the amplitudes can be so that we can represent them from 0 to 1 probability
	- Amplitude of a single qubit can be represented as $\psi = (1/\sqrt{2}|0\rangle + (1/\sqrt{2}|1\rangle$   
		- This means that qubit psi has two states 0 and 1 with amplitudes of $1/ \sqrt{2}$ each
		- Getting the modulus (hypotenuse of complex number plain) of it, you get the actual probability. In this case, 0 and 1 each have 1/2 probability of appearing for this qubit
	- A group of qubits, say 4 qubits can be represented like $\alpha_0|0000\rangle$  + $\alpha_1|0001\rangle$  + $\alpha_2|0010\rangle$  ... $\alpha_{15}|1111\rangle$ that is, the probability of getting 0000, '' for 0001, and so forth
	- You can represent a group of qubits with a variable, say $\psi$ like this, $|\psi \rangle$ 
- Quantum gate
	- Something that transforms the amplitude of qubits. Likened to matrix multiplication of a vector, except this is done through physical transformation of physical particles rather than actual matrix multiplication
	- The profound idea is that a quantum gate affecting n-qubits has an effect on $2^n$ amplitudes 
	- And due to the inversibility of matrices, you can compute back the original qubits
	- Schema of quantum gates is called "quantum circuit"

## Quantum speedups
- Speedups (compared to classical computers) to solve a problem is what is important
- Grover's algo
	- Searching n items in O($\sqrt n$)
- Simon's problem
	- Let $f(x)$ be a function that returns a random-looking n-bit string with n-bit string input, but with a property of $f(x) = f(y)$ iff $y = x \oplus m$ 
	- Finding $m$ given access to $f$ 
	- $2^n$ search problem, so in classical computers, finding a collision in $2^{n/2}$ ops
	- Quantum algo speedup can do this in $O(n)$
- Shor's algo
	- Efficiently Finds a period, $\omega$, when $f$ is a periodic function, that is, $f(x+ \omega) = f(x)$ 
	- This has an impact on RSA mod prime factoring and mod discrete log problem
	- Can compute these classical exponential problems in polynomial time

## Post-quantum crypto algos
* Base algos
	* Code-based 
	* Lattice-based
		* closest-vector-problem (CVP). Finding the vector in a lattice closest to a given point
	* Multivariate-based
	* Hash-based 
- 2022 NIST standards
	- Kyber - Module-Lattice (ML) based key encapsulation/encryption scheme for key agreements
	- Dilithium - ML based signature scheme
	- Falcon - Lattice based signature scheme, different assumptions than Dilithium. Around half as short signature length
	- SPHINCS+ - Hash-based signature scheme

## Things to note for PQC transition 
- Post quantum encryption is more important than post quantum signatures
- Say PQC transition happened
	- Then for signatures, you can revoke the previous certs and compute fresh signatures with the updated scheme
	- But if we assume the transmitted ciphertexts are stored, it would be broken 
- What about key agreement? (e.g. DHKE, EC-DHKE)
	- In many cases, the protocol implementation details are more than just simple DHKE
	- For example, Signal's DHKE computation happens for every message, but on top of the public numbers that are generated with DH, it's combined with an internal state of the messages (internal secrets)
	- This makes it pretty difficult for both classical and quantum computers


