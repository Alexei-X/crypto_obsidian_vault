<div class="latex-title-block">
<h1 class="title">Exam Questions</h1>
<div class="author">Bataille Alex</div>
<div class="date"></div>
</div>
## E-voting
### Question 1
<b>Question</b><br>Explain the meaning of "end-to-end verifiable voting system". Describe how advanced cryptographic tools can be used to build these systems. 
<b>Answer</b><br>An end-to-end verifiable voting system is a voting system in which the voters can verify that their votes are : 
- Cast as intended
- Recorded as cast
- Counted as recorded
The advanced cryptographic tools we can use to build such systems are : 
- Encryption schemes
	- To encrypt the vote such that no one can read who the candidate voted is
	- To decrypt the vote to cast it as intended
- Homomorphic encryption
	- To allow vote aggregation, ciphertext re-randomization
- Zero-knowledge proofs
	- To prove the vote was legit without having to give it away
- Mixnets
	- To shuffle the votes before encryption so that no vote is related to the original voter
### Question 2
<b>Question </b><br>Compare the homomorphic encryption and mixnet approaches for e-voting. What are their respective advantages and drawbacks ? 
<b>Answer </b><br>With homomorphic encryption, we can 
- compute the the encryption of the election result from the encryption of the individual votes 
- Re-randomize ciphertexts without changing underlying plaintext
- Make threshold decryption to make sure decryption is possible only when trustees work together
With the mixnet approach, we can
- 
