# Homomorphic Encryption

## Definition and properties
Let $\oplus$ be an operation on plaintexts and $\otimes$ be an operation on ciphertexts. 
<b>Definition 1</b><br>The encryption scheme is homomorphic with respect to those operations if $$
\mathrm{Enc}(m_1 \oplus m_2) = \mathrm{Enc}(m_1) \otimes \mathrm{Enc}(m_2)
$$<i>Property 1</i><br>If an encryption scheme is homomorphic, then it cannot be INC-CCA2 secure. 
<i>Proof. </i>
We do the proof in the additive case, but it is general to other cases. 
- The adversary sends arbitrary messages $m_0, m_1$
- Adversary receives $c_b = \mathrm{Enc}(m_b)$ for a random bit $b \in \{0, 1\}$ 
- Adversary computes $c_b' = c_b \otimes \mathrm{Enc}(0)$ 
- Adversary asks the oracle to decipher $c_b'$ and get $m_b$ !
Indeed, doing $c_b' = c_b \otimes \mathrm{Enc}(0)$ is just re-randomizing the ciphertext, but the underlying plaintext is the same !$$
c_b' = \mathrm{Enc}(m_b) \otimes \mathrm{Enc}(0) = \mathrm{Enc}(m_b \oplus 0) = \mathrm{Enc}(m_b)
$$$$\tag*{$\blacksquare$}$$
## Multiplicatively homomorphic cryptosystems
### RSA Encryption
Let $p, q$ be two primes and $n = pq$. Let $\mathbb{Z}_n = \mathbb{Z}/n\mathbb{Z}$ be the ring of integers modulo $n$. 
Let $\varphi(n) = (p-1)(q-1) = \lvert \mathbb{Z}_n^*\rvert$ be the Euler function. 
<b>Theorem 1. (Euler's theorem)</b><i></i><br>$\forall x \in \mathbb{Z}_n^*$, we have $$
x^{\varphi(n)} = 1 \bmod n
$$The RSA encryption works with the following 3 algorithms : 
**Key generation** : 
- Generate two strong primes $p, q$ of same size (size given by security parameter)
- Compute $n = pq$
- Generate random $e \in [1, \varphi(n)]$ with $\gcd(e, \varphi(n)) = 1$
- Compute $d$ such that $ed = 1 \bmod \varphi(n)$ 
- Return private key $(p, q, d)$ and public key $(n, e)$
**Encryption** : 
- Given message $m$ and public key $(n, e)$, return $$
c = m^e \bmod n
$$
**Decryption** : 
- Given ciphertext $c$ and private key $(p, q, d)$, return $$
m' = c^d \bmod n
$$
The security of RSA encryption is the following : $$
\text{Computing SK from PK} \iff \text{Factoring }n
$$Decrypting a ciphertext without the private key requires computing $e$-th roots modulo $n$, a problem called the *RSA problem*. 
Textbook RSA is deterministic $\Rightarrow$ not IND-CPA $\Rightarrow$ not IND-CCA $\Rightarrow$ not IND-CCA2
<i>Property 2</i><br>RSA is multiplicatively homomorphic.
<i>Proof. </i>
Let $c_1 = m_1^e \bmod n$ be the encryption of $m_1$ and $c_2 = m_2^e \bmod n$ be the encryption of $m_2$. Then, $c_1c_2 \bmod n$ is an encryption of $m_1m_2$ ! Indeed, $$
c_1c_2 = (m_1^e m_2^e)\bmod n = (m_1m_2)^e \bmod n = \mathrm{Enc}(m_1 \cdot m_2)
$$
 $$\tag*{$\blacksquare$}$$
### ElGamal Encryption
<b>Definition </b><br>cf. [[EVoting#ElGamal Encryption]]. 
<i>Property 3</i><br>ElGamal encryption scheme is multiplicatively homomorphic. 
<i>Proof. </i>
cf. [[EVoting#Homomorphic encryption#Properties]].
 $$\tag*{$\blacksquare$}$$
## Additively homomorphic cryptosystems
### Exponential ElGamal
<b>Definition </b><br>cf. [[EVoting#Exponential ElGamal]].
<i>Property 4</i><br>Exponential ElGamal is additively homomorphic. 
<i>Proof. </i>
cf. [[EVoting#Exponential ElGamal]], property 2. 
 $$\tag*{$\blacksquare$}$$
### Paillier Cryptosystem
#### Key Generation
Let $p, q$ be large primes and $n = pq$ (as in RSA).
Consider the group $\mathbb{Z}^*_{n^2}$ and its $n$-th roots of unity : 
- Given by $x_k = (1+n)^k = 1 + kn \bmod n^2, \forall k$ 
$$
(1+n)^k \bmod n^2 = 1 + \begin{pmatrix}
k \\ 1
\end{pmatrix}n + \underset{\bmod n^2 = 0}{\underbrace{\begin{pmatrix}
k \\ 2
\end{pmatrix}n^2 + \begin{pmatrix}
k \\ 3
\end{pmatrix}n^3 + \cdots}} = 1+kn \bmod n^2
$$

- Let $L : x_k \mapsto k = \frac{x_k - 1}{n}$ 
- DLP is easy in the subgroup $$
L(x_k^m) = L((1+n)^{km}) = km = mL(x_k)
$$Indeed, $$
\begin{align*}
L((1+n)^{km}) &= \frac{(1+n)^{km} - 1}{n}
\\ &= \frac{1+nkm - 1}{n} = km \\ &= mL(x_k)
\end{align*}
$$
Let $\lambda = \lambda(n) = \mathrm{lcm}(p-1, q-1)$ : 
- For any $g \in \mathbb{Z}^*_{n^2}$, the element $g^\lambda$ is a $n$-th root of unity
- Let $g \in \mathbb{Z}_{n^2}^*$, random with order divisible by $n$
- Let $\mu = (L(g^\lambda \bmod n^2))^{-1} \bmod n$
The public key is $(n, g)$ and the secret key is $(\lambda, \mu)$. 
#### Encryption and Decryption
**Encryption** : 
- Let $0 \leq m < n$ be a message to be encrypted
- Let $r \in \mathbb{Z}_n^*$
- The ciphertext is $c = g^mr^n \bmod n^2$ 
**Decryption** : 
- Let $m = \mu \cdot L(c^\lambda \bmod n^2)\bmod n$ . Indeed, we have $$
c^\lambda = \underset{\text{easy instance of DLP}}{\underbrace{(g^\lambda)^m}} \underset{\text{ } = 1 \bmod n^2}{\underbrace{r^{\lambda n}}}
$$
Intuitions : 
- DLP easy for $n$-th roots of unity (with function $L$)
- Powering $g^m$ by $\lambda$ gives you $n$-th root of unity
- Multiplication by $r^n$ brings in some randomness, disappears when powering by $\lambda$ 
There is one big advantage compared to EEG, there is no bound !
This cryptosystem has correctness : 
- $\exists k : g^\lambda = 1 + kn \bmod n^2$ 
- $c^\lambda = g^{m\lambda}r^{\lambda n} = g^{m\lambda} = (1+kn)^m = 1 + kmn \bmod n^2$ 
- $\mu L(c^\lambda) = \frac{L(c^\lambda)}{L(g^\lambda)} = \frac{km}{k} = m$ 
Paillier cryptosystem is IND-CPA secure if composite residuosity assumption holds :
- Given $z \in \mathbb{Z}_{n^2}^*$, it's hard to decide whether $\exists y \in \mathbb{Z}_{n^2}^*$ such that $z = y^n \bmod n^2$ . 
The Paillier cryptosystem is homomorphic : $$\begin{align*}
\mathrm{Enc}(m_1) \cdot \mathrm{Enc}(m_2) &= g^{m_1}r_1^n \cdot g^{m_2}r_2^n \\
&= g^{m_1 + m_2}(r_1r_2)^n \\ &= \mathrm{Enc}(m_1 + m_2)
\end{align*}
$$
## Somewhat Fully Homomorphic Encryption
$\Rightarrow$ Can do both homomorphic additions and multiplications but not an arbitrary number of them. 
### Pairing-based Construction
#### Recall Elliptic Curves 
For $y^2 = x^3 + Ax + B$, we have 
![[InfoF514_03_HomEncryption_0ad0184cbb427812267c7e4cf9d479b4.png|600]]
#### Recall Scalar Multiplication
For $k \in \mathbb{Z}$ and any $P \in E(K)$, define $$
[k](P) := \underset{k \text{ times}}{\underbrace{P + P + \dots + P}}
$$If $K$ is finite, then for any $P \in E(K)$, there is $m \in \mathbb{Z}$ such that $[m](P) = O$. 
#### Elliptic Curve Discrete Logarithm Problem (ECDLP)
Let $K$ be a finite field, let $E$ be an elliptic curve over $K$, let $P \in E(K)$ and let $Q \in \langle P \rangle = \{P, [2]P, [3]P, \dots, O\}$ . 
The problem is to find $k$ such that $Q = [k]P$. 
Typically, the order of $P$ is a large prime, at least 256 bits. 
Most DLP-based protocols have an ECDLP-based version. 
#### Cryptographic Pairings
<b>Definition </b><br>Pairings are non-degenerate, bilinear maps $$
e : G_1 \times G_2 \to G_3
$$where $G_i$ are all cyclic groups of the same order $n$. (we usually consider $G_1, G_2$ to be additive and $G_3$ to be multiplicative).

Bilinear : 
- $e(P_1 + Q_1, P_2) = e(P_1, P_2)e(Q_1, P_2)$
- $e(P_1, P_2+Q_2) = e(P_1,P_2)e(P_1,Q_2)$ 
Non-degenerate : $$
\forall P_1 \in G_1, P_1 \neq O : \exists P_2 \in G_2 \text{ s.t. }e(P_1, P_2) \neq 1
$$ (and vice-versa).

Pairing properties : 
- $e(jP, Q) = e(P, Q)^j = e(P, jQ) \quad \forall j \in \mathbb{N}$ 
- $e(P, O) = e(O, P) = 1$ 
- $e(-P, Q) = e(P, Q)^{-1} = e(P, -Q)$ 
- $e(jP, Q) = e(P, Q)^j = e(P, jQ) \quad \forall j \in \mathbb{Z}$ 
We say that the pairing is symmetric if $G_1 = G_2$. If it is symmetric, then $$
e(P_1, P_2) = e(k_1P, k_2P) = e(P, P)^{k_1k_2} = e(P_2, P_1)
$$Many well-chosen elliptic curves come with suitable (efficient) symmetric pairings. Indeed, all ECs have pairings, but not necessarily efficient. 
#### Boneh-Goh-Nissim Cryptosystem
Let $p, q$ be large primes and $n = pq$, as in RSA. 
Let $r = 4n\ell - 1$ be a prime, for small $\ell$. (to get it, iterate $\ell$ until you get a prime). 
Consider the curve $y^2 = x^3 + x$ over $\mathbb{F}_r$ : 
- Supersingular curve, order $\#E(\mathbb{F}_r) = r+1 = 4n\ell$, this means there are exactly $r+1$ points on the curve.
- Let $P$ be a point of order $n$ and let $G_1 = \langle P \rangle$. 
- Let $G_3 = \mathbb{F}_r^*$.
- There is a symmetric pairing $e : G_1 \times G_1 \to G_3$.
Let $Q' \in G_1$ and let $Q = qQ'$ of order $P$. 
The secret key is $q$, and the public key is $(n, P, Q)$. 
**Encryption** of $m$ :
- Choose $t$ randomly in $[1, n]$ 
- Let $C = mP + tQ$
**Decryption** of $C$ : 
- Compute $pC$ and $pP$. 
- Compute the discrete log of $pC$ with respect to $pP$. Indeed, we have that $$
pC = mpP + \underset{ = tpqQ' = tnQ' = 0\bmod n}{\underbrace{tpQ}} = mpP
$$Note that the decryption is efficient iff $m$ only takes a few values, as in EEG. 
**Correctness** : $$
pC = mpP + tpQ = mpP + tpqQ' = mpP
$$
This cryptosystem is additively homomorphic : $$
\begin{align*}
\mathrm{Enc}(m_1) + \mathrm{Enc}(m_2) &= (m_1P + t_1Q) + (m_2P + t_2Q) \\
&= (m_1+m_2)P + (t_1+t_2)Q
\end{align*}
$$We can also say that it is "multiplicatively" homomorphic, indeed : $$\begin{align*}
e(C_1, C_2) &= e(m_1P + t_1Q, m_2P + t_2Q) \\
&= e(P, P)^{m_1m_2}e(P, Q)^{m_1t_2 + m_2t_1}e(Q, Q)^{t_1t_2} \\
&= e(P, P)^{m_1m_2}e(P, Q')^{(m_1t_2 + m_2t_1)q}e(Q', Q')^{t_1t_2q^2}
\end{align*}
$$To decrypt after the homomorphic multiplication, do : 
- Compute discrete logarithm of $e(C_1, C_2)^p$ with respect to $e(P, P)^p$. 
- Indeed, $e(C_1, C_2)^p = e(P, P)^{pm_1m_2}$.
Notes :
- we can do this only once ! That's why the quotes :p and why we call it somewhat homomorphic (SHE). 
- The homomorphic multiplications change the ciphertext group from $G_1$ to $G_3$.
- We can perform any number of homomorphic additions, followed by a single homomorphic multiplication, followed by any number of homomorphic additions. 
- In other words, we can compute the encryption of any quadratic function of plaintexts (for example, the variance).
- The decryption is more expensive if the result is larger. 
### Lattice-based Construction
#### Symmetric Key Version
The secret key is large prime $p$. To encrypt a bit $m$, choose a random $r \ll p$ and a large $q$, then return $$
c = m+ 2r+pq
$$To decrypt $c$, compute $$
m' = (c \bmod p) \bmod 2
$$Correctness : $$
m' = ((m+2r+pq) \bmod p)\bmod 2 = (m+2r)\bmod2 = m
$$(as long as $m+2r < p$).
The security of this construction is related to **approximate gcd problem**. In other words, that it is hard to compute $p$ from several samples $pq_i + s_i$. 
##### Additively Homomorphic Properties
$$
\begin{align*}
\mathrm{Enc}(m_1) + \mathrm{Enc}(m_2) &= (m_1 + 2r_1 + pq_1) + (m_2 + 2r_2 + pq_2) \\
&= \underset{\text{as long as }<p}{\underbrace{(m_1 + m_2) + 2(r_1 + r_2)}} + p(q_1 + q_2)
\end{align*}
$$In a general way, we have $$
\mathrm{Enc}(\sum_{}^{} m_i) = \sum m_i + 2 \sum r_i + p\sum q_i
$$And this will decrypt as long as $\sum m_i + 2\sum r_i < p$. 
##### Multiplicatively Homomorphic Properties
$$\begin{align*}
\mathrm{Enc}(m_1) \cdot \mathrm{Enc}(m_2) &= (m_1 + 2r_1 + pq_1) \cdot (m_2 + 2r_2 + pq_2) \\
&= (m_1 \cdot m_2) + 2(m_1r_2 + r_1m_2 + 2r_1r_2) + p(\dots)
\end{align*}
$$In a general way, we have $$
\mathrm{Enc}(\prod m_i) = (\prod m_i) + 2R + pY
$$And this will decrypt as long as $(\prod m_i) + 2R < p$. 
##### Noise Growth
We see that every homomorphic operation makes the noise $r$ grow ! And this growth is much faster for multiplication than addition. 
When the noise is too big, ($2R > p$), the decryption algorithm may fail...
This still allows to homomorphically evaluate some functions on encrypted data (with few multiplications).
With clever tricks, this suffices in many applications !
#### Public Key Version
The public key has several encryptions of $0$ : $$
c_i = 2r_i + pq_i
$$The encryption of $m$ is $$
c = m + \sum_{i \in I}^{} c_i + 2r
$$For a random subset $I$. 
The decryption and homomorphic operations are as before. 
## Fully Homomorphic Encryption
This is the strongest notion of homomorphic encryption. When the scheme supports arbitrary computations on the encrypted data. This allows to do cool stuff such as statistics on encrypted data. 
### Key Ideas
- Encrypt your messages as noisy ring elements like in previous encryption schemes based on lattices
- This gives SFHE. Homomorphic additions and multiplications of ciphertexts, but not too many. 
- We could decrease the noise by decrypting and re-encrypting but that would reveal intermediary plaintexts.
- Solution : **Bootstrapping** 
	- Include encrypted version of somewhat homomorphic encryption decryption key in the public key, and regularly decrease noise with homomorphic decryption. 
	![[InfoF514_03_HomEncryption_0ad0184cbb427812267c7e4cf9d479b4 2.png]]
- Use SFHE to homomorphically evaluate encryption of $f_c(SK) = \mathrm{Dec}(c, SK)$ from $c$ and encryption of $SK$. In other words, homomorphically compute $$
\mathrm{Enc}((c \bmod p) \bmod 2) = \mathrm{Enc}(m)
$$from $c$ and $\mathrm{Enc}(p)$, where $\mathrm{Enc}$ is above SFHE scheme. 
- Suppose that SFHE noise small enough after evaluating $f_c(SK)$ + one additional multiplication homomorphically. 
- Then iterate this to evaluate any circuit homomorphically !
