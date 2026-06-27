---
title: "Keys to the Digital World"
slug: "keys-to-the-digital-world"
date: 2026-06-27T00:00:00+00:00
draft: false
summary: "Setting up a server, I delegated the configuration to an AI agent — and one unexpected step pulled me into the world of cryptography for half an hour. Here's how the locks of the digital world actually work."
categories: ["Архитектура и исследования"]
rubrics: ["Перекрестки"]
tags: []
---

Setting up another VDS for a small personal project, I decided to run an experiment. Lately I've been delegating routine tasks to AI agents more and more: compare a couple of tables, write a justification, parse some logs. The tasks are templated — and agents handle them well enough. Initial server configuration falls squarely into this category: the steps are known, the sequence is always the same, with minor variations.

I was handed a completely clean virtual machine. The standard checklist looks something like this: update packages, set the hostname, create an unprivileged user, configure sudo, disable SSH root login, switch off password authentication and leave only keys, bring up the firewall — allow needed ports, block everything else, install brute-force protection. All of this goes by the name "first ten minutes on a server" and has been copied from article to article since approximately the beginning of time.

I watched the agent work with half an eye, confirming commands — I still don't fully trust these new miraculous technologies. At some point my gaze caught something unfamiliar: configuring SSH access, I habitually type `ssh-keygen -t rsa -b 4096` into the terminal — years of muscle memory, completely automatic, no thought required.

The agent suggested something different.

I decided to look into it — and that pulled me into the world of cryptography and encryption for half an hour. I'd like to share what I found.

---

Let's start from the beginning: why do we need keys at all, if passwords work fine? It seems like you could just pick a longer password and get the same protection...

A password is a secret only you know. The logic is simple and universally understood, going back to childhood fairy tales: "Open Sesame!" The right combination, the key to the lock — and the door opens. The longer the combination, the harder it is to guess, and the stronger the protection.

But there's a non-obvious side to this. If only you know the password — it has no value or meaning. Like a key without a lock. There must always be a second party that accepts your password and opens the door for you. The password information must be distributed symmetrically — between the one who provides it and the one who receives it.

This is the first, and arguably most obvious, method of information protection ever invented by humanity — **symmetric encryption**. The idea is simple: both parties have the same key. Encrypt with it — decrypt with it. This is exactly how a password-protected archive works, or an encrypted flash drive.

{{< alert icon="circle-info" >}}
The famous hotline between Moscow and Washington, established after the Cuban Missile Crisis, ran on one-time pads until the 1980s — a classic example of symmetric encryption in action. Soviet intelligence, however, once made a fatal mistake: they began reusing supposedly one-time keys. American counterintelligence noticed this and, through Operation VENONA, spent decades decrypting KGB communications. Dozens of Soviet agents were exposed as a result — including those who had passed atomic bomb secrets.
{{< /alert >}}

Symmetric encryption works perfectly well — if both parties already have the key, or if the channel for delivering that key is completely reliable and secure.

Problems begin the moment you need to transmit that password somewhere. For instance, over the internet — across a network you don't control, through servers you don't fully trust, past people whose existence you don't even suspect. Any of them could gain access to the encryption key if it's transmitted unencrypted, in plain text. If you open a browser for the first time and visit an unfamiliar site — you have no shared secret. How do you establish one without revealing it to outsiders? This is where symmetry hits a wall.

Suppose your password is the encryption key, and you've encrypted your message with it. For the receiving party to decrypt it — you need to transmit the password to them. But to encrypt the password, you'd need yet another encryption key — which doesn't solve the problem, it just reproduces it at the next level. Here is the fundamental paradox at the root of all modern cryptography: **to establish a secure connection, you need a secure channel — which doesn't exist yet.**

The answer appeared in 1976, and it was revolutionary. Whitfield Diffie and Martin Hellman proposed an idea that forms the foundation of modern asymmetric cryptography: **what if encryption and decryption used different keys?**

{{< alert icon="circle-info" >}}
The story behind asymmetric cryptography is remarkable in itself. Diffie — a self-taught mathematician with no academic position — spent years driving across the country looking for people who thought about cryptography. The NSA, learning of the upcoming publication, tried to block it — the idea seemed too revolutionary. It didn't work.

But here's a little-known fact: British intelligence agency GCHQ had independently discovered the same concept three years earlier, in 1973. Mathematician Clifford Cocks developed the algorithm we now know as RSA literally during a lunch break — by his own account, in about twenty minutes. The result was immediately classified. The world didn't learn about it until 1997.
{{< /alert >}}

And so **asymmetric cryptography** was born — two mathematically linked keys. One is **public**: you can hand it out to anyone, publish it in a newspaper ad, transmit it in the open. The other is **private**: kept only by the owner and never transmitted anywhere. In terms of analogy: imagine I hand out open padlocks to everyone — unlocked, no key included. Anyone can take such a padlock, put a letter inside, and snap it shut. Only I can open it, because there is one key and it stays with me.

This is exactly what happens every time you see the lock icon in your browser's address bar. Your browser and the server "exchange padlocks" and agree on a temporary shared secret — in a way that makes it practically impossible to intercept the process and obtain the contents. It all happens in fractions of a second, completely invisible to the user.

The same mechanism powers SSH for connecting to servers. End-to-end encrypted messengers. Electronic document signing. Bitcoin. Asymmetric cryptography is the invisible foundation on which the entire digital world stands.

Now for the most interesting part: how does it actually work? Where does the mathematical magic come from that allows encryption so that only the private key owner can decrypt?

Everything rests on one mathematical idea — the **one-way function**. This is an operation that's easy to perform in one direction and practically impossible to reverse. Mixing two paint colors is easy. Separating the resulting color back into its components — impossible. Various one-way functions can be used; in practice, a few well-established algorithms have proven themselves.

**RSA**

Multiplying two prime numbers is a trivial task. Take 17 and 19, you get 323. Fast, simple, almost any schoolchild can do it.

Now the reverse: you're given the number 1147 and asked to find its two prime factors. Already harder — you have to try combinations. The answer, by the way, is 31 × 37.

Now imagine the number isn't three digits, but 600 digits long. Multiplying two large primes of that scale is trivial for any computer. Factoring the result back — that's a task which, at today's computing power, would take time comparable to the age of the universe. This asymmetry is what RSA is built on. The public and private RSA keys are mathematically derived from a pair of large primes. The public key is their product, which you can hand out freely. The private key is knowledge of the factors themselves, which enables decryption. As long as no one can quickly solve the factorization problem — the system holds.

In 1977, the algorithm's authors published a challenge in Scientific American: factor a 129-digit number — RSA-129. Their estimate: 40 quadrillion years. In 1994, it was solved in eight months — using distributed computing across 1,600 computers around the world, connected via the internet. The authors slightly underestimated the pace of progress.

Hence the need for long keys. The larger the number, the longer factoring takes. 1024-bit keys were the norm in the 2000s — that's no longer considered safe. 2048 bits is the recommended minimum today. 4096 bits — the key length I'd habitually generate, without a second thought.

**Elliptic curves**

Mathematicians long noticed that RSA isn't the only option. You can build a cryptosystem on entirely different mathematical problems — ones that are even harder to crack, with keys significantly shorter.

Elliptic curves are a family of mathematical objects of the form:

y² = x³ + ax + b

They look like smooth, symmetric lines, and it's not immediately obvious what they have to do with encryption. But points on such a curve have an interesting property: they can be "added" according to special rules.

Take a point G on the curve and add it to itself d times. You get point H. This operation is fast. The reverse: given G and H, find d — is **computationally infeasible** for a sufficiently large d.

This is where the key difference from RSA lies: the mathematical problem on elliptic curves is significantly harder than factorization. This means that for an equivalent level of protection, a much shorter key is needed. A 256-bit elliptic curve key is roughly equivalent to a 3072-bit RSA key. And 32 bytes is exactly 256 bits.

This is why Ed25519 isn't just "a different algorithm." It's different mathematics — more elegant and more efficient. The same security guarantees, a key sixteen times shorter, faster operations. And as a bonus — a deterministic signature: the algorithm doesn't require a high-quality random number source for each operation, unlike some predecessors that failed precisely because of this.

The number 25519 in the name is not arbitrary. It's the exponent in the number 2²⁵⁵ − 19, modulo which the curve operates. A prime of this form was chosen deliberately: arithmetic with it runs especially efficiently on modern processors.

The Curve25519 algorithm was developed in 2005 by Daniel Bernstein — a mathematician and cryptographer little known to the general public, but a near-cult figure in professional circles. He created ChaCha20 — a stream cipher that today protects your traffic in modern TLS and QUIC — and Poly1305, a data integrity algorithm. Together they form the ChaCha20-Poly1305 suite, which Google once promoted as an alternative to AES on mobile devices where hardware AES acceleration isn't available. Today this combination runs on billions of devices daily. In 1995, Bernstein sued the US government, challenging export control laws on cryptography — and won. Following that ruling, American cryptographic software stopped being classified as a military export.

In 2013, the Snowden documents confirmed what had long been whispered: the NSA had deliberately embedded backdoors in cryptographic standards. The most scandalous case — the Dual_EC_DRBG random number generator, adopted by NIST as an official standard in 2006. Bernstein had publicly flagged its suspicious parameter structure back in 2007. After Snowden, it was clear he had been right. According to Reuters, the NSA paid RSA Security $10 million to make it the default generator in their products. NIST withdrew the standard. RSA Security apologized to its customers.

The standard NIST curves — P-256, P-384, P-521 — also raise questions. Their parameters are derived from some "random" seed value whose origin has never been officially explained. In cryptography, this is called the "nothing up my sleeve" principle — used to describe algorithms where every number has an open and verifiable justification. The NIST curves don't meet this criterion. Curve25519 does, fully. This is precisely why the community adopted it as an alternative with no unexplained numbers. And why Bernstein's work is trusted so deeply by those who work in security professionally.

Ed25519 arrived in OpenSSH version 6.5 — in January 2014. Twelve years ago. All of this passed me by. In 2014 I was generating RSA keys without a second thought. In 2024 — same. And in 2026. Muscle memory proved stronger than curiosity. Though it's probably too early to declare RSA obsolete and displaced by Ed25519. More accurately: the modern cryptographic landscape offers several options, with a few clear leaders among them:

**RSA** — a veteran turning 48 this year. Described in 1977, named after its authors: Rivest, Shamir, Adleman. It works. Battle-tested. Supported literally everywhere — from smart cards to the most modern HSMs. The main downside is weight: longer keys are needed for acceptable security, operations are slower, traffic is heavier. On constrained devices — IoT, smart cards, embedded systems — this matters. The undeniable advantage is near-universal support.

**ECDSA** — an attempt to take the best of both worlds: elliptic curve mathematics with a familiar interface. Short keys, high resilience. Widely used in TLS, Bitcoin, and document signing. There's one catch: the algorithm requires high-quality randomness for each signature.

{{< alert icon="circle-info" >}}
In 2010, Sony made a fatal mistake in the PlayStation 3 firmware: the same "random" number was used for every signature. Mathematically, this is a catastrophe — given two signatures with the same random value, the private key can be recovered by simple algebra. Hackers did exactly that. PS3 piracy became trivial. One predictable byte — and the entire cryptographic protection collapsed.
{{< /alert >}}

**Ed25519** — elliptic curves in Bernstein's implementation: deterministic signatures with no dependence on random numbers, transparent parameters, a 32-byte key, and speed that significantly outpaces RSA. On a modern processor, Ed25519 can verify tens of thousands of signatures per second on a single core. Supported in OpenSSH since 2014, in modern TLS, and in most current cryptographic libraries. The de facto standard for new code.

The choice might seem obvious. But the story doesn't end there.

**On the horizon — quantum computers.** Both RSA and elliptic curves rest on one assumption: that exhaustively searching all options in reasonable time is impossible. A quantum computer running Shor's algorithm breaks this assumption — it solves the factorization problem in a fundamentally different way, turning exponential time into polynomial time. Peter Shor described this algorithm back in 1994 — thirty years before machines capable of running it existed. To break RSA-2048, a quantum computer would need roughly four thousand logical qubits with error correction — which in practice means millions of physical qubits. The best machines today have around a thousand physical qubits with high error rates. Still far off. But intelligence agencies are already collecting encrypted traffic as a stockpile — planning to decrypt it later when the technology catches up. This is the approach known as "harvest now, decrypt later."

In 2024, NIST approved the first **post-quantum standards**: ML-KEM and ML-DSA — based on lattice cryptography, a class of problems quantum algorithms can't solve faster than classical ones. The competition to develop them launched in 2016, received 69 submissions from around the world, and took eight years to conclude. Migration has already begun: major browsers, cloud providers, and government systems are gradually adding support for the new algorithms.

Cryptography is not a frozen monument. It's a living race between those who build locks and those who try to break them. Ed25519 is perhaps the best lock available today. What will be best in ten years — that question remains open.

---

Having studied all this theory at some point in the past, I always generated RSA keys on autopilot, never once asking myself: is what I'm doing actually right?

Maybe I wasn't that wrong. The world simply found something better in the meantime — and I missed it.

In the end, having consumed several megabytes of information and spent a certain amount of time satisfying my curiosity, I agreed with the agent and pressed Enter — confirming the generation of an elliptic curve key.

```
ssh-keygen -t ed25519
```

What would you have done in my place?
