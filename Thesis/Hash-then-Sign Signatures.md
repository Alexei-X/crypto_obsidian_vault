# Hash-then-Sign Signatures

**Hash-then-Sign** paradigm : Instead of signing messages directly, messages are first hashed and then their hash values are signed. 
This has attractive properties ! 
- The core signing algorithm doesn't have to get involved with handling arbitrarily long message inputs
- The task of signing and hashing can be performed by different entities
## Signature Schemes
<b>Definition DSS</b><br>A *digital signature scheme* for a message space $\mathcal{M}$ consists of a signing key space $\mathcal{SK}$, a verification key space $\mathcal{VK}$, a signature space $\Sigma$, a key generation algorithm $\mathrm{gen} \to \mathcal{SK}\times\mathcal{VK}$, a signing algorithm $\mathcal{SK} \times \mathcal{M} \to \mathrm{sgn} \to \Sigma$, and a verification algorithm $\mathcal{VK}\times \mathcal{M} \times \Sigma \to \mathrm{vfy} \to \{0, 1\}$. 
For correctness, we want $\forall m \in \mathcal{M}$, after $(sk, vk) \leftarrow \mathrm{gen}$ and $\sigma \leftarrow \mathrm{sgn}(sk, m)$ and $v \leftarrow \mathrm{vfy}(vk, m, \sigma)$ that $v = 1$. 

<b>Definition 1</b> (Unforgeability)<br>Consider the 
## Hash-then-Sign Signatures : HtS
<b>Definition HtS-DSS</b><br>A Hash-then-Sign digital signature scheme for a message space $\mathcal{M}$ consists of a signing key space $\mathcal{SK}$, a verification key space $\mathcal{VK}$, a signature space $\Sigma$, a hash seed space $\mathcal{HS}$, a hash value space $\mathcal{HV}$, a key generation algorithm $\mathrm{gen} \to \mathcal{SK}\times\mathcal{VK}\times \mathcal{HS}$, a hash function $\mathcal{HS}\times\mathcal{M} \to \mathrm{hash} \to \mathcal{HV}$, and algorithms $\mathcal{SK}\times \mathcal{HV} \to \mathrm{sgn} \to \Sigma$ and $\mathcal{VK} \times \mathcal{HV} \times \Sigma \to \mathrm{vfy} \to \{0, 1\}$. 

