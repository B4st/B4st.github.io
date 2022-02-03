---
title: "Old School Cryptography"
date: 2020-04-05T13:54:00-04:00                  
categories:
  - Cryptography
tags:
  - Encryption
toc: true
toc_label: "Navigation"
---

> Cryptography, the methodology of concealing the content of messages, comes from the Greek root words kryptos, meaning hidden and graphikos, meaning writing.

## Common Notation

**Bob and Alice** = Two people trying to communicate securely
**Eve** = Attacker trying to learn what Bob and Alice are communicating to each other (eavesdropping)

## Caesar Cipher / Shift Cipher

All that is required for this method of encryption is knowledge of the alphabet of the language being used. Each letter is changed to another in the same alphabet. The letters they are changed to are chosen by their position in the alphabet. A "Caesar's Cipher" shifts the letter up or down the alphabet by a set amount, say 5 letters right, then a = f, b = g, ... and so on. The alphabet loops, so that z = e.

// To-do: add cipher wheel image or interactive

*Problem?:* it takes Eve very little time to decrypt. All she needs to do is try each of the 26 possible shifts (for the english alphabet).

## Simple Substitution Cipher

A slightly more complex replacement scheme, you can swap pairs of letters for each other. This can be viewed as a rule or function where every letter in the domain is assigned another in the range (if pairing the letters then plaintext is x values and cipher y values such that (a, C), (b, I), (c, S) ... and the definition of domain is all x values, range all y).

{a, b, c, ..., z} &larr; {A, B, C, ..., Z}

one to one / injective = a property of encryption such that no two plaintext letters go to the same cipher text letter.

To encrypt the message typically all the spaces are removed, every letter is swapped out according to the encryption table (a small table with plaintext letters on top and their substitution underneath) and then the encrypted message is broken up into groups of five letters. Decryption is the same process using either the encryption or decryption table (same as encryption except the top is cipher letters and the bottom is plaintext).

### Cryptanalysis

**Cryptanalysis** = The process of decrypting a message without knowing the underlying key

*How many different substitution ciphers exist?*

If we start with A, there are 26 possible values (A letter could be subbed for itself as otherwise the search space is actually smaller and thus less secure).

B can then be assigned to any of the 25 remaining letters (cannot be assigned the same as A as this cipher requires injectivity)

So the possible ways to assign A and B are 25 * 26 = 650. Expanded it will be:
  26·25·24···4·3·2·1 = 26! = 403291461126605635584000000 > 10<sup>26</sup>

A computer able to check one million cipher alphabets per second, it would take more than 10<sup>13</sup> years to try them all. Yet they are possible to break.

> The security of an encryption system depends on the best known method to break it. As methods are developed, the level of security can only get worse, never better.

The weakness of the substitution cipher is the nature of languages themselves, the letter frequency of the language is specific and can be used to uncover which letters have been used to represent the most frequent letters of the language like e, t, a, o, and n. There is also *bigram* frequency, which is letter pairs, that can be used for similar analysis.

Steps to break:

1. Assign the highest frequency letters to E, T, A, O, N, R (could assign more if cipher text is long)
2. Perform frequency analysis of bigrams comparing cipher text pairs to english letter pair frequency
3. Sub in the suspected letter substitutions and attempt to guess common letter words where possible (use common letter groupings to help)
4. Continue matching letters from inspection using frequency to guide as necessary.
