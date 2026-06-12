## 1. Multivariate Polynomials
<b>Definition 1.1 </b><br>Let $\mathbb{F} = \mathbb{F}_q$ be a finite field with $q$ elements. We define the **ring of multivariate polynomials** in $n$ variables over $\mathbb{F}$ as $$
\mathbb{F}[x_1, \dots, x_n] = \left\{  \sum_{i=1}^{s} c_i t_{\mathbf{a}_i} \mid s \in \mathbb{N}, \mathbf{a}_i = (a_{i, 1}, \dots, a_{i, n}) \in \mathbb{N}^n_0\right\} 
$$We call $c_i \in \mathbb{F}$ a coefficient and $t_{\mathbf{a}_i} = x_1^{a_{i, 1}}x_2^{a_{i, 2}}\cdots x_n^{a_{i, n}}$ a monomial. 
The set of all monomials in $\mathbb{F}[x_1, \dots, x_n]$ is denoted by $T^n$. 

<b>Definition 1.2</b><br>For a polynomial $p = \sum_{i=1}^{s}c_i t_{\mathbf{a}_i} \in \mathbb{F}[x_1, \dots, x_n]$ , we define the **support** of $p$ as the set of all monomials appearing in $p$, i.e. $$
\mathrm{Supp}(p) = \left\{ t_{\mathbf{a}_i} \mid c_i \ne 0 \right\} 
$$For a system $\mathcal{P} = (p^{(1)}, \dots, p^{(m)})$ , the support of $\mathcal{P}$ is defined as $$
\mathrm{Supp}(\mathcal{P}) = \bigcup_{i = 1}^m \mathrm{Supp}(p^{(i)})
$$
<b>Definition 1.3</b><br>The **degree** of a monomial $t_\mathbf{a} = x_1^{a_1}x_2^{a_2}\cdots x_n^{a_n} \in \mathbb{F}[x_1, \dots, x_n]$ is defined as $$
\mathrm{deg}(t_\mathbf{a}) = \lvert \mathbf{a} \rvert = \sum_{j=1}^{n}a_j 
$$The degree of a polynomial $p = \sum_{i=1}^{s}c_i t_{\mathbf{a}_i}$ is defined as $$
\mathrm{deg}(p) = \max_{i \in \left\{ 1, \dots, s \right\} } \mathrm{deg}(t_{\mathbf{a}_i}) = \max_{i \in \left\{ 1, \dots, s \right\} } \lvert \mathbf{a}_i\rvert
$$
<b>Definition 1.4</b><br>For a monomial $t_a = x_1^{a_1}\cdots x_n^{a_n} \in \mathbb{F}[x_1, \dots, x_n]$, we define $$
\log(t_a) = (a_1, \dots, a_n) \in \mathbb{N}_0^n
$$
<b>Definition 1.5</b><br>An **order of monomials** is a complete relationship $\sigma \subset T^n \times T^n$ on $T^n$ . Instead of $(t_1, t_2)\in \sigma$ we write $t_1 >_\sigma t_2$ . An order of monomials is called **admissible**, if $\forall t_1, t_2, t_3 \in T^n$ we have : 
1. $t_1 >_\sigma 1$
2. $t_1 >_\sigma t_2 \Rightarrow t_1t_3 >_\sigma t_2t_3$

<b>Definition 1.6</b><br>For a multivariate polynomial $$
p(x_1, \dots, x_n) = \sum_{i=1}^{s} c_i t_{a_i} \in \mathbb{F}[x_1, \dots, x_n]
$$with $t_{a_1} >_\sigma t_{a_2} >_\sigma \dots >_\sigma t_{a_s}$ we define the coefficient vector $\Pi(p)$ by $$
\Pi(p) = (c_1, \dots, c_s) \in \mathbb{F}^s
$$
<b>Definition 1.7</b><br>For a system $\mathcal{P}$ of $m$ multivariate polynomials $$
\begin{cases}
p^{(1)}(x_1, \dots, x_n) = \sum_{i=1}^{s} c_i^{(1)}t_{a_i} \\
p^{(2)}(x_1, \dots, x_n) = \sum_{i=1}^{s} c_i^{(2)}t_{a_i} \\
\vdots \\
p^{(m)}(x_1, \dots, x_n) = \sum_{i=1}^{s} c_i^{(m)}t_{a_i} \\ 
\end{cases}
$$ with $t_{a_1} >_\sigma t_{a_2} >_\sigma \dots >_\sigma t_{a_s}$, we define the **Macaulay matrix** of $\mathcal{P}$ by $$
\mathbf{M}_\mathcal{P} = (\Pi(p^{(1)}), \dots, \Pi(p^{(m)}))^T = \begin{pmatrix}
c_1^{(1)} & c_2^{(1)} & \cdots & c_s^{(1)} \\
c_1^{(2)} & c_2^{(2)} & \cdots & c_s^{(2)} \\
\vdots & & & \vdots \\
c_1^{(m)} & c_2^{(m)} & \cdots & c_s^{(m)}
\end{pmatrix}
$$We get $$
\mathcal{P}(x_1, \dots, x_n) = \mathbf{M}_\mathcal{P}\cdot (t_{a_1}, \dots, t_{a_s})^T
$$
In multivariate cryptography, we mainly deal with systems of multivariate quadratic polynomials. The following shows such a system $\mathcal{P} = (p^{(1)}, \dots, p^{(m)})$ : $$
\begin{cases}
p^{(1)}(x_1, \dots, x_n) = \sum_{i=1}^{n} \sum_{j=i}^{n} p_{ij}^{(1)}x_ix_j + \sum_{i=1}^{n} p_i^{(1)}x_i + p_0^{(1)} \\
p^{(2)}(x_1, \dots, x_n) = \sum_{i=1}^{n} \sum_{j=i}^{n} p_{ij}^{(2)}x_ix_j + \sum_{i=1}^{n} p_i^{(2)}x_i + p_0^{2)} \\
\vdots \\
p^{(m)}(x_1, \dots, x_n) = \sum_{i=1}^{n} \sum_{j=i}^{n} p_{ij}^{(m)}x_ix_j + \sum_{i=1}^{n} p_i^{(m)}x_i + p_0^{(m)}
\end{cases}
$$If $m = n$, the system is said to be *determined*, if $m < n$, it's said to be *underdetermined* and if $m > n$ it's *overdetermined*. 
### 1.1 Matrix Representation
For a multivariate quadratic system $\mathcal{P}$ as shown above, we can write each component $p^{(k)}$ as a matrix-vector product using an upper triangular $(n+1)\times (n+1)$ matrix $MP^{(k)}$ of the form : $$

$$
