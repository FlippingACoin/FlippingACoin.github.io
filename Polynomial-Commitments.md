# Polynomial Commitments

Many modern universal zk-SNARKs follow a construction paradigm that combines a polynomial interactive oracle proof (PIOP) with a polynomial commitment scheme. The construction typically first arithmetizes a circuit into a [constraint system](https://flippingacoin.github.io/constraint-systems), such as R1CS, a PLONKish constraint system, or AIR, and then uses a PIOP to efficiently prove the satisfiability of the constraint system through interaction. 

Using techniques such as the Schwartz–Zippel lemma and error-correcting codes, complex global algebraic relations can be reduced to checks at a small number of random points or locations, such that an invalid relation is detected with overwhelming probability. This is closely related to the local randomized verification paradigm embodied by the PCP theorem.[^1] 

Accordingly, in a PIOP, the prover sends polynomial oracles that can be randomly queried by the verifier; polynomial commitment schemes are then used to instantiate these oracles cryptographically. Like any commitment scheme, a polynomial commitment scheme must first satisfy the binding property, and hiding is also required in many settings. Unlike other commitments, a polynomial commitment scheme must bind the prover to a polynomial in a prescribed polynomial space and support opening proofs for claimed evaluations at points specified by the verifier.

## Definition and properties

We give an **informal** description of polynomial commitment schemes (PCS) below. 

A polynomial commitment scheme may consist of the following algorithms :

* $\mathsf{Setup}(1^\kappa,d)\rightarrow pp$:  generate public parameters for security parameter and maximum supported degree. 
* $\mathsf{Commit}(pp,f(X))\rightarrow C$: generate a commitment to the polynomial. 
* $\mathsf{Open}(pp,C,f(X))\rightarrow f$: open the polynomial directly. 
* $\mathsf{VerifyPoly}(pp,C,f(X))\rightarrow b$: Verify that $C$ is a valid commitment to the polynomial $f$, and output a bit $b\in\{0,1\}$.
* $\mathsf{Eval}(pp,C,f(X),z)\rightarrow(v,\pi)$: Compute the evaluation $v=f(z)$ and generate an evaluation proof $\pi$ for the point $z$.
* $\mathsf{VerifyEval}(pp,C,f(X),z,v,\pi)\rightarrow b$: Verify that the value committed by $C$ evaluates to $v$ at the point $z$, and output a bit $b\in\{0,1\}$.

A polynomial commitment may satisfy the following properties, some of which are optional depending on the application. 

* **Correctness**: the output of $\mathsf{Open}$ is successfully verified by $\mathsf{VerifyPoly}$, and the output of $\mathsf{Eval}$ is successfully verified by $\mathsf{VerifyEval}$ for all valid evaluation points. 
* **Polynomial Binding**: the probability that any efficient malicious prover outputs a commitment and two distinct polynomials, such that both openings are accepted by $\mathsf{VerifyPoly}$ is negligible. 
* **Evaluation Binding**: the probability that any efficient malicious prover can produce two distinct evaluations and proofs $(v,\pi)$ and $(v',\pi')$  that are both accepted by $\mathsf{VerifyEval}$ for the same commitment and evaluation point is negligible. 
* **Hiding**: the probability that any malicious verifier can infer any information about the polynomial underlying a commitment is negligible.  
* **Extractability**: for any efficient malicious prover, there exists an efficient extractor, given access to the malicious prover $\mathcal{A}$'s random tape, can extract a valid (which means consistent with the commitment and evaluation proof) polynomial from $\mathcal{A}$. 

Some works use another term, verifiable polynomial delegation (VPD), which can generally be viewed as a specialized PCS. Zero-knowledge verifiable polynomial delegation (zk-VPD) [^2] was further proposed to keep the evaluation $v$ private as well, by only revealing $\mathsf{Com}(v)$. 

## Taxonomy

Based on their commitment and opening mechanisms, polynomial commitment schemes can be broadly divided into the following three categories. (This is an informal, non-exhaustive taxonomy)

* [Quotient-Polynomial-Based PCS](): The evaluation claim $f(z)=v$ is reduced to the divisibility relation
  $$
  f(X)-v=(X-z)q(X)
  $$
  by constructing a quotient polynomial $q(X)$, and usually checked with pairing. Classical examples include KZG[^3], multivariate KZG[^4], vSQL[^5], Zeromorph[^6], etc.

* [Low-Degree-Test-Based PCS](): The polynomial is first represented by its evaluation vector over a sufficiently large domain, and then the vector is committed by vector commitment like Merkle tree. A low-degree test (e.g., FRI[^7]) is then used to prove that the committed vector is, or is close to, a Reed--Solomon codeword corresponding to a low-degree polynomial. Classical examples include FRI, DEEP-FRI[^8], polynomial commitments used in STARKs[^9], and BaseFold[^10], etc. 

* [Inner-Product-Based PCS](): Polynomial evaluation is expressed as the inner-product relation
  $$
  v=\left\langle
  (a_0,a_1,\ldots,a_d),
  (1,z,\ldots,z^d)
  \right\rangle.
  $$
  Classical examples include Halo[^11], which use recursive inner-product arguments (e.g., bulletproofs[^12]). 

  Brakedown[^13] observes that $v=\left<(a_0,a_1,\ldots,a_d),\mathbf{z_1}\otimes\mathbf{z_2}\right>=\mathbf{z_1}^\mathsf{T}M_\mathbf{a}\mathbf{z_2}$. 

  where $M_{\mathbf a}$ is the matrix obtained by reshaping the coefficient vector $\mathbf a$. It then uses linear error-correcting codes to prove the corresponding product relations. 

🚧To-do: performance comparison

## References

[^1]: S. Arora and S. Safra. Probabilistic checking of proofs: A new characterization of NP. *J. ACM*, 45(1):70–122, Jan. 1998. Prelim version FOCS ’92.
[^2]: Y. Zhang, D. Genkin, J. Katz, D. Papadopoulos, and C. Papamanthou. A zero-knowledge version of vSQL. *IACR Cryptol. ePrint Arch.*, Report 2017/1146, 2017.
[^3]: A. Kate, G. M. Zaverucha, and I. Goldberg. Constant-size commitments to polynomials and their applications. In *Advances in Cryptology—ASIACRYPT 2010*, pp. 177–194, 2010.
[^4]: C. Papamanthou, E. Shi, and R. Tamassia. Signatures of correct computation. In *Theory of Cryptography—TCC 2013*, pp. 222–242, 2013.
[^5]: Y. Zhang, D. Genkin, J. Katz, D. Papadopoulos, and C. Papamanthou. vSQL: Verifying arbitrary SQL queries over dynamic outsourced databases. In *2017 IEEE Symposium on Security and Privacy*, pp. 863–880, 2017.
[^6]: T. Kohrita and P. Towa. Zeromorph: Zero-knowledge multilinear-evaluation proofs from homomorphic univariate commitments. *J. Cryptol.*, 37(4):38, 2024.
[^7]: E. Ben-Sasson, I. Bentov, Y. Horesh, and M. Riabzev. Fast Reed–Solomon interactive oracle proofs of proximity. In *45th International Colloquium on Automata, Languages, and Programming—ICALP 2018*, pp. 14:1–14:17, 2018.
[^8]: E. Ben-Sasson, L. Goldberg, S. Kopparty, and S. Saraf. DEEP-FRI: Sampling outside the box improves soundness. In *11th Innovations in Theoretical Computer Science Conference—ITCS 2020*, pp. 5:1–5:32, 2020.
[^9]: E. Ben-Sasson, I. Bentov, Y. Horesh, and M. Riabzev. Scalable, transparent, and post-quantum secure computational integrity. *IACR Cryptol. ePrint Arch.*, Report 2018/046, 2018.
[^10]: H. Zeilberger, B. Chen, and B. Fisch. BaseFold: Efficient field-agnostic polynomial commitment schemes from foldable codes. In *Advances in Cryptology—CRYPTO 2024*, pp. 138–169, 2024.
[^11]: S. Bowe, J. Grigg, and D. Hopwood. Recursive proof composition without a trusted setup. *IACR Cryptol. ePrint Arch.*, Report 2019/1021, 2019.
[^12]: B. Bünz, J. Bootle, D. Boneh, A. Poelstra, P. Wuille, and G. Maxwell. Bulletproofs: Short proofs for confidential transactions and more. In *2018 IEEE Symposium on Security and Privacy*, pp. 315–334, 2018.
[^13]: A. Golovnev, J. Lee, S. Setty, J. Thaler, and R. S. Wahby. Brakedown: Linear-time and field-agnostic SNARKs for R1CS. In *Advances in Cryptology—CRYPTO 2023*, pp. 193–226, 2023.
