# E-Voting
Math definitions : [[Math Background]]
We want **end-to-end verifiable** voting systems, where the voters can verify that their votes are :  
- Cast as intended
- Recorded as cast
- Counted as recorded
The main security goals of such systems are : 
- Correctness : 
	$\rightarrow$ Result consistent with intended votes
	$\rightarrow$ Verifiability : system should give correctness guarantee
- Privacy : 
	$\rightarrow$ Confidentiality of individual votes 
	$\rightarrow$ Coercion resistance : cannot prove your vote
There can be 2 approaches to e-voting : 
## Homomorphic encryption
cf. [[Homomorphic Encryption]]
Use encryption scheme such that $$
\mathrm{Enc}(v_1 + v_2) = \mathrm{Enc}(v_1) \otimes \mathrm{Enc}(v_2)
$$for some operation $\otimes$. 
ElGamal is homomorphic !
### ElGamal Encryption
The public key is a cyclic group $G$, a generator $g$ and $h = g^s$. 
The private key is $s$. 
- Encryption of $m \in G$ : $$
(c_1, c_2) = (g^r, mh^r) = (g^r, mg^{sr})
$$for some random $r$
- Decryption of $(c_1, c_2)$ : $$
m' = \frac{c_2}{c_1^{s}} = \frac{mg^{sr}}{g^{sr}} = m
$$([[Provable Security#ElGamal Encryption Protocol]])
### Properties
<i>Property 1</i><br>ElGamal encryption is homomorphic. 
<i>Proof. </i>
Let $(c_1, c_2) = (g^r, mh^r)$ be the encryption of $m$ and $(c_1', c_2') = (g^{r'}, m'h^{r'})$ be the encryption of $m'$. Then, we have $$
(C_1, C_2) := (c_1c_1', c_2c_2') = (g^{r + r'}, mm'h^{r+r'})
$$which is an encryption of $mm'$ !$$\tag*{$\blacksquare$}$$<i>Property 2</i><br>ElGamal is re-randomizable.
<i>Proof. </i>
To prove it, we show that we can modify the ciphertext without changing the underlying plaintext. Using the equation above, if $m' = 1$, then $(C_1, C_2) \neq (c_1, c_2)$ is an encryption of $mm' = m$. $$\tag*{$\blacksquare$}$$
### Exponential ElGamal
We can also build an exponential version of ElGamal : 
- Public key : cyclic group $G$, a generator $g$, $h = g^s$ and a bound $B$
- Private key : $s$ 
- Encryption of $0 \leq m \leq B$ : $(c_1, c_2) = (g^r, g^mh^r)$ for some random $r$
- Decryption of $(c_1, c_2)$ : 
	- First compute $\frac{c_2}{c_1^s} = g^m$ 
	- solve DLP over a small interval to get $m$
<i>Property 1</i><br>Exponential ElGamal has the same security as ElGamal. 
<i>Proof. </i>
Assume an adversary $\mathcal{A}$  can break IND-CPA of EEG with advantage $\epsilon$. 
We construct an adversary $\mathcal{B}$ that breaks IND-CPA of standard ElGamal. 
$\mathcal{B}$ can choose $g^{m_0}, g^{m_1}$ as the 2 plaintexts, where $m_0 \neq m_1 \in \mathbb{Z}/\mathbb{Z}_B$ and sends it to the challenger. 
$\mathcal{B}$ receives $(c_1, c_2)$ which is either an encryption of $g^{m_0}$ or $g^{m_1}$. 
Then, $\mathcal{B}$ can query $\mathcal{A}$ and if $\mathcal{A}$ answers correctly, $\mathcal{B}$ is able to know $b \in \{0, 1\}$ such that $$

(c_1, c_2) = \mathrm{Enc}_{EG}(g^{m_b})$$
Since $\mathcal{A}$ answers correctly with a probability of $\frac{1}{2} + \epsilon$, so does $\mathcal{B}$, breaking the IND-CPA of ElGamal.
$$\tag*{$\blacksquare$}$$
<i>Property 2</i><br>Exponential ElGamal is homomorphic. 
<i>Proof. </i>
Let $(c_1, c_2) = (g^r, g^mh^r)$ be an encryption of $m$ and $(c_1', c_2') = (g^{r'}, g^{m'}h^{r'})$ be an encryption of $m'$. Then, we have $$
(C_1, C_2) = (c_1c_1', c_2c_2') = (g^{r + r'}, g^{m+m'}h^{r+r'})
$$which is an encryption of $m+m'$ modulo the group order. 
$$\tag*{$\blacksquare$}$$
<i>Property 3</i><br>Exponential ElGamal is re-randomizable.
<i>Proof. </i>
If $m' = 0$ in the above equation, then $(C_1, C_2)$ is a new encryption of $m$. $$\tag*{$\blacksquare$}$$
### Secret Sharing
With homomorphic encryption, we can also do secret sharing. The main motivation behind this technique is that we want to prevent election officials to be able to decrypt individual votes. 
The goal is to share a decryption key among several trustees. 
ElGamal Variant : 
- Each trustee chooses secret $s_i$ and shares $h_i = g^{s_i}$
- The public key is $h = \prod_{i}^{} h_i = g^{\sum_{i}^{}s_i}$
- Encryption of $m$ like in EG : $(c_1, c_2) = (g^r, mh^r)$ for some random $r$
- To decrypt $(c_1, c_2)$, trustees must jointly compute $\frac{c_2}{\prod_{i}^{}c_1^{s_i}} = m$ 
#### Threshold Encryption
This is a fancy variant of the previous scheme. The goal is also the share a decryption key among several trustees, but moreover, there is an integer $t$ such that any set of $n \geq t$ trustees can decrypt, but no smaller can. 
- Assume each trustee has a private key share $s_i = F(i)$ for some polynomial $F$ known to none, with $\deg F = k-1$.
- The corresponding secret key is $F(0)$ and the public key is $h = g^{F(0)}$.
- Encryption of $m$ like EG : $(c_1, c_2) = (g^r, mh^r)$ for some random $r$.
- To decrypt $(c_1, c_2)$, trustees need to compute $\frac{c_2}{c_1^{F(0)}} = m$
Why does it work ? Thanks to Lagrange interpolation !
For any subset $\{x_1, \dots, x_t\} \in \{1, \dots, n\}$, we have $$
F(0) = \sum_{i}^{} L_iF(x_i) 
$$where $$
L_i = \prod_{i\neq j}^{} \left(\frac{-x_j}{x_i - x_j}\right) 
$$<i>Proof. </i>
Use Lagrange interpolation $$
F(x) = \sum_{i}^{} L_i(x)F(x_i) 
$$where $$
L_i(x) = \prod_{i\neq j}^{} \left(\frac{x-x_j}{x_i-x_j}\right) 
$$We can compute the public key as $$
\prod_{i}^{} \left(g^{F(x_i)}\right)^{L_i} = g^{F(0)} 
$$And the decryption can be performed as $$
\frac{c_2}{\prod_{i}^{} \left(c_1^{F(x_i)}\right)^{L_i}} = \frac{c_2}{c_1^{F(0)}} = m
$$$$\tag*{$\blacksquare$}$$
Note that the polynomial $F$ has to remain secret for trustees since the secret key is $F(0)$ ! How can we make sure that party $i$ has $F(i)$ ? 
- Each party chooses a random polynomial $F_i(x)$ 
- Party $i$ privately sends $F_i(j)$ to party $j$
- Define $F(x) = \sum_{}^{} F_i(x)$ 
- So $F(j) = \sum_{}^{} F_i(j)$
Note that $F(x)$ is random as long as one trustee is honest. 

## Mixnets
We re-randomize and shuffle the votes before decryption
$\rightarrow$ Votes can no longer be linked to voters
$\rightarrow$ Votes can be decrypted individually
Using multiple trustees, they can : 
- Each trustee generates their keys 
- Each vote is encrypted several times, one per trustee
- Each trustee removes one encryption layer and shuffle all the votes
![[Screenshot 2026-05-26 at 16.36.37.png|700]]
## HELIOS
HELIOS is a full voting system, including voter registration, voting process, bulletin board, tallying. 
It's **open audit**, meaning that the election outcome can be verified even when the whole system is corrupt
It has limits : 
- Only suitable for elections with low coercion risk
- Vote privacy relies on server goodwill
It has multiple version. 
### HELIOS 1.0
- Mixnet approach with a single trustee
- Uses ElGamal encryption
- Hash of encryption given to voters as receipt
- Bulletin board of votes
- Sako-Killian proofs for mixnets
- Chaum-Pedersen proofs for correct decryptions
	[[ZK Proofs#ZK Protocols for DLP-related Relations]]
- Ballot auditing
- "Coerce me" button
