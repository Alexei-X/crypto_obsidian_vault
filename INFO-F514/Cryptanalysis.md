<div class="latex-title-block">
<h1 class="title">Cryptanalysis</h1>
<div class="author">Alex Bataille</div>
<div class="date">2026</div>
</div>
## The Birthday Paradox
Suppose there are $N_2$ people in a room, what is the probability that 2 people have the same birthday ? 
How many people needed to have a probability larger than 50% ? $\Rightarrow 23$ people $$
\mathbb{P}[\text{all distinct}] = 1 \cdot \frac{364}{365} \cdot \frac{363}{365}\cdots \frac{365-22}{365} < \frac{1}{2}
$$More generally, suppose you choose $N_2$ elements randomly in a set of $N$ elements, what is the probability that 2 elements are equal ? $\Rightarrow \mathcal{O}(\sqrt{N})$ 
<i>Proof. </i>
$$ \begin{align*}
\mathbb{P}[\text{All distinct}] &= 1 \cdot \frac{N-1}{N} \cdot \frac{N-2}{N} \cdots \frac{N-N_2 + 1}{N} \\
&\approx e^{-\frac{1}{N}} \cdot e^{-\frac{2}{N}} \cdots e^{-\frac{N_2 - 1}{N}} \\
&\approx e^{-\frac{N_2(N_2 - 1)}{N}}
\end{align*}$$Taking $N_2 \approx \sqrt{N}$ ensures $1 - \mathbb{P}[\text{all distinct}] \in \Omega(1)$
$$\tag*{$\blacksquare$}$$
### Computing Collisions
Reminder : A hash function $H : \{0, 1\}^* \to \{0, 1\}^n$ is collision resistant if it's hard to find $(m, m')$ with $H(m) = H(m')$ but $m \neq m'$. 
A simple algorithm would be : 
- Choose random messages $m_i$ and compute $h_i = H(m_i)$ until you get a collision. 
By the BD paradox, you "only" need $\mathcal{O}(2^{n/2})$ elements to get a collision !
## Solving DLPs
### Discrete Logarithm Problem
<b>Definition DLP</b><br>Given a finite cyclic group $G$ of prime order $q$, a generator $g$ and a second element $h$, find $k$ such that $h = g^k$. 

The hardness of this problem depends on the group. 
### Algorithms
Pollard's rho, Baby-Step-Giant-Step algorithm work for any group, with complexity $\mathcal{O}(\sqrt{\lvert G\rvert})$. 
#### Exhaustive Search
Let $h = g^k$. We want to compute $k$. 
![[Capture d'écran 2026-06-12 152619.png]]
Time complexity : $\lvert G \rvert$ in the worst case, $\lvert G \rvert / 2$ on average. 
#### Baby Step, Giant Step (BSGS)
Let $h = g^k$. We want to compute $k$. Let $N' = \lceil \sqrt{\lvert G\rvert}\rceil$. 
There exists $0 \leq i, j < N'$ such that $k = jN' + i$ : $$
h = g^{jN' + i} \Leftrightarrow hg^{-jN'} = g^i
$$Compute $$
\begin{cases}
L_B := \left\{ g^i \mid i = 0, \dots, N'-1 \right\} \\
L_G := \left\{ hg^{-jN'} \mid j = 0, \dots, N' - 1\right\}  
\end{cases}
$$Find an element in $L_G \cap L_B$. 
This attack requires time and memory $\mathcal{O}(\sqrt{\lvert G \rvert})$ !
#### Pollard's rho (iterative function)
Define $G_1, G_2, G_3$ of about the same size such that $G = G_1 \cup G_2 \cup G_3$ and $G_i \cap G_j = \emptyset$. 
Over $\mathbb{Z}_p^*$, can choose $$
\begin{cases}
G_1 = \left\{ 0, \dots, \left\lfloor  \frac{p}{3} \right\rfloor \right\} \\
G_2 = \left\{ \left\lfloor  \frac{p}{3} \right\rfloor + 1, \dots , \left\lfloor  \frac{2p}{3} \right\rfloor\right\}   \\
G_3 = \left\{ \lfloor \frac{2p}{3}\rfloor + 1, \dots, p-2 \right\} 
\end{cases}
$$Define a function $f : G \to G$ such that $$
\begin{cases}
f(z) = zg \quad z \in G_1 \\
f(z) = z^2 \quad z \in G_2 \\
f(z) = zh \quad z \in G_3
\end{cases}
$$Steps : 
- Start from $g_0 := g$ and apply $f$ recursively to get $g_i$
- If $f$ is "random enough", we obtain random elements in $G$ and a collision after $\mathcal{O}(\sqrt{\lvert G \rvert})$ elements. 
- By the way $f$ is defined, we can keep track of $a_i, b_i$ such that $g_i = g^{a_i}h^{b_i}$ 
- Collision gives DLP solution
![[Capture d'écran 2026-06-12 153834.png|500]]
Pseudo-algorithm : 
![[Capture d'écran 2026-06-12 153947.png]]
Correctness : 
- Every $(a, b, \tilde h)$ in the list satisfies $\tilde h = g^ah^b$ 
- $g^{a_1}h^{b_1} = g^{a_2}h^{b_2}$ implies $h = g^{-\frac{a_1 - a_2}{b_2 - b_2}}$
#### Pohlig-Hellman
Assume $\lvert G \rvert = n_1n_2$ and let $g$ a generator of $G$. $$
h = g^k \implies h^{n_1} = (g^{n_1})^k
$$where $g^{n_1}$ generates a subgroup of order $n_2$. 
Solving DLP in that subgroup gives $k \bmod n_2$. 
$\rightarrow$ repeat for each (power of prime) factor of $\lvert G \rvert$ 
Use a constructive version of Chinese Remainder Theorem to get $k$
Note : to counter that, use a large prime order group. 
##### Extended Euclide Algorithm
Goal : compute $d, r, s$ such that $d = ra + sb = \gcd(a, b)$. 
![[Capture d'écran 2026-06-12 155112.png]]
Indeed, if $rb + s(a - qb) = d$ then $sa + (r - qb)b = d$. 
Complexity : $\mathcal{O}(\lvert a \rvert^2)$. 
##### Chinese Remainder Theorem
Let $p_1, p_2$ prime and let $n = p_1p_2$. For any $x_1, x_2$ there is one and only one value of $x \bmod n$ such that $x = x_1 \bmod p_1$ and $x = x_2 \bmod p_2$. 
This value can be efficiently computed : 
1. Compute $(p_1^{-1}\bmod p_2)$ and $(p_2^{-1}\bmod p_1)$ using the extended Euclide algorithm. $$
1 = p_1(p_1^{-1}\bmod p_2) + p_2 (p_2^{-1}\bmod p_1)
$$
2. Deduce $$
x = x_2 p_1(p_1^{-1}\bmod p_2) + x_1 p_2 (p_2^{-1}\bmod p_1)
$$
More generally, if $n = \prod_{i=1}^{N} p_i^{e_i}$, for any $x_1, \dots, x_N$, there is one and only one value of $x \bmod n$ such that $$
x = x_i \bmod p_i^{e_i}
$$And this value can be efficiently computed. 
#### Generic DLP Algorithms
Exhaustive search, Pollard's rho, BSGS and Pohlig-Hellman algorithms work for **any group**. 
For any group $G$, solving DLP in $G$ costs at most $\mathcal{O}(\sqrt{q})$ group operations, where $q$ is the largest prime dividing $\lvert G\rvert$. 
#### DLP Algorithms for Finite Fields
In cryptography, we often use multiplicative groups of finite fields. 
For $\mathbb{F}_p^*$, the best algorithm has complexity : $$
L_p\left( \frac{1}{3}, \sqrt[3]{\frac{64}{9}} \right) = \exp\left( \left( \sqrt[3]{\frac{64}{9}}  + o(1)\right)(\log p)^{1/3} (\log \log p)^{2/3}  \right) 
$$
### Index Calculus
This is a generic framework to solve DLPs, but some steps are group-specific. 
Let $g, h$ a DLP. Define a factor basis $\mathcal{F} \subset G$, ensuring $\mathcal{F}$ contains a generator. 
We can assume $g \in \mathcal{F}$, otherwise do the following : 
- Pick a generator $g' \in \mathcal{F}$
- Compute $a$ such that $g = (g')^a$ 
- Compute $b$ such that $h = (g')^b$ 
- Compute $k = \frac{b}{a} \bmod \lvert G \rvert$ 
Find about $\lvert \mathcal{F}\rvert$ relations between factor basis elements $$
\mathcal{R}_j : \prod_{f_i \in \mathcal{F}}^{} f_i^{a_{i, j}} = 1 
$$(the algorithm to compute the relations is group-specific)
Deduce $$
\sum_{f_i \in \mathcal{F}}^{} a_{i, j} \log_g f_i = 0 
$$Use linear algebra to compute all $\log_g f_i$, and deduce the discrete logarithm of $h$ (group-specific)
Remarks : 
- Relations often involve few elements, hence linear algebra is sparse
- In some cases, $h$ is included in the factor basis and the last step is avoided
#### Example : for $\mathbb{F}_p^*$ 
DLP : given $g, h \in \mathbb{F}_p^*$ , find $k$ such that $h = g^k$
Factor basis made of **small primes** and $h$ : $$
\mathcal{F}_B := \left\{ \text{primes }p_i \leq B  \right\} \cup \left\{ h \right\} 
$$Relation search : 
- Compute $r_j := g^{a_j}h^{b_j}$ for random $a_j, b_j \in \{1, \dots, p-1\}$
- If all factors of $r_j$ are $\leq B$, we have one relation $$
g^{a_j}h^{b_j} = \prod_{p_i \in \mathcal{F}}^{} p_i^{e_{i, j}} 
$$
Linear algebra produces $a, b$ such that $g^ah^b = 1$. 
### ECDLP 
([[Homomorphic Encryption#Elliptic Curve Discrete Logarithm Problem (ECDLP)]])
We have an $L\left( \frac{2}{3} \right)$ algorithm to solve ECDLP over fields $\mathbb{F}_{q^n}$ if $q$ and $n$ have the right size. In most applications, we are interested in ECDLP over either prime fields, or $\mathbb{F}_{2^n}$ with extension degree $n$ prime. 
## Discrete Logarithm Variants and Factoring
