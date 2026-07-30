# Constraint Systems

The first step in constructing efficient proof systems is **arithmetization**, which transforms a circuit or program into a system of algebraic constraints that can be represented and verified efficiently. Arithmetization played a central role in early results on the computational power of interactive proofs, including the algebraic methods of Lund, Fortnow, Karloff, and Nisan[^1], and Shamir’s proof that $\mathsf{IP}=\mathsf{PSPACE}$[^2].

## Rank 1 Constraint System (R1CS)

R1CS[^3] transforms an arithmetic circuit into an algebraic constraint system. Its core idea is to represent each multiplication gate as a constraint while incorporating additions and scalar multiplications into linear combinations. For instance, $(1+3x_0+w_0)\cdot x_1=w_1$ represents a multiplicative gate that takes the linear combination of the constant $1$, the scalar multiple $3x_0$, and the witness value $w_0$ as its left input and the instance value $x_1$ as its right input, and outputs $w_1$. The constraint can be written as $\left<\mathbf{a},\mathbf{z}\right>\cdot\left<\mathbf{b},\mathbf{z}\right>=\left<\mathbf{c},\mathbf{z}\right>$, where $\mathbf{a}=(1,3,0,1,0)$, $\mathbf{b}=(0,0,1,0,0)$, $\mathbf{c}=(0,0,0,0,1)$, $\mathbf{z}=(1,x_0,x_1,w_0,w_1)$. Stacking the vectors as rows of matrices yields an R1CS instance, specifically, 

$\mathbf{z}=(1\Vert\mathbf{x}\Vert\mathbf{w}),(\mathbf{A}\cdot\mathbf{z})\circ(\mathbf{B}\cdot\mathbf{z})=\mathbf{C}\cdot\mathbf{z}.$

R1CS is supported by a mature ecosystem of libraries. In practice, developers describe the relation to be proven using a frontend language such as Circom. The compiler then translates this description into an R1CS instance and a witness-generation code . Many proofs systems are built on R1CS, e.g., Aurora[^4], Marlin[^5], Fractal[^6] and Bulletproofs[^7]. 

A glimpse of  what comes next (univariate version): 

Let $H$ be a multiplicative subgroup of $\mathbb{F}$, the prover interpolates $\mathbf{A}\cdot\mathbf{z},\mathbf{B}\cdot\mathbf{z},\mathbf{C}\cdot\mathbf{z}$ over H to get $\tilde{z}_A(X),\tilde{z}_B(X),\tilde{z}_C(X)$ and commits to them using [polynomial commitments](https://flippingacoin.github.io/polynomial-commitments). Then it needs to prove

$$\tilde{z}_A(h)\cdot\tilde{z}_B(h)=\tilde{z}_C(h),\forall h\in H,$$

$$\tilde{z}_M(h)=\sum_{j\in H}\widetilde{M}(h,j)\tilde{z}(j),\forall h\in H,M\in \{A,B,C\}.$$

The above relations can be reduced to univariate sumcheck protocol. 

## Algebraic Intermediate Representation (AIR)

AIR[^8] refers to "algebraize the intermediate representations of the program", It is the core arithmetization and constraint system used in STARK-based proof systems.

Unlike R1CS, AIR transforms the computation program directly to constraints. An algebraic execution trace of a program can be represented by a $\mathsf{T}\times\mathsf{w}$ array, where each row represents the state at one execution step, and each column records the value of one register over time. The computation integrity can be expressed using

* transition constraints: $$\mathbf{P}(reg[t],reg[t+1])=0, \forall t\in[\mathsf{T}-1]$$, where $reg[t]=(reg_0[t],...,reg_{\mathsf{w}-1}[t])$, and $\mathbf{P}$ is a collection of low-degree polynomials. These constraints enforce the validity of all $\mathsf{T}-1$ state transitions.
* boundary constraints: $reg_i[j]=\alpha,\forall(i,j,\alpha)\in B$. These constraints are commonly used to bind the trace to public inputs, outputs, and other specified values.

Each register column is then represented as a polynomial through interpolation. For every $i\in[\mathsf{w}]$, a polynomial $\widetilde{reg}_i(X)$ is interpolated, s.t., $\widetilde{reg}_i(g^0)=reg_i[0],...,\widetilde{reg}_i(g^{\mathsf{T}-1})=reg_i[\mathsf{T}-1]$. 

The polynomial relations are then checked in a STARK[^8] using a FRI-based polynomial commitment scheme. To support efficient FFT-based interpolation and recursive FRI[^9] folding, $\mathsf{T}$ is generally padded to a power of two. 

AIR is particularly suitable for iterative computations such as  hash functions and virtual-machine execution. In fact, zkVMs such as Cairo[^10] and Miden[^11] are built on AIR.

## PLONKish

PLONK[^12] introduced an arithmetization that forms the basis of what is now commonly called the PLONKish constraint system. Briefly speaking, suppose that a circuit has $n$ gates and $m$ wires, whose values are represented by $\mathbf{x}\in\mathbb{F}^m$.

Let $\mathbf{a},\mathbf{b},\mathbf{c}\in[m]^n$ represent the indices of the left-input, right-input, and output wires of the $n$ gates, respectively. Let $\mathbf{q_L},\mathbf{q_R},\mathbf{q_O},\mathbf{q_M},\mathbf{q_C}\in\mathbb{F}^n$ be the selector vectors for the left-input, right-input, output, multiplication, and constant terms, respectively. A wire assignment $\mathbf{x}$ satisfies the circuit if the following constraints hold.

* $$\forall i\in[n],(\mathbf{q_L})_i\cdot\mathbf{x}_{\mathbf{a}_i}+(\mathbf{q_R})_i\cdot\mathbf{x}_{\mathbf{b}_i}+(\mathbf{q_O})_i\cdot\mathbf{x}_{\mathbf{c}_i}+(\mathbf{q_M})_i\cdot(\mathbf{x}_{\mathbf{a}_i}\mathbf{x}_{\mathbf{b}_i})+(\mathbf{q_C})_i=0$$
* All wire occurrences referring to the same circuit wire must carry the same value. This is further proved with permutation check. 

Modern PLONKish systems further support custom gates and lookup arguments[^13], while HyperPlonk[^14] uses multilinear polynomials and sumcheck to handle high-degree constraints more efficiently.

## CCS[^15]

(🚧 todo)

## Arithmetization over Binary Fields[^16]

(🚧 todo)

## References

[^1]: Carsten Lund, Lance Fortnow, Howard J. Karloff, Noam Nisan. Algebraic Methods for Interactive Proof Systems. J. ACM 39(4): 859-868 (1992)
[^2]: Adi Shamir. IP = PSPACE. J. ACM 39(4): 869-877 (1992)
[^3]: Jens Groth. On the Size of Pairing-Based Non-interactive Arguments. EUROCRYPT 2016: 305–326.

[^4]: Eli Ben-Sasson, Alessandro Chiesa, Michael Riabzev, Nicholas Spooner, Madars Virza, Nicholas P. Ward. Aurora: Transparent Succinct Arguments for R1CS. EUROCRYPT 2019: 103–128.

[^5]: Alessandro Chiesa, Yuncong Hu, Mary Maller, Pratyush Mishra, Noah Vesely, Nicholas P. Ward. Marlin: Preprocessing zkSNARKs with Universal and Updatable SRS. EUROCRYPT 2020: 738–768.

[^6]: Alessandro Chiesa, Dev Ojha, Nicholas Spooner. Fractal: Post-Quantum and Transparent Recursive Proofs from Holography. EUROCRYPT 2020: 769–793.

[^7]: Benedikt Bünz, Jonathan Bootle, Dan Boneh, Andrew Poelstra, Pieter Wuille, Greg Maxwell. Bulletproofs: Short Proofs for Confidential Transactions and More. IEEE Symposium on Security and Privacy: 315–334 (2018).

[^8]: Eli Ben-Sasson, Iddo Bentov, Yinon Horesh, Michael Riabzev. Scalable, Transparent, and Post-Quantum Secure Computational Integrity. Cryptology ePrint Archive, Paper 2018/046 (2018).

[^9]: Eli Ben-Sasson, Iddo Bentov, Yinon Horesh, Michael Riabzev. Fast Reed-Solomon Interactive Oracle Proofs of Proximity. ICALP 2018: 14:1–14:17.

[^10]: Lior Goldberg, Shahar Papini, Michael Riabzev. Cairo – A Turing-Complete STARK-Friendly CPU Architecture. Cryptology ePrint Archive, Paper 2021/1063 (2021).

[^11]: Miden. [Miden VM Documentation](https://docs.miden.xyz/reference/miden-vm/).

[^12]: Ariel Gabizon, Zachary J. Williamson, Oana Ciobotaru. PLONK: Permutations over Lagrange-Bases for Oecumenical Noninteractive Arguments of Knowledge. Cryptology ePrint Archive, Paper 2019/953 (2019).

[^13]: Ariel Gabizon, Zachary J. Williamson. Plookup: A Simplified Polynomial Protocol for Lookup Tables. Cryptology ePrint Archive, Paper 2020/315 (2020).

[^14]: Binyi Chen, Benedikt Bünz, Dan Boneh, Zhenfei Zhang. HyperPlonk: Plonk with Linear-Time Prover and High-Degree Custom Gates. EUROCRYPT 2023: 499–530.

[^15]: Srinath Setty, Justin Thaler, Riad S. Wahby. Customizable Constraint Systems for Succinct Arguments. Cryptology ePrint Archive, Paper 2023/552 (2023).

[^16]: Benjamin E. Diamond, Jim Posen. Succinct Arguments over Towers of Binary Fields. EUROCRYPT 2025: 93–122.
