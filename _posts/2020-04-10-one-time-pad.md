---
title: "One-Time Pad"
date: 2020-04-10T22:54:00-04:00                  
categories:
  - Cryptography
tags:
  - Encryption
---

I hear about this things a lot when looking into cryptography, usually in reference to the possibility of perfect security. So what is a one time pad?

## The Basic Idea

Alice has a plaintext message that she wants to send to Bob, and doesn't want anyone else to read it. Standard encryption scenario. So she decides to use a substitution cipher. Every letter in the plaintext is shifted to another, but the standard "caesar" cipher is breakable through brute force (that is shifting all the letters by the same amount). So she goes to the "Vigenere cipher" shifting the letters by different amounts based on a key. This too is breakable through cryptanalysis (if there is enough cipher text).

But the longer the key the less likely it is that someone could break the cipher. In fact if the key was as long as the plaintext then it would be the most secure right? Because there would be no pattern of shifts for an attacker to find.

In fact if the key is:

1. truly random,
2. at least as long as the plaintext,
3. never reused in whole or in part, and
4. kept completely secret

Then the cipher text could never be decrypted. (This has been proven by Shannon's 1945 paper "Communication Theory of Secrecy Systems" and independently by Vladimir Kotelnikov though this apparently remains classified)

## Example

The Vigenere cipher works as shown:

|   Message         | PlainTextMessage |
|-------------------|------------------|
|     Key           | keykeykeykeykeyk |
| Message Numerical | 15 11  0  8 13 19  4 23  19 12 4 18 18 0  6  4 |
|   Key Numerical   | 10  4 24 10  4 24 10  4  24 10 4 24 10 4 24 10 |
| Modular Addition  | (message + key) mod 26 for each letter |
| Cipher Text       | TjkmlDivdQccwyqi |

The one-time pad:

|   Message   | PlainTextMessage |
|-------------|------------------|
| Random Pad  | siwuejfvisvhsdhm |
| Cipher Text | HTWCRCJSBEZZKDNQ |

So now the message has been encrypted perfectly (except that everyone reading this knows the key).

## Security

### Simple Proof of Security

This isn't really a proof but displays the issue with trying cryptanalysis.

Eve intercepts the message HTWCRCJSBEZZKDNQ (from before) and tries to decrypt it without the key. Say she tries to brute force the result. The problem that she's going to run into is that it could be any message that is 16 letters long, for example:

|   Message   | HiThereEveFriend |
|-------------|------------------|
| Random Pad  | ALDVNLFOGAUICZAN |
| Cipher Text | HTWCRCJSBEZZKDNQ |

This is perfectly valid, and Eve would have no way to determine which is the true message/key combination.

### Perfect Secrecy

In OTP the cipher text C gives no information about the original plaintext. This is because of the proof before, since the pad is truly random and only used once a cipher text can be transformed back into any plaintext message of the same length, each one equally likely. This makes the OTP immune to brute force attacks, even when part of the plaintext or pad is known, because the attacker cannot gain any knowledge of the rest of the pad from comparing the plain text section and cipher text.

## New Methods

This classic "vigenere style" is what the KGB did, putting one time use pads on pads of paper made of material that would burn instantly without leaving any ash behind. The agents would just need some mental arithmetic and a good memory to get the message.

Now it can be implemented with a program, using data files as the "plaintext" input, the "cipher text" output, and pad (the required random sequence). The XOR operation is usually used to replace the modular addition. There are some issues with implementing these systems however.

## Implementation Problems

Though proven to be perfectly secure there are a lot of problems with translating that to real world implementations.

1. Truly random pads are required, as opposed to pseudorandom, which is a non-trivial thing.
2. The pad has to be securely generated and exchanged.
3. The pad cannot be reused in whole or in part.

### Truly Random

The random generation functions that are used most often in programming are actually pseudorandom and unfit for cryptographic purposes. Even those that are random they can be using unproven crypto functions. One example of truly random value generation is measuring radioactive emissions. True random number generators exist, but are typically slow and quite specialized, often making them impractical for common cryptographic purposes.

### Secure Key Exchange

If a communication channel existed that could perfectly securely transmit the pad to the recipient then encryption does not actually need to be used. Because the message could be transmitted over that same channel and be perfectly secure. The pad has the same qualities as the message and is the same length.

This though is assuming that such a channel exists, which for all intents and purposes it does not. There are ways that things can be assumed very secure, but often to do so is very inconvenient. The pads are typically longer than other encryption method keys and OTP does not have the key exchange methods supplied by modern public key encryption protocols. This means that the pad could be carried around on a USB drive for example which carries the risk of everyday pickpockets or others simply stealing the physical device.

On top of all of that there is a scaling issue. Sure OTP for a message 16 characters long doesn't seem hard but most encryption in use is encrypting a lot more than that. On top of which everyone in the system is using it so for large systems having a pad the size of every message sent is unreasonable.

### Secure Key Disposal

An issue with the OTP is that the pad cannot be reused in whole or in part. Thus the pad must be securely disposed of after use. This is surprisingly non-trivial when using computer media, often assured deletion of the pad would essentially require destruction of the storage device.

## Authentication

There is actually another reason that OTP is not commonly used and that is authentication. There are some authentication methods that can be used with OTP such as adding an authentication code to the message, but that needs to be actively added to the system, and would also require transmission of the authentication code, bringing back many of the issues of key exchange.

Authentication is a large part of encryption because it is important to be able to trust that you've received the message from the correct person.
