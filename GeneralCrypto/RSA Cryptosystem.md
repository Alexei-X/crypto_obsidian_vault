# RSA Cryptosystem
## Key Generation
1. Choose 2 distinct large prime numbers $p$, $q$. These are kept secret
2. Compute $n = pq$ 
3. Compute $\lambda(n) = \mathrm{lcm}(\lambda(p), \lambda(q))$. Since $p, q$ are prime, $$
\begin{cases}
\lambda(p) = \varphi(p) = p-1 \\
\lambda(q) = \varphi(q) = q-1
\end{cases} \Rightarrow \lambda(n) = \mathrm{lcm}(p-1, q-1)
$$
4. Choose integer $e$ such that $1 < e < \lambda(n)$ and $\gcd(e, \lambda(n)) = 1$
5. Determine $d \equiv e^{-1} \bmod \lambda(n)$ 
6. Return $(PK, SK) = \big((n, e), d\big)$ 
## Encryption
$$
c = m^e \bmod n
$$
## Decryption
$$
m' = c^d = (m^e)^d \bmod n
$$
