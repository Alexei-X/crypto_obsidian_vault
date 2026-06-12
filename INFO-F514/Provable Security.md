<div class="latex-title-block">
<h1 class="title">Provable Security</h1>
<div class="author">Bataille Alex</div>
<div class="date">May 2026</div>
</div>

## Reductionist Approach & Hard Problems
How can we test the security of a scheme ? No matter what kind of experiments we run, the adversary might do something unexpected ! It's very hard, perhaps impossible to think of all the way someone could break a scheme from one way or another. In cryptography, the best we can do is : 
- Make precise definitions of security
- Argue that the protocol satisfies those definitions
$\implies$ Always ask yourself what is meant by "secure" !!
### Reductionist Approach
First we have to precisely define what it means to break the protocol, to do that, define : 
- Adversary's goal
- Adversary's resources
- Adversary's access to the system
Then, we can choose our favorite hard problem (A computational problem that cannot be solved efficiently), and build a protocol in such a way that we can prove that $$
\text{Breaking the protocol} \implies \text{Solving the hard problem}
$$But then what does "hard" mean ? 
We define the **asymptotic hardness** : no polynomial-time adversary can solve a given problem instance with a non-negligible probability
	$\rightarrow$ Choose a security parameter $\lambda \sim$ problem size
	$\rightarrow$ Adversary is an algorithm 
	$\rightarrow$ Adversary's complexity is a function of $\lambda$ 
	$\rightarrow$ Negligible $=$ smaller than $\frac{1}{p(\lambda)}$ for any polynomial $p$
We also define **concrete hardness** : cannot solve the problem today, even with the best computers available 
#### Integer Factorization
Every integer can be uniquely decomposed as a product of prime numbers.
Given the prime numbers $p_i$, it is of course easy to compute their product and check the predicate. 
But given $n = pq$ where $p, q$ are well-chosen primes, the best known algorithm requires $$
L_n\bigg(\frac{1}{3}, \sqrt[3]{\frac{64}{9}}\bigg) = exp\Bigg(\bigg(\sqrt[3]{\frac{64}{9}} + o(1)\bigg)(\log n)^{1/3}(\log \log n)^{2/3}\Bigg)
$$operations to factor $n$ !
#### Discrete Logarithm
Let $p$ be a prime, and $g$ a generator of $\mathbb{Z}_p^*$ . The function $$
f : [0, \dots, p-2] \rightarrow \mathbb{Z}_p : x \mapsto g^x \bmod p
$$is easy to compute using the **square-and-multiply** algorithm : 
```python
Let x = sum_{i = 0}^n x_i * 2^i
ap = a; c = a^{x_0}
for i = 1 to n do:
	ap = ap^2 mod p
	if x_i == 1 then
		c = c*ap mod p
return c
```
But computing the inverse function $f^{-1}$ is believed to be hard ! This is the **discrete logarithm problem**. 
The best algorithm for it today has a complexity of : $$
L_p\bigg(\frac{1}{3}, \sqrt[3]{\frac{64}{9}}\bigg) = exp\Bigg(\bigg(\sqrt[3]{\frac{64}{9}} + o(1)\bigg)(\log p)^{1/3}(\log \log p)^{2/3}\Bigg)
$$The discrete logarithm problem can be generalized to any **cyclic group**. The groups we use in cryptography are therefore : 
- Multiplicative groups of finite field, particularly $\mathbb{F}_p$ 
- Elliptic curve and hyperelliptic curve subgroups
We can get a complexity of $\mathcal{O}(\lvert G \rvert^{1/2})$ for well-chosen elliptic curves today
#### Polynomial Systems
Let $K$ be a finite field, let $R = K[x_1, \dots, x_n]$ be a polynomial ring over $K$, and let $f_i \in R$. 
Solving polynomial systems such as $$
\begin{cases}
f_1(x_1, \dots, x_n) = 0 \\
\vdots \\
f_m(x_1, \dots, x_n) = 0
\end{cases}
$$is a hard problem in general, in fact it is $\text{NP-hard}$. 
#### Lattice Problems
A lattice is a subgroup of $\mathbb{R}^n$.
![[Screenshot 2026-05-24 at 22.42.48.png|400]]
We distinguish 2 problems : 
- SVP (Shortest Vector Problem) : Given a lattice, compute its shortest vector
- CVP (Closest Vector Problem) : Given a lattice and an extra point, compute the lattice vector that is the closest to the point
$\rightarrow$ this is a very active research area
## ElGamal Encryption Protocol
### Security Definitions for PK Encryption
We define a public key encryption scheme to be made of 3 algorithms : 
- Key Generation algorithm : **KeyGen** 
	- Given security parameter $\lambda$
	- Return pair of private and public keys $(SK, PK)$ 
- Encryption algorithm : **Enc**
	- Given public key $PK$ and message $m$
	- Return ciphertext $c$
- Decryption algorithm : **Dec**
	- Given private key $SK$ and ciphertext $c$
	- Return message $m$
Note that **KeyGen**, **Enc** and **Dec** are probabilistic algorithms. 
$\rightarrow$ Keys implicitly define message and encryption space
$\rightarrow$ Algorithms must abort on wrong inputs 
#### Correctness
A public key encryption scheme must be correct : $$
\mathrm{Pr}\left[m' = m : \begin{array}{1}
(PK, SK) \leftarrow \mathrm{KeyGen}(\lambda) \\
c \leftarrow \mathrm{Enc}(PK, m) \\
m' \leftarrow \mathrm{Dec}(SK, c)
\end{array}\right] \approx 1
$$We talk of **perfect correctness** iff the probability is exactly $1$. 
#### Models of Security
To model the security, we use an interactive game between a "good guy" (the challenger) and a "bad guy" (the adversary). The scheme is then secure if the adversary cannot win the game except with a negligible advantage over trivial strategy. 
$\Rightarrow$ Most popular security notions : **IND-CPA**, **IND-CCA**, **IND-CCA2**
##### IND-CPA Security
This means **indistinguishability against chosen plaintext attacks**. 
The security game is : 
- Challenger runs $(PK, SK) \leftarrow \mathrm{KeyGen}(\lambda)$
- Challenger sends $PK$ to adversary
- Adversary chooses 2 messages $m_0, m_1$ 
- Adversary sends $m_0, m_1$ to challenger
- Challenger picks a random bit $b \leftarrow \{0, 1\}$ 
- Challenger sends $c = \mathrm{Enc}(PK, m_b)$ to adversary
- Adversary sends a bit $b'$ to challenger
- Adversary wins if $b' = b$
Then, we say that an encryption scheme is **IND-CPA** secure if $$
\lvert\mathrm{Pr}[\text{Adversary wins}] - \frac{1}{2}\rvert \approx 0
$$Note that no deterministic public key encryption scheme is IND-CPA secure ! Indeed, the adversary could just compute $$
c_i = \mathrm{Enc}(PK, m_i)
$$and compare with $c$ to always guess $b$ correctly. 
##### IND-CCA(2) Security
This means **indistinguishability against (adaptively) chosen ciphertext attacks**. 
In this, the adversary is given an additional power : **decryption oracle**
- Before sending $m_0, m_1$, adversary can call a decryption oracle on ciphertexts of its choice
- In the adaptive version, can also call the decryption oracle after receiving $c$ and before sending $b'$

We have that an attacker against IND-CPA game is also an attacker against IND-CCA game which is also an attacker against IND-CCA(2) game : $$
\text{IND-CCA2 Security} \Rightarrow \text{IND-CCA Security} \Rightarrow \text{IND-CPA Security}
$$The converse is not true ! 
### ElGamal Encryption Scheme
#### Recall
Let's recall the discrete logarithm problem : 
Let $G$ be a finite cyclic group of prime order $q$, and let $g$ be a generator. 
The **discrete logarithm problem** (DLP) is : given $G, g, h$, find $x$ such that $$
h = g^x
$$The **Diffie-Hellman problem** is : given $G, g, g^a, g^b$, compute $g^{ab}$
We can also define the **decisional** Diffie-Hellman problem : let $G, g, g^a, g^b, g^c$ where $c$ is either $ab \bmod q$ or random with a probability of $\frac{1}{2}$, guess the correct distribution with probability significantly bigger than $\frac{1}{2}$. 
We have that finding an efficient **DLP** algorithm implies finding an efficient algorithm for **DHP** which implies finding an efficient algorithm for **DDHP**. 
#### Key Agreement
Alice and Bob want to agree on a common secret key but they can only exchange public messages over insecure channels ! This means that Eve can see all messages exchanged, yet she should not be able to infer the secret key.
To do this, we can use the Diffie-Hellman key agreement : 
- Choose $g$ generating a cyclic group $G$
- Alice picks a random $a$ and sends $g^a$
- Bob picks a random $b$ and sends $g^b$ 
- Alice computes $(g^b)^a = g^{ab}$
- Bob computes $(g^a)^b = g^{ab}$ 
- But Eve cannot compute $a, b$ or $g^{ab}$ from $g^{a}, g^b$. 
#### ElGamal Encryption
**Key Generation** : 
- Generate cyclic group $G$ of prime order $q$
- Generate random $x\in [1, q]$
- Pick a generator $g$ and set $h = g^x$
- Private key is $x$ and the public key is $(G, g, h)$
**Encryption** : Given $PK = (G, g, h)$ and message $m$
- Pick random $r \in [1, q]$
- Return $c = (c_1, c_2) = (mh^r, g^r) = (mg^{xr}, g^r)$
**Decryption** : Given $SK = x$ and ciphertext $c$
- Return $m' = c_1c_2^{-x} = mg^{xr}g^{-xr} = m$
#### ElGamal Security
This scheme is perfectly correct. Computing the private key from the public key means solving the discrete logarithm problem, and finding an algorithm to decrypt messages implies finding an algorithm to solve DHP : 
- Let $G, g, g^a, g^b$ a given DHP instance
- Set $h = g^a, c_1 = 1, c_2 = g^b, c = (c_1, c_2)$
- Ask decryption algorithm for $m = \mathrm{Dec}(c, SK)$ 
- Return $g^{ab} = m^{-1}$ 
- Conversely, let $m = c_1(g^{ab})^{-1}$ where $g^{ab}$ is a solution to the DHP instance $G, g, h, c_2$
Since **DDHP** is **hard**, ElGamal is **IND-CPA** secure
<i>Proof. </i>
We use and IND-CPA adversary to build a DDHP solver. 
- Let $G, g, g^a, g^b, g^c$ be a given DDHP instance
- Use $h = g^a$ as public key
- Receive $m_1, m_2$ from adversary
- Choose random bit $b$
- Send $c = (m_bg^c, g^b)$ to adversary
- Receive the guess $b'$ from adversary
- Guess "$c = ab$" if $b = b'$
Suppose the IND-CPA adversary wins with probability $\frac{1}{2} + \epsilon$. 
- When $c = ab$, IND-CPA adversary receives inputs with expected distribution, so $b = b'$ with probability $\frac{1}{2}+\epsilon$
- When $c$ is random, his inputs are random and independent of $b$, so $b = b'$ occurs with probability $\frac{1}{2}$
- As both cases are as likely to occur, we have $b = b'$ with probability $\frac{1}{2} + \frac{\epsilon}{2}$
$$\tag*{$\blacksquare$}$$
ElGamal is **not** IND-CCA secure. Indeed, given $(c_1, c_2) = (m_bh^r, g^r)$, use the decryption oracle on valid and different ciphertext $(c_1h, c_2g)$ to get $m$. 
### Limitations to Provable Security Approach
How could this go wrong ? 
- Find out that the security proof is wrong
- Solve underlying hard problem
- Find weaknesses not covered by the proof
#### Subgroup Attacks
Imagine the following naive implementation of the Diffie-Hellman key agreement : 
```python
p = random_prime(2**1024)
g = 2
a = random_integer(2**1024)
h = modexp(g, a, p)
```
Is this implementation secure ? 
Let $N|p-1$ be the order of $g$, and $q$ a small divisor of $N$. Then $(g^{N/q})$ has order $q$ and $(g^{N/q})^a = (h^{N/q})$. 
If $q$ is small, we can recover $a \bmod q$. 
From $a \bmod q_i$ for every $q_i|N$ , recover $a \bmod N$ using Chinese Remainder Theorem. 
If $N$ is smooth, then DLP is easy !
For random $p$, factors of $p - 1$ likely to contain very small primes, some medium size primes, and one large prime. This implies that at least some information about $a$ can be recovered. 
We consider safe primes to be primes such that $\frac{p-1}{2}$ is prime. 
#### Weak Randomness
Suppose Alice uses RSA private key $(p, q_a)$ and Bob uses RSA private key $(p, q_b)$. Is it safe ? 
Everybody sees $n_a := pq_a$ and $n_b = pq_b$. 
Alice can compute $q_b = \frac{n_b}{p}$ and Bob can compute $q_a = \frac{n_a}{p}$. 
Anyone can compute $\mathrm{gcd}(n_a, n_b) = p$ and then $q_a$ and $q_b$ ! 
##### RSA with Small Decryption Key
Using a small decryption of RSA is appealing for efficiency reasons, moreover : 
- If $d$ has 80 bits then exhaustive search not possible
- If $n = pq$ is large enough then factoring is not possible
However, we then have $$
de = k\varphi(N) + 1 = k(N -z) + 1
$$where $z = \mathcal{O}(\sqrt{N})$ and $d, k$ are small. 
This is the "Small root problem", which has been solved using lattice reduction, following Coppersmith's methods. 
##### ECDSA (Elliptic Curve Digital Signature Algorithm)
= Public key signature standard
Its parameters are defined by : 
- $H$ a hash function
- $K$ a finite field
- $q$ a prime
- $E$ an elliptic curve over $K$ with $qh$ points, $h \leq 4$
- $P$ a point of order $q$ on $E$
**Key generation** : 
- Choose secret key $x$ randomly in $\{1, \dots, q-1\}$
- Set public key $Q = xP$
Let $f : E \to \mathbb{Z}_q : P = (x, y) \mapsto x \bmod q$ where $x$ is some well-defined integer representation of the $x$-coordinate of $P$. 
To **sign** a message $m$ : 
- Choose $k$ randomly in $\{1, \dots, q-1\}$
- Let $T = kP$
- Let $r = f(T)$. If $r = 0$ start again. 
- Let $e = H(m)$
- Let $s = \frac{e+xr}{k}\bmod q$. If $s = 0$ start again. 
- Return $(r, s)$. 
To **verify** a signature $(r, s)$ on a message $m$ : 
- Reject if $r, s \notin \{1, \dots, q-1\}$
- Let $e = H(m)$
- Let $u_1 = \frac{e}{s}\bmod q$ and $u_2 = \frac{r}{s} \bmod q$. 
- Let $T = u_1P + u_2Q$. 
- Accept iff $r = f(T)$.
**Correctness** : $$
u_1 + xu_2 = \frac{e + rx}{s} = k \bmod q
$$Hence $u_1P+u_2Q = T$.

Security : 
- Existential unforgeability : if we replace the hash function by a random function and the group by a generic group, and suppose $f$ is "almost invertible". 
- Essential that $k$ does not repeat as otherwise 2 signatures $(r, s)$ and $(r', s')$ give $$
x = \frac{se' - s'e}{r(s'-s)} \bmod q
$$
- There are more attacks if some bits of $k$ leak or repeat. 
#### Strong Adversaries
##### Side-channel Attacks
So far we assumed that the attacker had access to some public data and some oracles, and was trying to deduce private data using mathematical algorithms. But in practice, the attacker may have access to side-channels such as power consumption, etc. 
This attacks are passive attacks because the attacker only observes the computing device. 
##### Fault Attacks
Fault attacks, on the other hand modify the computation result ! 
$\to$ physically achieved by changing the ground power level, flashing with a laser light, etc
This can be useful for : 
- Work in another group of smooth order
- Remove some access control condition
- etc
