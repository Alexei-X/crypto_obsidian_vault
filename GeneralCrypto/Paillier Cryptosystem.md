# Paillier Cryptosystem
## Notations
We set $n = pq$ where $p$ and $q$ are large primes. We denote by $\phi(n) = (p-1)(q-1)$ **Euler's totient function**. 
We denote by $\lambda(n) = \mathrm{lcm}(p-1, q-1)$ **Carmichael's function** taken on $n$. 
Recall that $$
\lvert \mathbb{Z}_{n^2}^* \rvert = \phi(n^2) = n\phi(n)
$$And for any $w \in \mathbb{Z}_{n^2}^*$, $$
\begin{cases}
w^\lambda = 1 \bmod n \\ w^{n\lambda} = 1 \bmod n^2
\end{cases}
$$We denote by $\mathrm{RSA}[n, e]$ the problem of extracting $e$-th roots modulo $n$ where $n = pq$ is of unknown factorisation. 
## Deciding Composite Residuosity 
<b>Definition 1</b><br>A number $z$ is said to be a $n$-th residue modulo $n^2$ if $\exists y \in \mathbb{Z}_{n^2}^*$ such that $$
z = y^n \bmod n^2
$$
The set of $n$-th residues is a multiplicative subgroup of $\mathbb{Z}_{n^2}^*$ of order $\phi(n)$. 
Each $n$-th residue $z$ has exactly $n$ roots of degree $n$, among which exactly one is strictly smaller than $n$. 
The $n$-th roots of unity are the numbers of the form $(1+n)^x = 1+xn \bmod n^2$. 
The problem of deciding $n$-th residuosity is denoted $\mathrm{CR}[n]$. 
<b>Conjecture 2</b><i></i><br>There exists no polynomial time distinguisher for $n$-th residues modulo $n^2$. 
This hypothesis will be referred as the *Decisional Composite Residuosity Assumption* (DCRA). 
## Computing Composite Residuosity Classes
Let $g \in \mathbb{Z}_{n^2}^*$ and denote by $\mathcal{E}_g$ the function $$
\mathcal{E}_g : \mathbb{Z}_{n} \times \mathbb{Z}_{n}^* \to \mathbb{Z}_{n^2}^* : (x, y) \mapsto g^x \cdot y^n \bmod n^2
$$<b>Lemma 3</b> <i></i><br>If the order of $g$ is a non-zero multiple of $n$, then $\mathcal{E}_g$ is a bijection. 

## The Encryption Scheme
