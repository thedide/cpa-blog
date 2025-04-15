---
title: "Quantum Resistant Encryption"
date: 2025-04-13T21:29:32-07:00
categories:
  - Technology
tags:
  - Encryption
  - Quantum Computing
  - Post-Quantum Cryptography
  - PQC
  - RSA
  - Lattice-Based Cryptography
  - Multivariate Polynomial Cryptography
  - Hash-Based Cryptography
  - Isogeny-Based Cryptography
---

# The Quantum Threat to RSA: And What Comes Next

In the world of digital security, few acronyms carry as much weight as RSA. Since its creation in 1977, RSA encryption has served as a foundational technology for securing emails, digital signatures, online banking, and virtually every corner of our digital lives. It is based on a beautiful and elegant idea: multiplying two very large prime numbers is easy, but factoring their product is extremely hard. This assumption has held true for over four decades—but a new challenger is emerging that threatens to break this bedrock of digital security: the **quantum computer**.

---

## Part I: Why Quantum Computers Threaten RSA

### The Mathematics Behind RSA

RSA relies on a mathematical trapdoor. Here’s a simplified version of how it works:

1. Choose two large prime numbers: `p` and `q`.
2. Multiply them together to get `n = p × q`.
3. Choose a public exponent `e`, and compute a private exponent `d` such that `e × d ≡ 1 (mod φ(n))`, where `φ(n) = (p−1)(q−1)`.

The public key is `(e, n)` and the private key is `d`. Anyone can encrypt messages using the public key, but only someone with the private key can decrypt them.

The strength of RSA lies in the difficulty of **factoring** a large number `n` to retrieve its original prime factors `p` and `q`. Classical computers take an impractical amount of time to do this when `n` is large enough—e.g., 2048 bits.

### Enter Quantum Computing

Quantum computers aren’t just faster classical computers. They operate on a different principle altogether, harnessing the strange and powerful behaviors of quantum mechanics like **superposition** and **entanglement**.

In 1994, mathematician Peter Shor showed that a sufficiently powerful quantum computer could **factor large integers exponentially faster** than the best known classical algorithms. Shor’s algorithm, if implemented on a large-scale quantum machine, would reduce the time complexity of factoring from sub-exponential to polynomial time.

This means a task that would take **centuries** on a classical computer could potentially be done in **minutes or hours** on a quantum one.

### How Shor’s Algorithm Breaks RSA

Shor’s algorithm works by transforming the problem of factoring into a **period-finding** problem, which quantum computers are particularly good at solving. Here's the basic idea:

1. Choose a random number `a` < `n`.
2. Find the **period** `r` such that `a^r ≡ 1 mod n`.
3. Use this period `r` to find a non-trivial factor of `n`.

This is where the quantum magic happens. Finding the period `r` is extremely hard for classical machines—but a quantum computer can do it efficiently using the **Quantum Fourier Transform**.

Once the period is found, classical post-processing steps yield the prime factors of `n`. Game over.

### How Big Would a Quantum Computer Need to Be?

To break a 2048-bit RSA key, a quantum computer would need:

- Roughly **4000–6000 stable, logical qubits**
- Along with **millions of physical qubits** to support error correction

As of 2025, we are still far from building such a machine. Current quantum computers have **tens to low hundreds** of noisy qubits. But progress is accelerating, and experts estimate that a cryptographically relevant quantum computer could arrive in **10–20 years**—or possibly sooner.

The bottom line: **RSA, as we know it, will not survive the quantum era**.

---

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- cpa -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-2843564932689995"
     data-ad-slot="3526097725"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## Part II: Post-Quantum Cryptography – What Comes Next?

The looming threat of quantum computing has led researchers to develop **Post-Quantum Cryptography (PQC)**—encryption methods that are believed to be secure even against quantum adversaries. Unlike RSA and ECC, these schemes are not broken by Shor’s or Grover’s algorithms.

Here’s a breakdown of the most promising quantum-resistant cryptographic methods.

---

### 1. Lattice-Based Cryptography

Lattice-based crypto is built on the difficulty of solving problems like the **Shortest Vector Problem (SVP)** or the **Learning With Errors (LWE)** problem in high-dimensional spaces. These problems are believed to remain hard even for quantum computers.

#### 🌟 Notable Algorithms:
- **Kyber**: A key encapsulation mechanism (KEM) selected by NIST for standardization
- **Dilithium**: A digital signature scheme, also selected by NIST
- **FrodoKEM**: Another lattice-based KEM with conservative design choices

#### ✅ Pros:
- Fast key generation and encryption/decryption
- Relatively small key and ciphertext sizes (especially for Kyber)
- Well-studied security assumptions

#### ⚠️ Cons:
- Some implementations require careful tuning to avoid side-channel attacks

Lattice-based cryptography is currently the **front-runner** in the race for post-quantum standards.

---

### 2. Code-Based Cryptography

Code-based cryptography relies on the difficulty of decoding a general linear error-correcting code—a problem that’s been studied since the 1970s and is still unbroken.

#### 🌟 Notable Algorithm:
- **Classic McEliece**: A KEM that uses Goppa codes

#### ✅ Pros:
- Long-standing security track record (over 40 years!)
- Resistant to known quantum attacks

#### ⚠️ Cons:
- **Huge public keys**—often hundreds of kilobytes to megabytes
- Less efficient than lattice-based methods in constrained environments

Still, McEliece is a strong candidate for high-security environments where key size isn’t a bottleneck.

---

### 3. Multivariate Polynomial Cryptography

This approach is based on the difficulty of solving systems of multivariate quadratic equations—a problem known to be NP-hard and quantum-resistant.

#### 🌟 Notable Algorithms:
- **Rainbow**: A digital signature scheme (broken in 2022)
- **GeMSS**: Still under evaluation

#### ✅ Pros:
- Very fast signing and verification (in theory)

#### ⚠️ Cons:
- Many schemes have been broken
- Large signature or key sizes
- Less mature than lattice or code-based schemes

Although promising, this category has suffered setbacks due to recent cryptanalysis.

---

### 4. Hash-Based Cryptography

Hash-based signatures rely purely on the security of cryptographic hash functions like SHA-256, which are still believed to be safe against quantum attacks (though Grover’s algorithm provides a square-root speedup).

#### 🌟 Notable Algorithm:
- **SPHINCS+**: A stateless, hash-based signature scheme selected by NIST

#### ✅ Pros:
- Very strong security guarantees
- Simple design with few assumptions

#### ⚠️ Cons:
- Large signature sizes
- Slower than other schemes

Hash-based crypto is particularly good for **firmware signing** and applications where long-term security matters more than speed.

---

### 5. Isogeny-Based Cryptography

This exotic field involves finding "isogenies" (morphisms) between elliptic curves—a problem believed to be hard for quantum computers.

#### 🌟 Notable Algorithm:
- **SIKE** (Supersingular Isogeny Key Encapsulation)

#### ⚠️ Status:
- SIKE was broken in 2022 using a classical attack, casting doubt on the viability of isogeny-based crypto

While still an active research area, isogeny-based schemes are currently not front-runners due to recent vulnerabilities.

---

## Part III: What's Actually Being Adopted?

In 2022, the **U.S. National Institute of Standards and Technology (NIST)** selected the first batch of quantum-resistant cryptographic algorithms for standardization:

### 🥇 NIST-Selected Algorithms:

| Use Case           | Algorithm   | Category               |
|--------------------|------------|------------------------|
| Key Encapsulation  | Kyber      | Lattice-Based          |
| Digital Signatures | Dilithium  | Lattice-Based          |
|                    | SPHINCS+   | Hash-Based             |
|                    | Falcon     | Lattice-Based (compact)|

These algorithms will gradually replace RSA and ECC in systems like HTTPS, VPNs, digital signatures, email encryption, and more.

---

## Part IV: What You Should Do Now

The shift to post-quantum cryptography isn’t an overnight flip of a switch—it’s a multi-year transition. But preparation should start now, especially for organizations that:

- Handle long-term secrets (e.g., medical, legal, military)
- Rely on public-key infrastructure (PKI)
- Use embedded systems where updates are difficult

### 🔐 Best Practices for Today:

- Use **hybrid encryption**: Combine classical (e.g., RSA or ECC) and quantum-resistant (e.g., Kyber) algorithms during the transition phase.
- Monitor developments from **NIST**, **Open Quantum Safe**, and **industry standards bodies**
- Inventory your cryptographic dependencies and design a migration path

---

## Warp Up

Quantum computing holds great promise for science, materials, medicine, and optimization—but it also poses a **fundamental threat to current cryptography**. RSA, a pillar of modern digital security, will not survive the quantum age. But that doesn’t mean we’re doomed. A vibrant ecosystem of quantum-resistant algorithms is emerging, and standards bodies like NIST are already taking action.

