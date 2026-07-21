# Notes on Abstract Algebra

## Groups

**Definition**: A nonempty set equipped with a binary operation forms a group under that operation if it satisfies: 1) associativity, 2) the existence of an identity element, and 3) the existence of an inverse for every element.

The identity element is unique, and the inverse of every element is also unique.

Groups may be finite or infinite. The size of a finite group is called its order and is denoted by $\lvert G\rvert$.

If there exists a positive integer $n$ such that $a^n=1$, then the order of an element $a$, denoted by $o(a)$, is the smallest such positive integer $n$. For every integer $m$ satisfying $a^m=1$, we have $n\mid m$. Moreover, the order of $a^d$ is $\frac{n}{(n,d)}$. If no such positive integer exists, then $a$ is said to have infinite order.

For a finite abelian group, the following two properties hold:<br>
If $(o(a),o(b))=1$, then $o(ab)=o(a)o(b)$. <br>
The maximum order of an element in the group is divisible by the order of every element.

An equivalence relation partitions a set into pairwise disjoint equivalence classes. Any two elements in the same equivalence class are related by the equivalence relation. For example, two integers that are congruent modulo $3$ can be regarded as “equivalent.”

A key idea in group theory is that a subgroup can be used to define an equivalence relation. A subgroup is a subset of $G$ that itself forms a group under the operation of $G$.

$$(a,b)\in R_H$$, if $a=bh$ for some $h\in H$. Then $R_H$ is an equivalence relation.

The equivalence class $aH$ is called a left coset of $H$. Right cosets are defined similarly. 

[Example] let $G=\mathbb{Z}_9,H={[0],[3],[6]}$. Then $[0]+H$ is an equivalence class. In fact, it is also a residue class consisting of the elements congruent to $0$ modulo $3$. Similarly, the other equivalence classes are $[1]+H$ and $[2]+H$. The elements $[0],[1],[2]$ form a complete residue system modulo $3$.

The number of cosets is called the index of $H$ in $G$.

Lagrange's theorem: For a finite group, $\lvert G\rvert=\lvert H\rvert\cdot [G:H]$. Therefore, the order of every subgroup divides the order of $G$.

**Definition**: Let $G$ be a group and let $H$ be a subgroup of $G$. The set of left cosets of $H$ in $G$ is denoted by $G/H={gH:g\in G}$. 

**Definition**: Let $G$ be a group, let $H$ be a subgroup of $G$, and let $g$ be an element of $G$. The $g$-conjugate of $H$ is the subgroup $g^{-1}Hg$.

**Definition**: $H$ is a normal subgroup of $G$ ($H\trianglelefteq G$) if, for every element $g\in G$, $g^{-1}Hg=H$. 
(**intuition**: The purpose of defining normal subgroups is to ensure that the equivalence classes induced by $H$ form a group. In particular, for any $a,b\in G$, we would like to define $(aH)(bH)=(ab)H$. That is, for arbitrary (h_1,h_2\in H), we need $(ah_1)(bh_2)\in(ab)H$. Since $ab(b^{-1}h_1b)h_2\in (ab)H$, this requires $b^{-1}h_1b\in H$. Thus, $b^{-1}Hb\subseteq H$. Requiring this condition for every $b\in G$, and applying it also to $b^{-1}$, gives the reverse inclusion. Therefore, $b^{-1}Hb=H$, which is exactly the definition of a normal subgroup.)

Every subgroup of an abelian group (commutative group) is normal.

Let $G$ be a group and let $H$ be a normal subgroup of $G$. Then $G/H$ forms a group, called the quotient group.

## Rings

**Definition**: A nonempty set equipped with two operations, generally called addition and multiplication, is a commutative ring with identity if it satisfies: 1)The set forms an abelian group under addition, whose identity element is denoted by $0_R$. 2)Multiplication is associative and commutative and has an identity element denoted by $1_R$. Elements are not required to have multiplicative inverses. 3)Multiplication is distributive over addition. For all $a,b,c$ in the ring, $a\cdot(b+c)=a\cdot b+a\cdot c$ and $(a+b)\cdot c=a\cdot c+b\cdot c$. 

For every $a\in R$, $0_R\cdot a=0_R$.

**Definition**: Let $R$ and $R'$ be rings. A ring homomorphism $\phi:R\rightarrow R'$ satisfies:<br>
$\phi(1_R)=1_{R'}$, <br>
$\phi(a+b)=\phi(a)+\phi(b)$, <br>
$\phi(a\cdot b)=\phi(a)\cdot\phi(b)$. <br>
The set $\ker(\phi)=\lbrace a\in R:\phi(a)=0_{R'}\rbrace$ is called the kernel of $\phi$.

**Definition**: If there exists a bijective homomorphism from $R$ to $R'$, then $R$ and $R'$ are said to be isomorphic. 

For every ring $R$, there exists a unique ring homomorphism from $\mathbb{Z}$ to $R$. 

**Definition**: Let $R$ be a commutative ring. An element $a\in R$ is called a zero divisor if $a\neq0$ and there exists a nonzero element $b\in R$ such that $ab=0$. 

**Definition**: If a nonzero commutative ring $R$ has no zero divisors, then $R$ is an integral domain.

Every field is an integral domain. Proof: Suppose that a field contains a zero divisor $a$. Then there exists a nonzero element $b\in R$ such that $ab=0$. Since every nonzero element in a field has an inverse, we have $a^{-1}ab=a^{-1}0=0$, and hence $b=0$, which is a contradiction.

**Definition**: Let $R$ be a commutative ring. The group of units of $R$, consisting of all invertible elements of $R$, is the group under multiplication $R^*=\lbrace a\in R:\exists b\in R\text{ such that }ab=1\rbrace$. 

The ring $R$ is a field if and only if $R^*=R\backslash{0}$. 

Let $R_1,\ldots,R_n$ be commutative rings. Then $(R_1\times\cdots\times R_n)^\times\cong R_1^\times\times\cdots\times R_n^\times$.

**Definition**: Let $R$ be a commutative ring. An ideal of $R$ is a nonempty subset $I\subseteq R$ satisfying:<br>
If $a\in I$ and $b\in I$, then $a+b\in I$.<br>
If $a\in I$ and $r\in R$, then $ra\in I$. 

An ideal $I$ of $R$ is an additive subgroup of $(R,+)$. ($0,-1\in R$ and apply the second property. )

**Definition**: The principal ideal generated by an element $c$ of a commutative ring $R$ is $cR=(c)={rc:r\in R}$. More generally, for any finite collection of elements $c_1,\ldots,c_n\in R$, the ideal generated by $c_1,\ldots,c_n$ is $(c_1,\ldots,c_n)=\lbrace r_1c_1+r_2c_2+\cdots+r_nc_n:r_1,\ldots,r_n\in R\rbrace$.

\[Example\]: Let $\mathbb{F}[X_1,\ldots,X_n]$ be a polynomial ring and let $\mathbf{z}=(z_1,\ldots,z_n)\in\mathbb{F}^n$, then the vanishing ideal of the singleton set ${\mathbf{z}}$ is $\lbrace f\in\mathbb{F}[X_1,\ldots,X_n]:f(\mathbf{z})=0\rbrace=(X_1-z_1,\ldots,X_n-z_n)$.

Every nonzero ring has at least two distinct ideals: the zero ideal $(0)=\lbrace 0\rbrace$ and the unit ideal $(1)=R$.

**Definition**: Let $I$ be an ideal of $R$. For every $a\in R$, the coset of $a$ is $a+I=\lbrace a+c:c\in I\rbrace$. If $a,b\in R$ satisfy $a-b\in I$, then $a+I=b+I$, and we write $a\equiv b\mod I.$ Define the operations $(a+I)+(b+I)=(a+b)+I$ and $(a+I)\cdot(b+I)=(a\cdot b)+I$, the set of all distinct cosets is denoted by $R/I$. The set $R/I$ forms a commutative ring, called the quotient ring.

Let $R$ be a commutative ring and let $I$ be an ideal of $R$. Then <br>
$R\rightarrow R/I,\qquad a\rightarrow a+I$ <br>
is a ring homomorphism whose kernel is $I$.

> **TIP**
>
> There is an analogous relationship between normal subgroups and ideals.
>
> A normal subgroup is a substructure of a group. A normal subgroup defines an equivalence relation: two elements $a,b\in G$ are equivalent if $a^{-1}b\in H$. The resulting equivalence classes form a quotient group. Thus, a normal subgroup induces a quotient group.
>
> An ideal is a substructure of a ring. An ideal defines an equivalence relation: two elements are equivalent if their difference belongs to the ideal. The resulting equivalence classes form a quotient ring. Thus, an ideal induces a quotient ring.

**Definition**: Recall that, for every ring $R$, there exists a unique ring homomorphism $\phi:\mathbb{Z}\rightarrow R$. There exists a unique integer $m\geq0$ such that $\ker(\phi)=m\mathbb{Z}$. The integer $m$ is called the characteristic of $R$. If $m>0$, then it is also the smallest positive integer satisfying $\underbrace{1_R+\cdots+1_R}_{m\text{ times}}=0_R$. 

**Definition**: Let $R$ be a commutative ring. An ideal $I$ of $R$ is a prime ideal if $I\neq R$ and, whenever $ab\in I$, we have $a\in I$ or $b\in I$. 

**Definition**: Let $R$ be a commutative ring. An ideal $I$ of $R$ is a maximal ideal if $I\neq R$ and, whenever $J$ is an ideal satisfying $I\subseteq J\subseteq R,$ we have $J=I$ or $J=R$. 

Let $R$ be a commutative ring and let $I$ be an ideal such that $I\neq R$. <br>
The ideal $I$ is prime if and only if $R/I$ is an integral domain.<br>
The ideal $I$ is maximal if and only if $R/I$ is a field.

### Polynomial Rings

**Definition**: The polynomial ring over $R$ is
$$
R[X]=\lbrace a_0+a_1X+\cdots+a_dX^d:
d\geq0,\ a_0,\ldots,a_d\in R\rbrace.
$$

For $c\in R$, the evaluation map<br>
$E_c:R\rightarrow R,\qquad E_c(f)=f(c)$<br>
is a ring homomorphism. Its kernel is the set of polynomials in $R[X]$ that are divisible by $X-c$.

**Definition**: Irreducible polynomial...

## Reference

[^1]: Joseph H. Silverman, *Abstract Algebra: An Integrated Approach*, Pure and Applied Undergraduate Texts, Vol. 55, American Mathematical Society, 2022.
