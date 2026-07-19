# 近世代数

## 群

Def: 一个非空集合上定义一种二元运算，并且需要满足：1) 结合律 2) 存在单位元 3) 每个元素都存在逆元，则该集合对于该运算构成群.

单位元和逆元都唯一.

群包括有/无限群，有限群的大小记为阶$|G|$.

群中元素$a$的阶$o(a)$为满足$a^n=1$的最小正整数$n$，并且对于任何$m$使得$a^m=1$，则$n|m$. 并且元素$a^d$的阶为$n/(n,d)$. 

对于有限交换群，有两个性质：
如果$(o(a),o(b))=1$，则$o(ab)=o(a)o(b)$.
群中最大阶可以被任何元素的阶整除.

> [!IMPORTANT]
>
> 等价关系可以将集合分成互不相交的等价类，等价类中任意两个元素都属于等价关系。例如模3相等的两个数可以看作是“等价”的
>
> 群论的关键在于由子群可以定义等价关系，其中子群为对于群中运算可构成群的$G$的子集. 
>
> $(a,b)\in R_H$，如果$a=bh$对于某个$h\in H$，$R_H$是等价关系.
>
> 等价类$aH$称为H的左陪集，同样有右陪集.
>
> 例如，G=Z_{9}, H = {[0],[3],[6]}, [0]+H是一个等价类（其实也是一个剩余类，即模3余0），同理，$[1]+H,[2]+H$. [0],[1],[2]构成一个模3的剩余系
>
> 陪集的个数称为H在G中的指数.
>
> Lagrange定理：对于有限群，$|G|=|H|\cdot$指数. 因此，任何子群的阶整除G的阶. 

> [!IMPORTANT]
>
> Def: 令G是一个群，H是G的一个子群，记G的（左）陪集的集合为$G/H=\{gH:g\in G\}$.
>
> Def: 令G是一个群，H是G的子群，g是G中一个元素，那么H的g-conjugate（共轭）是子群$g^{-1}Hg$. 
>
> Def: H是G的一个normal subgroup（正规子群），如果对于G中任意元素g，都有$g^{-1}Hg=H$. 
>
> 交换群的每一个子群都是正规子群. 
>
> 令G是一个群，H是G的正规子群，那么G/H构成一个群，称作商群(quotient group). 

## 环

Def: 一个非空集合上定义两种运算（一般称为加法和乘法），并且满足：1) 集合关于加法构成交换群，其中该群的单位元记作$0_R$. 2) 集合关于乘法几乎构成一个群，但是其中元素不一定有逆元，其中单位元记作$1_R$. 3) 满足分配律，即对于环中元素$a,b,c$，有$a\cdot (b+c)=a\cdot b+a\cdot c$. 

$0_R\cdot a=0_R,\forall a\in R.$ 

Def: 令R和R'都是环，从R到R'的环同态（ring homomorphism）$\phi: R\rightarrow R'$满足:
$\phi(1_R)=1_{R'}$,
$\phi(a+b)=\phi(a)+\phi(b)$,
$\phi(a\cdot b)=\phi(a)\cdot\phi(b)$,
ker($\phi$)$=\{a\in R: \phi(a)=0_{R'}\}$称为$\phi$的核(kernel). 

Def: 当$R$到$R'$有一个同态双射时，称二者是同构(isomorphic)的. 

对于每一个环$R$，都有一个唯一的从$\mathbb{Z}$到$R$的映射. 

Def: 令R是一个交换环，$a\in R$被称作零因子(zero divisor)若$a\neq0$且存在一个非零的$b\in R$使得$ab=0$. 

Def: 若环R没有零因子，则R是一个整环(integral domain). 

域一定是整环. （证明：若域中有零因子$a$，则有非零$b\in R$使得$ab=0$，而域中所有非零因子都有逆元，因此有$a^{-1}ab=a^{-1}0=0$，即$b=0$，矛盾. ）

Def: 令R是交换环，R的单位群（group of units of R，R中单位（R中的可逆元）组成的群）是R的子群$R^*=\{a\in R: \exists b\in R, s.t.,ab=1\}$. 

R是一个域，当且仅当$R^*=R\backslash\{0\}$. 

令$R_1,...,R_n$是交换环，则$(R_1\times\cdots\times R_n)^*\cong R_1^*\times\cdots\times R_n^*$. 

Def: 令R是交换环，R的理想(ideal)是非空子集$I\sube R$，且满足：
若$a\in I,b\in I$，则$a+b\in I$.
若$a\in I,r\in R$，则$ra\in I$.

Def: 交换环R中元素$c$生成的主理想$cR=(c)=\{rc:c\in R\}$. 更一般地，对于任意有限个元素$c_1,...,c_n\in R$，由$c_1,...,c_n$生成的理想为$(c_1,...,c_n)=\{r_1c_1+r_2c_2+\cdots+r_nc_n:r_1,...,r_n\in R\}$. 

【例子】（消失理想）：令$\mathbb{F}[X_1,...,X_n]$是一个多项式环，$\mathbf{z}=(z_1,...,z_n)\in\mathbb{F}^n$，那么单点集$\{\mathbf{z}\}$的消失理想为$\{f\in\mathbb{F}[X_1,...,X_n]:f(\mathbf{z})=0\}=(x_1-z_1,...,x_n-z_n)$. 

每个环都有至少两个理想，零理想$(0)=\{0\}$和单位理想$(1)=R$. 

Def: 令I是R的理想，那么对于所有$a\in R$，$a$的陪集是$a+I=\{a+c:c\in I\}$. 若$a,b\in R$满足$a-b\in I$，那么$a+I=b+I, a\equiv b\mod I$. 定义运算$(a+I)+(b+I)=(a+b)+I, (a+I)\cdot(b+I)=(a\cdot b)+I$，所有不同陪集的集合为$R/I$，R/I构成一个交换环，称作商环(quotient ring). 

令R是一个交换环，I是R的一个理想，那么
$R\rightarrow R/I, a\rightarrow a+I$
是一个环同态，且核为I. 

> [!TIP]
>
> 正规子群和理想有一种相似的对应. （从理解上）
>
> 正规子群是群的一个子结构，由正规子群可以定义一种等价关系（两个元素的差属于正规子群），而所有等价类构成商群. 即正规子群可以诱导出一个子群. 
>
> 理想是环的一个子结构，由理想可以定义一种等价关系（两个元素的差属于理想），而所有等价类构成商环. 即理想可以诱导出一个子环. 

Def: 回顾对于所有环R都有一个唯一的$\mathbb{Z}$到R的同态映射$\phi$，且有一个唯一的整数$m\geq0$使得ker$(\phi)=m\mathbb{Z}$，$m$称作R的特征(characteristic)，同时也是使得m个$1_R+...1_R=0_R$最小的m. （证明：）

Def: 令R是一个交换环，R的理想I是素理想如果$I\neq R$且若$ab\in I$，则$a\in I$或$b\in I$. 

Def: 令R是一个交换环，R的理想I是极大理想如果$I\neq R$且，若J是一个理想且$I\sube J\sube R$，那么$J=I$或$J=R$. 

令R是一个交换环，令I是一个理想且$I\neq R$，
I是一个素理想当且仅当R/I是一个整环. 
I是一个极大理想当且仅当R/I是一个域. 

### 多项式环

Def: R上的多项式环$R[x]=\{a_0+a_1x+...+a_dx^d\mbox{ of all possible degrees with coefficients }a_0,...a_d\in R\}$

$E_c:R[x]\rightarrow R, E_c(f)=f(c)$构成一个同态映射，映射的核是$R[x]$中能被$x-c$整除的多项式集合. 

Def: 不可约多项式



## Reference

[^1]: Joseph H. Silverman, *Abstract Algebra: An Integrated Approach*, Pure and Applied Undergraduate Texts, Vol. 55, American Mathematical Society, 2022.

