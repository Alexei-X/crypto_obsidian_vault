# Math Background

A **group** is a set $G$ along with a binary operation $\odot$ that satisfy the following properties : 
- closure : $\forall g, h \in G, \quad g \odot h \in G$ 
- associativity : $\forall g_1, g_2, g_3 \in G, \quad g_1 \odot(g_2 \odot g_3) = (g_1 \odot g_2) \odot g_3$ 
- identity : there exists an identity $e \in G$ such that $\forall g \in G, \quad e \odot g = g \odot e = g$ 
- inverse : $\forall g \in G$, there exists $h \in G$ such that $g \odot h = h \odot g = e$ 
The **order** of an element $a \in G$ is the smallest positive integer $m = \mathrm{ord}(a)$ such that $$
[m]a = \underset{m \text{ times}}{\underbrace{a \odot a \odot \dots \odot a}}=e$$$\forall a \in G$, $\mathrm{ord}(a)$ divides $\lvert G \rvert$. 
An element $g \in G$ is said to be a **generator** of $G$ if $\mathrm{ord}(g) = \lvert G \rvert$. A generator can be used to generate all the elements of the group. 

The group $(\mathbb{Z}_n, +)$ is the set $\{0, 1, \dots, n-1\}$ together with addition modulo $n$. In this group, the identity is $0$ and the inverse of $x$ is $-x \bmod n$. 
The number $1$ is a generator of the group because all the elements can be represented by adding $1$ to itself an appropriate number of times. 
<b>Theorem (Bézout)</b><i></i><br>For any positive integers $a, b$, there exists integers $x, y$ such that $$
ax + by = \gcd(a, b)
$$<b>Corollary </b> <i></i><br>$a$ and $b$ are relatively prime iff there exists integers $x, y$ such that $ax + bx = 1$. 

The group $(\mathbb{Z}_n^*, \times)$ is the set $\{x : 0 < x < n \land \gcd(x, n) = 1\}$ together with multiplication modulo $n$. In this group, the identity is $1$ and the inverse of $x$, denoted $x^{-1} \bmod n$ is such that $xx^{-1} \equiv 1 (\bmod n)$. 
The inverse $x^{-1} \bmod n$ exists and is unique iff $\gcd(x, n) = 1$. 
<i>Remark. </i> Note that when $n$ is a prime number, this group is the set $\{1, 2, \dots, n-1\}$.<br>
A **generator** $g$ of $(\mathbb{Z}_n^*, \times)$ is an element with $\mathrm{ord}(g) = \phi(n)$. Such a generator exists when $n$ is $2, 4, p^a$ or $2p^a$, with $p$ an odd prime. 

**Euler's totient function** $\phi(n) = (1-p)(1-q)$ is the number of integers smaller than $n = pq$ and that are relatively prime with $n$. 
We thus have $\lvert( \mathbb{Z}_n^*, \times) \rvert = \phi(n)$. 

<b>Theorem (Fermat's little theorem)</b><i></i><br>Let $p$ be a prime and $a$ an integer not a multiple of $p$, then $a^{p-1}\equiv 1 (\bmod p)$.
<b>Theorem (Euler)</b><i></i><br>Let $a, n$ two relatively prime integers, then $a^{\phi(n)} \equiv 1 (\bmod n)$. 
Consequently, when computing exponentiations modulo $n$, the exponent can be reduced modulo $\phi(n)$ : $$
a^e \equiv a^{e \bmod \phi(n)} (\bmod n)
$$
