<head>
<style>
div.LineHeight20per {
  line-height: 20%;
}
div.LineHeight100per {
  line-height: 100%;
}
div.LineHeight200per {
  line-height: 200%;
}
</style>
</head>

## Bulletproofs and Mimblewimble

@div[text-left]

"<i>Short like a bullet with bulletproof security assumptions</i>"

@divend

<div class="LineHeight200per"> <br></div>

- Introduction
- Terminology Recap
- How do Bulletproofs work?
- Applications for Bulletproofs
- Comparison to other ZK Proof Systems
- Interesting Bulletproofs Implementation Snippets
  - Current & Past Efforts
  - Security Considerations
  - Wallet Reconstruction and Switch Commitment - Grin
- Conclusions

<div class="LineHeight100per"> <br></div>

@div[text-left]

See full report [*here*](https://tlu.tarilabs.com/cryptography/bulletproofs-and-mimblewimble/MainReport.html).

@divend

---

## Introduction

- Bulletproofs form part of the family of distinct Zero-knowledge (ZK) proof systems, like zk-SNARK, STARK and ZKBoo.
- ZK proofs designed that a *prover* is able to indirectly verify a statement without providing any information beyond the verification of the statement, example to prove a number is found that solves a cryptographic puzzle and fits the hash value without having to reveal the nonce.
- Bulletproofs technology is a Non-interactive ZK (NIZK) proof protocol for general Arithmetic Circuits with very short proofs (arguments of knowledge) without a trusted setup. They rely on the Discrete Logarithm (DLP) assumption and are made non-interactive using the Fiat-Shamir Heuristic.
- Bulletproofs Multi-party Computation (MPC) protocol: Distributed proofs of multiple *provers* with secret committed values aggregated into a single proof before the Fiat-Shamir challenge is calculated and sent to the *verifier*, minimizing rounds of communication. Secret committed values stay secret.

+++

- Essence of Bulletproofs its inner-product algorithm, a proof for two independent *binding* vector Pedersen Commitments (PC). Bulletproofs yield communication-efficient ZK proofs.
- [Mimblewimble](https://tlu.tarilabs.com/protocols/mimblewimble-1/sources/PITCHME.link.html) (MW) is a blockchain protocol designed for confidential Txs. The essence is that a PC to $ 0 $ can be viewed as an Elliptic Curve (EC) Digital Signature Algorithm (ECDSA) public key, and for a valid confidential Tx the difference between outputs, inputs, and transaction fees must be $ 0 ​$.
- A *prover* can sign Txs with the difference of outputs and inputs as the public key. Thus a greatly simplified blockchain in which all spent Txs are pruned, and new nodes efficiently validate the entire blockchain without downloading any old spent Txs. 
- MW blockchain consists only of block-headers, remaining UTXOs with range proofs and an unprunable Tx kernel per Tx. MW allows Txs to be aggregated before being committed to the blockchain.



---

## Terminology Recap

<div class="LineHeight20per"> <br></div>

- Let $ p $ be a large prime number
- Let $ \mathbb G $ denote a cyclic group of prime order $ p $ 
- Let $ \mathbb Z_p $ denote the ring of integers $ modulo \mspace{4mu} p $ 
- Let $ \mathbb F_p $ be a group of elliptic curve points over a finite (prime) field
- If not otherwise specified, lower case $ x,r,y $ etc. are ordinary numbers (integers), upper case $ H,G $ are curve points

<div class="LineHeight100per"> <br></div>

@div[text-left]

A <u>commitment scheme</u> in a ZK proof is a cryptographic primitive that allows a *prover* to commit to only a single chosen value/statement from a finite set without the ability to change it later (*binding* property) while keeping it hidden from a verifier (*hiding* property).

<div class="LineHeight100per"> <br></div>

Both *binding* and *hiding* properties are then further classified in increasing levels of security to be *computational*, *statistical* or *perfect*. No commitment scheme can at the same time be perfectly *binding* and perfectly *hiding*.

<div class="LineHeight100per"> <br></div>

The <u>Discrete Logarithm Problem</u> (DLP) with $ \log_ba = k $ such that $ b^k=a $ for any integer $ k $ where $ a,b \in \mathbb G $ is hard to guess (has no efficient solution) for carefully chosen $ \mathbb F_p ​$.

@divend

+++

@div[text-left]

An <u>arithmetic circuit</u> $ C $ over a field $ F $ and `$ (x_1, ..., x_n) $` is a directed acyclic graph whose vertices are called gates. Linear consistency equations relate the inputs and outputs of the (addition and multiplication) gates.

<div class="LineHeight100per"> <br></div>

The size is the number of gates in it, with the depth being the length of the longest directed path. *Upper bounding* the complexity of a polynomial $ f $ is to find any arithmetic circuit that can calculate $ f $, whereas *lower bounding* is to find the smallest arithmetic circuit that can calculate $ f $.

<div class="LineHeight100per"> <br></div>

An example of a simple arithmetic circuit with size six and depth two that calculates a polynomial is shown below.

@divend


@div[s400px]

  ![Ricardian Contract](https://raw.githubusercontent.com/tari-labs/tari-university/master/src/cryptography/bulletproofs-and-mimblewimble/sources/ArithmiticCircuit.png)

@divend

+++

@div[text-left]

The <u>Elliptic Curve (EC) PC</u> to value $ x \in \mathbb Z_p $ with $ r \in \mathbb Z_p $ a random blinding factor is

@divend


`
$$
C(x,r) = xH + rG
$$
`

@div[text-left]

Here `$ G \in \mathbb F_p $` is a random generator point and `$ H \in \mathbb F_p $` specially chosen so that `$ x_H $` satisfying `$ H = x_H G $` cannot be found except if the EC DLP is solved. In secp256k1 $ H $ is the SHA256 hash of simple encoded $ x $-coordinate of generator point $ G $. The number $ H ​$ is what is known as a Nothing Up My Sleeve (NUMS) number.

<div class="LineHeight100per"> <br></div>

EC PCs are also additionally homomorphic, such that for messages `$ x, x_0, x_1 $`, blinding factors `$ r, r_0, r_1 $` and scalar $ k $ the following relations hold:

@divend

`
$$
\begin{aligned}
C(x_0,r_0) + C(x_1,r_1) &= C(x_0+x_1,r_0+r_1) \\
C(k \cdot x, k \cdot r) &= k \cdot C(x, r)
\end{aligned}
$$
`

@div[text-left]

A PC implementation uses three algorithms: **<code>Setup()</code>** to set up the commitment parameters $ G $ and $ H $; **<code>Commit()</code>** to commit to the message $ x $ using the commitment parameters $ r $, $ H $ and $ G ​$ and **<code>Open()</code>** to open and verify the commitment.

@divend

---

## How do Bulletproofs work?

???

---

## Applications for Bulletproofs

???

---

## Comparison to other ZK Proof Systems

???

---

## Interesting Bulletproofs Implementation Snippets

### Current & Past Efforts

???

---

### Security Considerations

???

### Wallet Reconstruction and Switch Commitment - Grin

???

---

## Conclusions

???