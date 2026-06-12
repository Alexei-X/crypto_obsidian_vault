<div class="latex-title-block">
<h1 class="title">Zero-Knowledge Proofs</h1>
<div class="author">Alex Bataille</div>
<div class="date">2026</div>
</div>

## Introduction
We want to be able to prove some statement, without revealing any extra information to the verifier. 
### Example
As an example, we can consider Mic Ali's Cave. We consider a cave with 2 entries and a wall in the middle, which can be opened by a secret password. 
![[Capture d'écran 2026-06-12 102813.png]]
* The prover wants to prove to the verifier that they know the password, but without revealing it. They also don't want the verifier to be able to prove to others that they know the password. 
* Proof : 
	$\rightarrow$ The prover privately enters one entry at random
	$\rightarrow$ The verifier chooses one entry at random, and tells the prover to exit that way
	$\rightarrow$ The prover comes by the requested exit, using the password if needed
* Without password, the prover cheats with 50% chance
* Repeating $k$ times, the probability to pass all tests is $2^{-k}$. 
This protocol is **correct** : if the prover knows the password, they will be able to convince the verifier that they know it. 
This protocol is **sound** : if the prover doesn't know the password, they will not be able to convince the verifier that they know it (except with a negligible probability).
We also have that observing this interaction between the prover and the verifier doesn't reveal anything about the password, indeed this interaction can be simulated without the password by choosing the exit direction beforehand. 
$\implies$ Verifier can't use the proof to convince someone else that the prover knows the password, there is no way to detect a potential collusion between prover and verifier. 

## Definitions

Statements to prove will be modeled in the form $x \in \mathcal{L}$ where $$
\mathcal{L} = \left\{ x \in \mathcal{X} \mid \exists w \in \mathcal{W} : R(x, w) = 1\right\} 
$$here, 
- $R : \mathcal{X} \times \mathcal{W} \to \{0, 1\}$ is a **binary relation**
- $w$ is called a **witness** for $x \in \mathcal{L}$
For instance, $\mathcal{L}$ is a set of caves with a password wall, or for DH tuples : 
- $\mathcal{X}= (\mathbb{F}_p^*)^4$, $\mathcal{W} = \mathbb{Z}_{p-1}$ 
- $R((g_1, h_1, g_2, h_2), w) = 1 \iff h_1= g_1^w \land h_2 = g_2^w$ 

The prover knows a pair $(x, w)$ such that $R(x, w) = 1$ and the verifier only gets $x$. 
The prover wants to prove $x  \in \mathcal{L}$ without revealing anything about the witness $w$. 
$\rightarrow$ The proofs are often interactive, several messages between the prover and the verifier can be sent, and the order matters !
Notes : 
- We typically consider only NP statements (R evaluated in polynomial time)
- It's typically hard for the verifier to compute $w$ given $x$
### Sigma Protocol
Goal : the prover wants to convince the verifier that $x \in \mathcal{L}$, knowing $w$ such that $R(x, w) = 1$. 
3-round communication protocol : 
1. Prover sends a **commitment** to the verifier
2. Verifier sends a **challenge** to the prover
3. Prover sends a **response** to the verifier
$\rightarrow$ at the end, the verifier either accepts or rejects the proof. 
### Security Properties
Let the prover $\mathcal{P}$ and the verifier $\mathcal{V}$ be probabilistic polynomial time algorithms. 
A protocol is **correct** : if the prover knows the witness, and follows the protocol then the proof will be accepted by the verifier : $$
Pr\left[ \mathcal{V}(x, \alpha, \beta, \gamma) = 1 \mid \begin{cases}
 \alpha \leftarrow \mathcal{P}(x, w); \\
\beta \leftarrow \mathcal{V}(x, \alpha); \\
\gamma \leftarrow \mathcal{P}(x, w, \alpha, \beta)
\end{cases}\right] = 1 
$$
A protocol is **sound** : if the prover doesn't know the witness then the verifier will reject the proof. 
A protocol is **zero-knowledge** : nothing is revealed about witness $w$, except the fact that it exists. 
### Identification Protocol
- A key generation algorithm produces a pair of public and private keys $(x, w)$.
- Interactive protocol aims to prove knowledge of secret key $w$ corresponding to public key $x$.
- Security property : cannot pass the verification without knowing the secret key, even after observing other protocol instances running. 
- Solution : run a ZK protocol for the specific relation$$R(x, w) = 1 \Leftrightarrow w\text{ is a private key for public key }x$$
### $k$-special Soundness
**Soundness** : if the prover does not know the witness, then the verifier will not accept the proof. 
**$\mathbf{k}$-special soundness** : one can efficiently extract a valid witness from $k$ valid responses to $k$ distinct challenges (all of them for the same initial commitment) : 
- If a witness can be extracted then $x \in \mathcal{L}$ 
- If Prover successful on random challenge, we can expect it is also able to produce valid answers to $k$ of them
- Prover could simulate verifier and produce $k$ responses then extract the witness $\Rightarrow$ Prover knows witness
Note that $k$ challenges and responses are **not exchanged** in the actual protocol taking place because this would leak the witness ! 
The Ali Baba protocol is sound but not special sound. 
Formally, 
<b>Definition k-special soundness</b><br>There exists a probabilistic polynomial time algorithm $\mathcal{E}$ (the extractor) such that whenever $\mathcal{E}$ is given as input a statement $x \in \mathcal{L}$ and $k$ accepting  conversations $(\alpha, \beta_1, \gamma_1), \dots, (\alpha, \beta_k, \gamma_k)$, such that $\beta_i \neq \beta_j \quad \forall i \neq j$, $\mathcal{E}$ always outputs $w \in \mathcal{W}$ such that $R(x, w) = 1$. 
### Honest Verifier Zero-Knowledge (HVZK)
$=$ there exists an efficient simulator algorithm which can produce a conversation $(\alpha, \beta, \gamma)$ indistinguishable from an actual prover-verifier conversation without knowing witness $w$. 
- If witness is not used to produce the triple $(\alpha,\beta, \gamma)$, then no information about it is leaked
Note that the simulator can produce $\alpha, \beta, \gamma$ in any order, so no contradiction with soundness. 
$\rightarrow$ The Ali Baba protocol is HVZK
Formally, 
<b>Definition Honest-verifier zero-knowledge</b><br>There exists a probabilistic polynomial time algorithm $\mathcal{S}$ (the simulator) such that takes as input $(x, \beta)$ and is such that : $\forall (x,\beta)$ , $\mathcal{S}$ always outputs a pair $(\alpha, \gamma)$ such that $(\alpha, \beta, \gamma)$ is an accepting conversation for $x$
### Perfect/statistical/computational Properties
Depending on the protocols, the properties above may hold perfectly, statistically or computationally : 
- **Perfectly** : property always true
- **Statistically** : true with overwhelming probability
- **Computationally** : true, unless some computational problem is solved (DLP)
Impossibility result : Cannot have perfect soundness and perfect ZK together !
A ZK proof that only has computational soundness is sometimes called a ZK argument instead. 
## ZK Protocols for Graph Isomorphism and 3-coloring
### Graph Isomorphism
<b>Definition Graph Isomorphism</b><br>A graph isomorphism is a function sending vertices of one graph to another graph while preserving the edges relations. 
A very famous problem in complexity theory is to decide whether 2 graphs are isomorphic to each other, it is believed to be NP-intermediate. 
Assume we have a solution to a graph isomorphism problem, how to prove this without revealing the solution ? 
Protocol : 
- Let $\pi$ be an isomorphism from $G_0$ to $G_1$, in other words, $\pi$ is a witness that $G_0$ and $G_1$ are isomorphic
- The prover constructs a random isomorphism $\sigma_1 : G_1 \to H$. Concretely, a random permutation of the vertices
- Prover commits with graph $H$
- Verifier sends a challenge bit $b$
- If $b = 0$, then the prover computes $\sigma_0 = \sigma_1 \circ \pi$ 
- The prover responds with isomorphism $\sigma_b : G_b \to H$ 
- Repeat this protocol $k$ times
#### Security Properties
- The protocol is correct
- $2$-special soundness : given valid answers to challenges $b = 0$ and $b = 1$, one can compute $\pi = \sigma_1^{-1} \circ \sigma_0$. 
* HVZK : a simulator can choose a random $b$, then a random $\sigma_b$, then deduce $H$ and return $(H, b, \sigma_b)$ 
Note that the simulator does not compute messages in the same order as in normal execution of the protocol but it's not a problem because the message triple distribution is still as expected. 
- The prover can cheat with probability 50% by guessing $b$ if $k = 1$, this is reduced to $2^{-k}$ after $k$ repetitions
### 3-coloring and Arbitrary NP Statements
<b>Definition k-coloring</b><br>Color vertices of a graph with $k$ colors such that no two adjacent vertices have the same color, or determine that no such coloring exists. 
For $k = 3$, it has been show to be an NP-complete problem. 
Protocol : 
- Let $G = (V, E)$ be a graph, and let $$
\phi : V \to \left\{ 0, 1, 2 \right\} 
$$be a valid 3-coloring of $G$.
- Let $f$ be a one-way permutation
- The prover chooses random $\pi : \{0, 1, 2\} \to \{0, 1, 2\}$ and lets $\phi' = \pi \circ \phi$
- The prover sends commitments $$
\left\{ f\left( \phi'(v) \Vert r_v \right) \mid v \in V \right\} 
$$where $r_v$ are sufficiently large random bitstrings
- The verifier challenges on one edge $e \in E$ 
- The prover reveals $\phi'(v)\Vert r_v$ for both $v$ on the edge $e$
- The verifier checks the two vertices have distinct colors. It also computes $f(\phi'(v)\Vert r_v)$ and compares this with the committed values
- Prover and verifier repeat this protocol $\lvert E \rvert^2$ times
#### Security Properties
- The protocol is correct
- Special soundness : valid responses to all edges will lead to a valid 3-coloring $\phi' = \pi \circ \phi$
- HVZK : simulator picks random $e$, gives two distinct random colors to the associated vertices, chooses random $r_v$, computes the commitments. 
	$\rightarrow$ only computational, a powerful adversary could invert the one-way permutation
<i>Property</i><br>All NP statements have zero-knowledge proofs. 
<i>Proof. </i>
Any NP statement can be reduced to a 3-coloring problem since 3-coloring is NP-complete. Both the prover and the verifier can do this reduction, the prover then proves 3-coloring with ZK protocol.  $$\tag*{$\blacksquare$}$$
## ZK Protocols for Conjonctions and Disjunctions
### Conjonctions
Let $R_0 : \mathcal{X}_0 \times \mathcal{W}_0 \to \{0, 1\}$ and $R_1 : \mathcal{X}_1 \times \mathcal{W}_1 \to \{0, 1\}$ be two relations. 
Let as before $$
\mathcal{L}_i = \left\{ x \in \mathcal{X}_i \mid \exists w \in \mathcal{W}_i : R_i(x, w)= 1 \right\} 
$$Let $$
\mathcal{L} = \left\{ (x_0, x_1) \in \mathcal{X}_0 \times \mathcal{X}_1 \mid x_0 \in \mathcal{L}_0 \land x_1 \in \mathcal{L}_1 \right\} 
$$The goal is : given ZK proofs protocols for $\mathcal{L}_0$ and $\mathcal{L}_1$, construct a ZK protocol for $\mathcal{L}$. 
Solution : 
- Run both proofs sequentially ! 
- It's correct, sound, and HVZK. 
Usually, we can run the proofs in parallel : 
![[Capture d'écran 2026-06-12 135441.png|600]]
### Disjunctions
Let $R_0 : \mathcal{X}_0 \times \mathcal{W}_0 \to \{0, 1\}$ and $R_1 : \mathcal{X}_1 \times \mathcal{W}_1 \to \{0, 1\}$ be two relations. 
Let as before $$
\mathcal{L}_i = \left\{ x \in \mathcal{X}_i \mid \exists w \in \mathcal{W}_i : R_i(x, w)= 1 \right\} 
$$Let $$
\mathcal{L} = \left\{ (x_0, x_1) \in \mathcal{X}_0 \times \mathcal{X}_1 \mid x_0 \in \mathcal{L}_0 \lor x_1 \in \mathcal{L}_1 \right\} 
$$The goal is : given ZK proof protocols for $\mathcal{L}_0$ and $\mathcal{L}_1$, construct a ZK proof protocol for $\mathcal{L}$.
Note that the proof should of course not reveal which $x_i \in \mathcal{L}_i$ is true. 
Solution : 
![[Capture d'écran 2026-06-12 141733.png]]
This has correctness, and for special soundness : 
- Suppose you have 2 valid transcripts $(\alpha_0, \alpha_1, \beta, \beta_0, \beta_1, \gamma_1, \gamma_2)$ and $(\alpha_0, \alpha_1, \beta', \beta_0', \beta_1', \gamma_1', \gamma_2')$ with $\beta \neq \beta'$ 
- Then for at least one value $b \in \{0, 1\}$, we have $\beta_b \neq \beta_b'$
- Run the special soundness extractor on $(\alpha_b, \beta_b, \gamma_b, \beta_b', \gamma_b')$
It's HVZK : to simulate the conversation, choose random $\beta_0, \beta_1$, run simulators on both proofs separately, set $\beta = \beta_0 \oplus \beta_1$. 
## ZK Protocols for DLP-related Relations
For efficiency purposes, we want specific constructions for specific statements ! 
### Schnorr Identification Protocol (DLP)
Let $G$ be a finite group of prime order $q$, and let $g$ be a generator for that group. The prover has a private key $x$, and a public key $h = g^x$. He wants to prove knowledge of $x$ without revealing anything about it. 
Protocol : 
- Prover chooses random $r$ and computes $R = g^r$
- Prover sends $R$ to verifier
- Verifier sends random challenge $c$ to prover
- Prover answers with $s = r + xc \bmod q$ 
- Verifier checks that $g^s = Rh^c$
### Chaum-Pedersen Protocol (DHP)

### Correct Decryption of ElGamal Ciphertext


## The Fiat-Shamir Transform
