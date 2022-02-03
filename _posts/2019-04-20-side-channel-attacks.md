---
title: "Side-Channel Attacks"
date: 2019-04-20T15:34:30-04:00
categories:
  - Methodology
tags:
  - Hardware
toc: true
toc_label: "Navigation"
---

A side-channel attack is any attack that is based on gaining information from the physical system that a process is being implemented on. Rather than an exploitation of weaknesses in the algorithm itself or peculiarities in the code, like cryptanalysis and software bugs. These attacks can use timing information, power consumption, electromagnetic leaks, or even sounds from the device as sources of information, which can then be exploited.

Some side-channel attacks require technical knowledge of the internal operation of the system. Others, such as differential power analysis, are effective as black-box attacks, meaning that the internal processes do not need to be well known or understood by the attacker. The rise of Web 2.0 applications and software-as-a-service has also raised the possibility of side-channel attacks on the web, even when transmissions between a web browser and server are encrypted (e.g. through HTTPS or WiFi encryption).

Attempts to break a cryptosystem by deceiving or coercing people with legitimate access are not side-channel attacks, these are usually [social engineering attacks](https://en.wikipedia.org/wiki/Social_engineering_(security)).

## History

Though many people believed that side-channel attacks began a few years into the cold war era (1947 – 1991), a declassified NSA document called Tempest shows that for the U.S. they began in 1943, during WWII. At the time Bell Labs was working on the Bell Telephone model 131-B2, a top secret encrypted teletype terminal used by the U.S. to transmit wartime communications. An engineer noticed that a freestanding oscilloscope on the other side of the lab would spike at the same time that the teletype encrypted a letter. It was then discovered that the spikes could be decrypted into the entire original plain text message.
Bell Telephone told the (U.S. Army) Signal Corps, who were using the teletype for communications, about the vulnerability, but they were met with skepticism and disbelief that it could be exploited under practical field conditions. They were told to prove it. So Bell engineers were placed in a building across the street (approx. 80 feet away) from the Signal Corps cryptocenter. The engineers recorded signals from the center for an hour. Three or four hours later, they produced 75% of the plain text that had been processed by the center during the recorded time. [1]

This information seems to have then been 'forgotten' by the U.S government until 1951. Other countries such as Russia however, appear to have also discovered similar vulnerabilities and are believed to have begun implementing protections against this technique by the beginning of the cold war.

Since then various agencies around the world have been documented using similar techniques:
- In 1951, the CIA told the newly formed NSA that they had tested the Bell teletype machines and found they could read plain text from a quarter mile (400m) down the signal line.
- In 1953, the Soviets planted not just the famous 40 microphones in the U.S.'s Moscow embassy, but also seeded mesh antenna into the concrete ceiling. The theorized purpose of which being to detect leaked energy pulses. This was not discovered by the U.S. until 1964. [2]
- In the 1960s according to a former MI5 officer, the British Security Service analyzed emissions from French cipher equipment. [3]
- In 1962, the Japanese, according to NSA documents, attempted to aim an antenna placed on top of a hospital at a U.S. crypto center. The believed intent being to intercept signals from communication devices.[1]
- In the 1980s, the KGB was suspected of having planted microphones inside IBM Selectric typewriters to monitor the electrical noise generated as the type ball rotated and pitched to strike the paper; the characteristics of which could reveal which key was pressed. [4]
- In 1985, computer researcher Wim van Eck published a paper explaining how cheap equipment could be used to pick up and redisplay information from a computer monitor, hundreds of meters away. This technique has since been coined as "[Van Eck Phreaking](https://en.wikipedia.org/wiki/Van_Eck_phreaking)"

Since 1951, the U.S. has developed and refined the science of tuning into radio waves leaking from electronic equipment. The NSA also issued strict standards for shielding sensitive buildings and equipment. Those rules are now known to government agencies and defense contractors as TEMPEST, and they apply to everything from computer monitors to encrypted cell phones that handle classified information. The paper titled "TEMPEST: A Signal Problem" detailing the fundamental security vulnerability called "compromising emanations", was written in 1972 and published in the NSA's secret in-house journal "Cryptologic Spectrum". The document was not declassified until 2008. The NSA did not declassify the entire paper however, leaving the description of two separate, but apparently related, types of attacks called "Flooding" and "Seismic", redacted.

## Classes of Side-Channel Attacks
In all cases, the underlying principle is that physical effects caused by the operation of a system can provide useful information about secrets in the system.

- Cache attacks: based on an attacker's ability to monitor cache accesses made by the victim in a shared physical system. Such as a virtualized environment or a type of cloud service.
- Timing attacks: based on measuring how much time various computations take to perform.
- Power-monitoring attacks: analyze the variance in power consumption by the hardware during computation.
- Electromagnetic attacks: based on leaked electromagnetic radiation, which can directly reveal plaintexts and other information. These measurements can be used to infer cryptographic keys or they can be used in non-cryptographic attacks.
- Acoustic cryptanalysis: attacks that exploit sound produced during a computation (similar to power analysis).
- Fault analysis: secrets can be discovered by introducing faults in a computation.
- Data remanence: in which sensitive data are read after supposedly having been deleted. (i.e. Cold boot attack)
- Optical attacks: sensitive data can be read by visual recording using a high resolution camera or other devices that have such capabilities.

### Cache Attacks
Cache attacks are based on having access to the cache behavior. In some cases this is paired with timing attacks because memory accesses are not always performed in constant time, and they can also be paired with power analysis as the use of cache memory has an impact on the power consumption. Usually the data contained within the cache cannot be seen directly and instead the areas of memory being accessed is all that the attacker can see. However there have been vulnerabilities that allowed an attacker to leak the memory contents of other processes and the operating system itself, like the [meltdown](https://en.wikipedia.org/wiki/Meltdown_(security_vulnerability)) and [spectre](https://en.wikipedia.org/wiki/Spectre_(security_vulnerability)) vulnerabilities. [5]

An example of a typical cache attack is a flush + reload attack in which the attacker can flush a cached memory location, then wait and re-access the location. If the victim has accessed the location the re-access time for the attacker is fast, if the victim has not it is slow.

### Timing Attacks
Timing attacks exploit the data-dependent behavioral characteristics of the implementation of an algorithm. Every logical operation performed by a computer takes some amount of time to execute, and this can vary based on the data being operated on, as well. Using precise measurements of the time required by each operation, an attacker can work backwards to recover the input.
The attacker can get timing information by measuring something like the time it takes for the system to respond to specific queries. How much useful information about the system that the attacker can get will depend on the accuracy of the measurements, the design of the system, the CPU running the design, the algorithms used, whether any countermeasures have been implemented, and other implementation details. Due to the nature of these types of attacks they are usually only seen in research conditions and rarely (if ever) 'in the wild'. As such avoidance of timing attacks is rarely considered when designing systems. Though timing attacks can still be important because they can be applied remotely through a network, while other side-channel leakages require the attacker to have gained some physical contact with the target. [6]

#### For Example:
In 2003, two Stanford University professors demonstrated a practical network-based timing attack on SSL-enabled web servers, based on another vulnerability having to do with the use of RSA with [Chinese remainder theorem](https://en.wikipedia.org/wiki/Chinese_remainder_theorem) optimizations. The network distance was small in their experiments, but the attack successfully recovered a server private key within a matter of hours. This demonstration led to the widespread deployment and use of blinding techniques, which are mentioned in the countermeasures section, in SSL implementations. In this context, blinding is intended to remove correlations between the key and encryption time. [7]

### Power-Monitoring Attacks
These types of attacks can provide detailed information about processes being run by observing the power consumption of a hardware device such as a CPU or cryptographic circuit. In a microprocessor when different instructions are performed the power consumption of the processor changes, creating a unique pattern of power profiles that reveal the operations that have been performed. As an example, in a power trace from a smart card (chip card) using DES encryption, the sixteen rounds of encryption can be clearly seen. Similarly, squaring and multiplication operations in RSA implementations can often be distinguished, enabling an attacker to compute the secret key. Even if the magnitude of the variations in power consumption are small, standard digital oscilloscopes can easily show the data-induced variations. Frequency filters and averaging functions, which are built into oscilloscopes, are often used to filter out high-frequency components. These attacks are roughly categorized into simple power analysis (SPA) and differential power analysis (DPA)

SPA involves visual examination of power traces usually collected by monitoring the current used by a device over time. Variations in power consumption occur as the device performs different operations.

DPA is a more advanced form of power analysis, which can allow an attacker to compute the intermediate values within cryptographic computations through statistical analysis of data collected from multiple cryptographic operations.

Power analysis attacks cannot generally be detected by a device, since the attacker's monitoring is normally passive. In addition, the attack is non-invasive. As a result, physical enclosures, auditing capabilities, and attack detectors are ineffective. Instead, engineers must ensure that device's power variations do not reveal information usable by attackers.

### Electromagnetic Attacks
These types of attacks require measuring the amount of electromagnetic radiation being emitted from a device and then performing signal analysis on those measurements. These attacks are both non-invasive and passive in nature, as they are performed by monitoring the normal behavior of the target. The techniques mentioned in the 'History' section of this document are types of electromagnetic attacks, and they are sometimes referred to as TEMPEST, Van Eck Phreaking, or radiation monitoring attacks.
Every wire that carries current creates a magnetic field because of Ampere's Law, and so electronic devices create some small magnetic fields when in use. The strength of these magnetic fields can unintentionally reveal information about the operations being performed on a device or the data being used by it. The term ‘device’ can refer to anything from a desktop computer, to mobile phone, to a smart card.
Like power monitoring attacks these too can be broken up into simple analysis, where the encryption key or other information is deduced directly from the trace. Or differential analysis, which uses comparisons to deduce the information.

#### For Example:
Smart credit cards (chip cards) contain embedded integrated circuits the perform cryptographic functions when connected to a card reader that provides the power to the circuit and the clock. Tampering with the card reader or placing a false one over the real one has been proven an effective method of obtaining the information on the card. [8]
By using an external USB sound card and an induction coil salvaged from a wireless charging pad, researchers were able to extract a user's signing key in Android's [OpenSSL](https://en.wikipedia.org/wiki/OpenSSL) from a cellphone. [9]
LCD screens are suceptable to Van Eck Phreaking because the amount of electricity passing through the crystals controls the colour that they display, when this happens electromagnetic radiation is released, and with an antenna and decoder you can create a replica of what someone has on their screen.

### Acoustic Cryptanalysis
This type of attack exploits the sounds emitted by devices such as keyboards and internal computer components. In the past this has also included things like [impact printers](https://en.wikipedia.org/wiki/Printer_(computing)) and electromechanical deciphering machines like [enigma](https://en.wikipedia.org/wiki/Enigma_machine) or [bombe](https://en.wikipedia.org/wiki/Bombe), though many different devices have been susceptible to this type of attack. Modern computers are often quite quiet to the human ear, but the microphone of a nearby cellphone can still pickup noise made by the processor. Some computer components make enough noise that they have even been used to make music, for instance when the Mars Curiosity Rover sang itself ['Happy Birthday'](https://youtu.be/uxVVgBAosqg?t=80) by running specific operations of it's soil sampling instruments, or a [youtuber](https://youtu.be/Oym7B7YidKs?t=14) who has recreated various songs from pop culture.
Processors can create acoustic emissions when under stress.  The power consumption of devices cause heating, which is then offset by cooling effects. These temperature changes create thermally induced mechanical stress, which can create low level acoustic emissions from operating CPUs (about 10 kHz in some cases). These emissions are not the human-audible humming of a cooling fan, but ultrasonic noise from the capacitors and inductors in the motherboards. Acoustic emissions occur in coils and capacitors because of small movements in the components when a current surge passes through them. Capacitors in particular change diameter slightly as their many layers experience electrostatic attraction/repulsion or piezoelectric effects.

#### For Example:
In 2004, researchers at the IBM Almaden Research Center announced that computer keyboards and keypads used on telephones and ATMs are vulnerable to acoustic attacks because of the sounds produced by different keys. The researchers employed a neural network to recognize which key had been pressed, and then they could record the information entered by users. These techniques could allow an attacker using covert listening devices to obtain passwords, passphrases, PINs, and other information entered via keyboards. [10]
In 2013 Israeli researchers successfully implemented a timing attack based on acoustic emissions, on a laptop running an [RSA](https://en.wikipedia.org/wiki/RSA_(algorithm)) implementation, using a mobile phone located close to the laptop. [11]

### Fault analysis
The principle of this attack is to induce faults into a device's operation, with the goal of revealing to the attacker the internal states. By inducing these faults the attacker is hoping that the processor may begin to output incorrect results, potentially due to physical data corruption, which could help them to deduce the instructions that the processor is running. Some examples of environmental conditions that can influence the operation of a CPU and cause these types of faults are high temperatures, unsupported supply voltages or currents, high overclocking, strong electromagnetic fields, or even radiation. Another way of introducing a fault is with a clock-glitch. A device with an external clock that can be manipulated could be given shorter clock pulses that cause the processor to execute the next instruction before the previous one was finished or to store invalid data in a memory location. [12]

Most of the countermeasures that have been proposed against this type of attack are error detection. For instance processors that limit or halt computations above certain detected temperatures.

#### For Example:
In 2002 it was shown that in implementations of [DES](https://en.wikipedia.org/wiki/Data_Encryption_Standard) and [Triple DES](https://en.wikipedia.org/wiki/Triple_DES), the last DES round key can be found after running less than 200 cipher texts by assuming that one bit of data in one of the 16 rounds is flipped with a uniform probability. [13]
A similar attack was performed on the [RC5](https://en.wikipedia.org/wiki/RC5) cipher by introducing register faults that affected the current round and then compared the faulty result with the correct one to obtain the round key. They found the round key of the final round first, then the round key of the second last round, and so on. [13]

### Data Remanence
Data remanence is the residual representation of digital data that can remain even after attempts have been made to remove or erase said data. This may result from data being left intact by a nominal file deletion operation, by reformatting of storage media that does not remove data previously written to the media, or through physical properties of the storage media that allow previously written data to be recovered. Data remanence may make inadvertent disclosure of sensitive information possible should the storage media be released into an uncontrolled environment (e.g. thrown in the trash or lost), or if someone were to gain access to the sections where the data remnants are located.

Various techniques have been developed to counter data remanence. These techniques are classified as clearing, purging/sanitizing, or destruction. Specific methods include overwriting, degaussing, encryption, and media destruction. However, effective application of countermeasures can be complicated by several factors, including inaccessible media, media that cannot effectively be erased, advanced storage systems that maintain histories of data throughout the data's life cycle, and persistence of data in memory that is typically considered volatile.

There are many tools that have been developed to recover data deleted by conventional means. The purpose of many of these is to recover accidentally deleted data, or return the data to a previous state after being corrupted.

### Optical Attacks
The threat of optical attacks has increased recently due to the widespread use of surveillance cameras that often have little security. These could be used to try and see what's on a computers monitor, but the LEDs on a computer can also tell a lot about what the device is doing, especially because they are usually easier to read than the monitor itself. There are three classes of LED used in today's computer equipment. [14]
- Unmodulated LEDs that indicate device state, such as power on.
- Time modulated LEDs that indicate device activity levels.
- Modulated LEDs that indicate the content of the data being processed.

This also opens a channel for exfiltrating data. That is, someone gains nominal access to the computer but is able to change the operation of an LED to transmit specific data. Storage drives have microprocessors embedded in their controllers, making them hackable, and their activity LEDs have demonstrated transmission speeds up to 4kbps using surveillance cameras as optical receivers. This is fast enough to handle encryption keys, keystroke logging, and text and binary files. Since drive lights flicker during normal operation and the human eye can only observe flickering up to about 60 Hz, users are unlikely to notice any additional flickering during data transmission. [15]

## Countermeasures
Side-Channel attacks rely on a relationship between the emitted or leaked information and the secret data. So the counter method itself depends on the side-channel being used and the information that can be accessed through it.  
There are two main categories of countermeasures:
1. Eliminating or reducing the amount of information that is being released, or
2. Eliminating the relationship between the leaked information and the secret data.

All countermeasures have drawbacks that can affect the operation of the system, usually by slowing it down, as many require adding calculations or further processing to the system. So designers need to make compromises when considering the types of countermeasures to implement.

### Countermeasures Method 1
Depending on the type of side channel attack that the device needs to be protected against there are different methods that can be used to reduce the amount of information that is leaked. For example:
- [Faraday Cage](https://en.wikipedia.org/wiki/Faraday_cage) based shielding can be used to prevent electromagnetic emissions. This would reduce the susceptibility to electromagnetic attacks.
- Power line conditioning and filtering can deter power-monitoring attacks. Though some correlation can remain and compromise security.
- The design of the physical enclosure can prevent the installation of microphones for acoustic attacks, or other micro-monitoring devices.
- The side-channel can also have random noise directly added to it. A random delay on computations could deter timing attacks, though most random noise is pseudo-random and so an attacker could still use statistical computations to work around this, though it would require them to obtain more measurements.
- Designing software to be isochronous, that is, designing every operation to take the same length of time, would completely prevent timing attacks. Though it is difficult to implement.
- A countermeasure against simple power-analysis attacks is to design the software as "program counter secure", meaning that the execution path of the program does not depend on any secret values and so there are no differences in power from choosing one branch over another.
- Using memory in a predictable fixed pattern can prevent cache attacks, as well as avoiding data controlled memory accesses like table lookups as the would reveal which part of the table was accessed and thus information about the data.
- It is possible to avoid leaking data-dependent power differences by correlating the number of 1 bits in a secret value. By manipulating both the value and it's complement at the same time the power spikes can be made to be uniform and thus reveal nothing about the data itself. Although unless the balance is perfect correlation will remain.

### Countermeasures Method 2
Depending on the type of side channel attack that the device needs to be protected against there are different methods that can be used to remove the relationship between the secret data and the information. For example:
- [Blinding](https://en.wikipedia.org/wiki/Blinding_(cryptography)) is an algorithm that allows the operation to be performed on a randomized version of the data. Which 'blinds' the attacker since they have no control or knowledge over this randomized data. Usually it works like this, Alice has an input x and Bob has a function f. Alice wants Bob to compute y = f(x) without him knowing x or y to him. Alice "blinds" the message by encoding it into some other function E(x); the encoding E must be a [bijection](https://en.wikipedia.org/wiki/Bijection) on the input space of f, ideally a random permutation. Bob returns f(E(x)) to her, to which she applies a decoding D to obtain D(f(E(x))) = y.
- A countermeasure that is effective against all side-channel attacks is masking. The principle of masking is to avoid manipulating any sensitive value directly, but rather manipulate a sharing of it. A set of variables (called "shares") that xor'd together equal the original value are used. So an attacker must recover all the values of the shares to get any meaningful information. [16]

## References
<font size="-2">
[1] https://www.wired.com/2008/04/nsa-releases-se/ <br>
  
[2] https://www.nytimes.com/1964/05/20/archives/in-moscow-walls-have-ears40-us-embassy-finds-microphones-after.html  <br>

[3] https://www.sans.org/reading-room/whitepapers/privacy/paper/981  <br>

[4] http://content.time.com/time/magazine/article/0,9171,964052-2,00.html  <br>

[5] https://arstechnica.com/gadgets/2018/01/meltdown-and-spectre-heres-what-intel-apple-microsoft-others-are-doing-about-it/  <br>

[6] https://www.bearssl.org/constanttime.html  <br>

[7] http://crypto.stanford.edu/~dabo/papers/ssl-timing.pdf  <br>

[8] Quisquater JJ, Samyde D (2001). Electromagnetic analysis (ema): Measures and counter-measures for smart cards. Smart Card 
Programming and Security.  
<br>
[9] http://mostconf.org/2012/papers/21.pdf  <br>

[10] https://www.berkeley.edu/news/media/releases/2005/09/14_key.shtml  <br>

[11] http://cs.tau.ac.il/~tromer/acoustic/  <br>

[12] https://www.esat.kuleuven.be/cosic/publications/article-2204.pdf  <br>

[13] https://ieeexplore.ieee.org/abstract/document/966796  <br>

[14] https://www.securityweek.com/hard-drive-led-allows-data-theft-air-gapped-pcs  <br>

[15] https://www.zdnet.com/article/stealing-data-from-air-gapped-computers/  <br>

[16] https://www.iacr.org/archive/eurocrypt2013/78810139/78810139.pdf  <br>
</font>
