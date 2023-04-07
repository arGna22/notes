# Foundations

## Terminology
* Sender: Wants to send a message to the reciever securely
* Plaintext: The message itself
* Encryption: disguising a message in such a manner as to hide its substance.
* Decryption: process of turning ciphertext back into plaintext
* Cryptography: The art and science of keeping messages secure.
* Crytanalysis: the art and science of breaking ciphertext (seeing through the disguise).
* Cryptography is practiced by cryptographers
* Cryptanalysis is practiced by cryptanalysts.
* Branch of mathematics that encompasses cryptography and cryptanalysis is called cryptology
* Ciphertext is usally the same size or larger than the plaintext

## Authentication, Integrity, and Nonrepudation
* Authetntication: Reciever can verify where message came from, intruder should not be ale to masquerade as someone else
* Integrity: Reciever cna verify that the message has not been modified in transit.
* Nonrepudation: A sender hsould not be able to falsely deny later that he sent a message.

## Algorithms and Keys
* A cryptographic algorithm, also called a cipher, is the mathematical fucntion used for encryption and decryption.
* Restricted algorithm: An algorithm that the security of which is based on keeping the way the algorithm works a secret.
* If only restriced algorithms were used, then everyone would have to come up with their own ciphers and implementations
* Modern cryptography uses keys, which allow for standaredization of algorithms because the security is based in the key(s); none is based on the details of the algorithm
* Some algorithms use different encryption and decryption keys
* A cryptosystem is an algorithm plus all possible plaintexts, ciphertexts, and keys.

## Symmetric algorithms
* One of two types of key-based algorithms.
* Symmetric algorithms, sometimes called conventional algorithms
* Encryptiopn key can be calculated from the decryption key, vice versa
* Most of the time, the encryption key and decrypotion key are the same.
* Algoritms that specificlly have the same public and private key are reffered to as single-key algorithms.
* Single-key algorithms require that the sender and reciever agree on a key before they begin communication
* Security of symmetric algorithms rests on the key
* Can be divided into 2 types: stream ciphers and block ciphers

### Stream Ciphers
* Operate on plaintext a single bit, sometimes a byte at a time

### Block Ciphers
* Operate on a group of bits, called blocks, at a time.
* Typical block size is 64 bits for modern computers

## Public-Key Algorithms
* Sometimes called asymmetric algorithms
* Key used for encryption is different from the key used for decryption
* Decryption key cannot be calculated from the encryption key in a reasonable amount of time
* Encryption key can be made public
* Only specific person with corresponding decryption key can decrypt the cipher
* Public key: encryption key
* Private key: decryption key

## Cryptanalysis
* Point of cryptography is to keep plaintext (and/or key) secretfrom eavsedroppers
* Eavsedroppers are assumed to have complete access to the communicatns between the center and the reciever.
* Cryptanalysis is the science of recovering plaintext of a message without access to the key
* The key or plaintext may be recovered in successful cryptanalysis
* Loss of a key through non-cryptanalytic means is known as a compromise.
* Attempted cryptanalysis is called an attack.
* Fundemental assumption in cryptanalysis that secrecy resides entirely in the key and that the attacker knows the details of the algorithm + implementation
* Four types of crytanalytic attacks, still keeping the above assumptions
* Ciphertext-only attack
** Attacker has ciphertext of several message, all of which have been encrypted using the same encryption algorithm
** Aim is to recover plaintext of as many messages as possible, or better, deduce the key used to encrypt the messages so that other messages encrypted with the same keys can be decrypted
* Known-plaintext attack
** Access to not only ciphertexts of several messages, but also plaintext.
** Aim is to deduce keys used to encrypt or an algorithm to decrypt any new messages encrypted with the same key(s).
* Chosen-plaintext attack
** Not only has access to associated plaintext and ciphertext to messages, but also can choose the plaintext that is encrypted
** More powerful because attacker can choose specific blocks of plaintext to encrypt, yielding more information about the key.
** job of cryptanalyst is to deduce key(s) or an algorithm to dedcuce any new messages encrypted with the same key(s).
* Adaptive-chosen-plaintext attack
** Special case of chosen-plaintext
** Attacker can modify his choice of what is encrypted based on the results of the previous encryption
** In chosen-plaintext, the cryptanalyst might jsut be able to choose one large block of plaintext to be encrypted; in this attack, he can choose a smaller block of plaintext and then another based on the results of the first
* Chosen-ciphertext attack
** Cryptanalyst can choose different ciphertexts to be decrypted into plaintext
** aim is to deduce key
** primarily applicable to public-key algorihtms
** sometimes effective against symmetric algorithm
* Chosen-key attack
** cryptanalyst has some knowledge about relationship between different keys
** strange and obscure, not too practical
* Rubber-hose cryptanalysis
** Cryptanalyst threatens, blackmails, or tortures somebody until they get the key.
* Purchase-key attack
** Bribery
* Above atacks are very powerful and are often the best way to break an algorithm
* Many messages have standard beginnings and endings that might be known to the cryptanalyst
* Encrypted source code is especially vulnurable to attacks becasue of the regular appearance of keywords
* Kerckhoff's assumption: If the atrength of your cryptosystem relies on the fact that your attacker does not known the algorithm's inner-workings, you're sunk.
* If you believe that keeping the algorithm's insides secret improvdes security of your cryptosystem more than letting the academic community analyze it, you're wrong.
* if you think somebody won't disassemble your source code and reverse-engineer your algorithm, you're naive.


## Security of Algorithms
* Different algorithms offer different degrees of security, depending on how hard they are to break.
* If the cost to break an algorithm is greater than the value of the encrypted data, you're probably safe
* If the time requird to break an algorithm is longer than the time the encrypted data must remain secret, then you're probably safe.
* If the amount of data encrypted with a single key is less than the amount of data requireds to break the algorithm, then you're probably safe.
* Lars Knudsen classified the following differene categories of breaking an algorithm
** Total break: Cryptanalyst finds the key, such that D~k~(C) = P.
** Global deduction: A crypt analyst finds an alternate algorithm, A, equivalent to D~k~(C), without knowing k.
** Instance (or local) deduction: cryptanalyst finds the plaintext of an intercepted ciphertext.
** Information deduction: A cryptanalyst gains some information about the key or plaintext. This information could be a few bits of the key, some information about the form of the plaintext, and so forth.
* An algorithm is unconditionally secure if, no matter how much ciphertext a cryptanalyst has, there is not enough informtion to recover the plaintext.
* Only a one-time pad is unbreakable given infinite resources. All other cryptosystems are breakable in a ciphertext-only attack, simply by tring every possible key one byone and checking whetehr the resulting plaintext is meaningful. 
* Brute force attack: Trying every possible key one-by-one and checking whether the resulting plaintext is meaningful. This is called a brute-force attack.
* Cryptography is more concerned with cryptosystems that are computationally infeasible to break.
* An algorithm is considered computationalyl secure (sometimes called strong) if it cannot be broken with available resources, either current or future. Exactly what constitutes "available resources" is open to interpretations.
* You can measure the complexity of an attack in different ways
* 1. Data complexity: The amount of data needed as input to the attack
* 2. Processing complexity: The time needed to perform the attack. This is often called the work-factor.
* 3. Storage requirements. The amount of memory needed to do the attack.
* As a rule of thumb, the compledxity of an attack is taken to be the maximum of these three factors.
* Some attacks involve trading off of the three complexities: A faster attack might be possible at the expense of greater storage requirements.
