---
layout: post
title:  "Network Security 101"
date:   2015-05-14
categories: [blog, security]
comments: true
---

## 1.1 What is Security

Being safe? Yes, but we are going to talk about information security which led me think of a lock.

But how to define if a message is secure?

- Confidentiality, no one ever read about it
- Integrity, message is complete, no one changed it
- Authentication, it comes from the right person

Knowing what is secure, then we can talk about being secure.

## 1.2 How to Keep a Secret

There’s a long long history of Cryptography and full of interesting stuff. 
I’m going to mention only two commonly used techniques in the past:

- Transposition cipher
- Substitution cipher

In cryptography, a **transposition cipher** is a method of encryption by which the positions held by units of 
plaintext (which are commonly characters or groups of characters) are shifted according to a regular system, 
so that the ciphertext constitutes a permutation of the plaintext. That is, the order of the units is changed.

For example, plaintext “hello world” -> ciphertext “IFSSP”

In cryptography, a **substitution cipher** is a method of encoding by which units of plaintext are replaced with ciphertext, 
according to a regular system; the “units” may be single letters (the most common), pairs of letters, triplets of letters, 
mixtures of the above, and so forth. The receiver deciphers the text by performing an inverse substitution.

![transposition-cipher](/source/img/transposition-cipher.png)

These two schemes are very similar and are easily broken using probability analysis of character.

## 1.3 How to Break a Secret

We just talk about two basic encryption scheme which are not secure. 
To build a secure system, the first thing to think of is how to break this system. 
This lead us to Threat Modeling.

> A threat model is a description of a set of security aspects; that is, 
when looking at a piece of software (or any computer system), 
one can define a threat model by defining a set of possible attacks to consider.

In other words, a threat model is a kind of attack for a specific system. OK, now let’s talk about attacks.

**Man in the middle attack**: Man stays in middle of communication channel and redirect the message

![man-in-the-middle-attack](/source/img/man-in-the-middle-attack.gif)

**Replay attack**: Send the same ciphertext more than one time

Example: A send B a message to transfer 100$ to B. B replay this message many times to get more than 100$.

Solution: Sequence number, Time Stamp

## 1.4 Kerckhoffs’s Principle

Now we know some attacks. But before going to the modern Cryptography, 
there’s one important principle we need to know, **Kerckhoffs’s principle**: A crypto-system should be secure even if everything about the system, except the key, is public knowledge. 
This means plaintext and ciphertext should not be assumed secrets during transmission.(example, telegram war)

So three stages for encryption:

- Distinguish messages -> Decrypt messages -> Learn key

## 2.1 What is Symmetric Key Encryption?

> Symmetric key algorithms are a class of algorithms for cryptography that use the same cryptographic keys for 
both encryption of plaintext and decryption of ciphertext.

The keys may be identical or there may be a simple transformation to go between the two keys. 
The keys, in practice, represent a shared secret between two or more parties that can be used to maintain a private information link.

To be simple, **two people share the same key**!

Great! Let’s look at an example: Alice & Bob both share the same key k. Alice wants to send Bob a message m. She encrypted it with key k and sent the ciphertext c. Bob got c, decrypted it and got message m!

![symmetric-key](/source/img/symmetric-key.png)

- Advantage: easy to implement, fast!
- Disadvantage: this algorithm requires that “both parties have access to the secret key”. Why it is bad to share the same key? Because two parties need to do a process called “key exchange” before communication. If someone sits in the middle and learn the key, then he knows everything!

In comparison to symmetric key encryption, there’s another algorithm called **public-key encryption**.

## 2.2. What is Public-key Encryption

> “Public-key cryptography, also known as asymmetric cryptography, refers to a cryptographic algorithm which requires two separate keys, one of which is secret (or private) and one of which is public. Although different, the two parts of this key pair are mathematically linked. The public key is used to encrypt plaintext or to verify a digital signature; whereas the private key is used to decrypt ciphertext or to create a digital signature.

> The term “asymmetric” stems from the use of different keys to perform these opposite functions, each the inverse of the other – as contrasted with conventional (“symmetric”) cryptography which relies on the same key to perform both.”

To be simple, the idea is everyone has two keys: public key and private key. Public key is known to the public and private key is only known to oneself. Now, if Alice wants to send a message to Bob, she encrypted the message with Bob’s public key and sent the ciphertext c. Bob got c, decrypted it with his private key and got message m.

More about public key will be talked in later lecture note.

## 2.3. One-time Pad & Perfect Secrecy

We just talked about symmetric key encryption and asymmetric key encryption. Both need key! Is there an algorithm that doesn’t need a key? YES! It’s one-time pad.

**What is One-time pad**?

> “Each bit or character from the plaintext is encrypted by a modular addition with a bit or character from a secret random key (or pad) of the same length as the plaintext, resulting in a ciphertext. If the key is truly random, at least as long as the plaintext, never reused in whole or part, and kept secret, the ciphertext will be impossible to decrypt or break without knowing the key.

> In cryptography, the one time pad (OTP) is a type of encryption that is impossible to crack if used correctly.”

IMPOSSIBLE to crack! Right! It’s impossible to crack! The way how I understand it is that every time we encrypt a message, we use a different key. So that no one can decrypt it because no message share the same key. And one time pad is the only algorithm which fits Shannon’s perfect secrecy.

**What is perfect secrecy**?

Perfect secrecy is, the ciphertext C gives absolutely no additional information about the plaintext.

**How to do One-time pad**?

Use stream cipher!

> “A stream cipher is a symmetric key cipher where plaintext digits are combined with 
a pseudo random cipher digit stream (keystream). 
In a stream cipher each plaintext digit is encrypted one at a time with the corresponding digit of the keystream, 
to give a digit of the ciphertext stream.”

## 2.4 Authentication

Send a message to let the other one knows who you are. There are two types:

- Based on symmetric key encryption
  - Example: email login
- Based on asymmetric key encryption
  - Example: SSL, digital signature

## 3.1 Public Key Cryptography Definition

> “Public-key cryptography, also known as asymmetric cryptography, 
refers to a cryptographic algorithm which requires two separate keys, 
one of which is secret (or private) and one of which is public. 
Although different, the two parts of this key pair are mathematically linked.

The public key is used to encrypt plaintext or to verify a digital signature; 
whereas the private key is used to decrypt ciphertext or to create a digital signature. 
The term “asymmetric” stems from the use of different keys to perform these opposite functions, 
each the inverse of the other – as contrasted with conventional (“symmetric”) cryptography which 
relies on the same key to perform both.”

![public-key-encryption](/source/img/public-key-encryption.png)

## 3.2 Usage of Public Key Encryption

Fact
- Each one has two keys: public key and private key.
- Public key is known to the world, private is known to the owner.
- Public key and private is mathematically related. But No one can compute one key even if he known the other (for now).
- The algorithm used to compute public & private key is RSA.

Use
- If A wants to send message to B, A decrypts message with B’s public key and sends the ciphertext to B. B gets the ciphertext and decrypted it with his own private key.

## 3.3 Key Exchange Protocol

Definition

- A and B use PKC & SIG to exchange a ephemeral key. 
- Then A and B communicate based on one shared key

Advantage

- Allow communication with symmetric crypto which is fast
- Lower number of keys

Disadvantage

- If someone knows B’s SK, he can recover all communications

Perfect forward secrecy

- It is a property of key-agreement protocols that ensures that a session key derived from a set of long-term keys will not be compromised if one of the long-term keys is compromised in the future.

![diffie-hellman-key-exchange](/source/img/diffie-hellman-key-exchange.png)

## Source
- Textbook: Introduction to Modern Cryptography, Jonathan Katz and Yehuda Lindell, 2007
