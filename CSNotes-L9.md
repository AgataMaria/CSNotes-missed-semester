Official Notes & Lecture Video: [https://missing.csail.mit.edu/2020/security/](https://missing.csail.mit.edu/2020/security/)

## TL;DR & personal favourites :icecream:  
- 4 word password is easier for humans to remember and harder for computers to guess than what we normally think of as a $ecuR3Pa55w0rd. All hail the _correct horse battery staple_
- Hash functions take any input and use an algoorithm to produce a fixed size output, that's deterministic. Crypto hash functions strive to be irreversible and collision resistant
- NERDOUT here: [Lifetimes of cryptographic hash functions](https://valerieaurora.org/hash.html)  
- KDFs are slow, and slow is right sometimes :turtle:  
- To keep something private you can use a good passphrase + KDF + key generated through that for encryption and then safely store it anywhere :exploding_head: - easy to do - see [example](#Encryption example)  


---

## Security 

### Entropy :twisted_rightwards_arrows:
Entropy is a measure of randomness. It's measured in bits. Important for things like **password security**.  
To measure entropy you need to take into account the number of possible outcomes, but also what you're trying to protect against - if the attacked knows the model of the password the entropy score will change.  
Entropy of ~40bits is enough for online attacks, for offline attacks a stronger password like ~80 bits would be more appropriate.

### Hash functions :sparkles:
They are functions that change data of arbitrary size to output of a fixed size. Hash functions are **deterministic algorithms**, which means they will always produce the same output, given the same input.  
An example is SHA1, which maps any input into 40 hexadecimal characters.  
Git hashes commits (the whole file structure) in their entirety and gives a hex hash number used to identify each commit. If you hashed the directory in the exact same state you would get the same hash value, which is got Git knows there has been no changes if you'd try to commit again without making any :) Magic. (Git uses SHA1, not magic)  
You can pipe files or stdin to `sha1sum` to get the SHA1 hash. Cool.  

Cryptographic hash functions are also meant to be **irreversible** and **collision resistant** (hard to naturally find two different inputs that will produce the same hash).  
Recommended resource -> [lifetimes of crypto hash functions](https://valerieaurora.org/hash.html)  

**Commitment schemes** - you can use hash functions to commit a value without revealing the actual value itself - and present only the hash. The receipient can then compare the value you claim you initially committed with the hash. (like in the heads/tails game)

- Own notes:
Hash functions use algorithms that change data in such way that, the same input will always offer the same output. Good hash algorithms would be difficult to reverse - the ideal would be a hash algorithms that makes it impossible to guess the input from the output.  
Take passwords - string data - if they were hashed and salted (hashed value + injected random string of characters) and the final value was stored, if you type in your password, after hashing and salting it, the server only needs to compare the salted hash of your password, to what is has stored in the db, without knowing the original input. If you see a service that confirms your actual password when you register or try to recover it RUN AWAY - this would mean that they are storing the actual password string instead of a hash value. If there was a breach and anyone got hold of the stored password, they would know your actual password, which is super risky.  
Computerphile has a beautiful explanation of hashing algorithms - [video here (8 min)](https://www.youtube.com/watch?v=b4b8ktEV4Bg)  
And another one on how NOT to store passwords (this is from 2013, but the idea is the same) - [video here (9 min)](https://www.youtube.com/watch?v=8ZtInClXe1Q)  

### Key derivation functions (KDFs) :turtle:  
Similar to hash functions, but **slow to compute** (which is good for defence against attackers, as slows  them down. An example is PBKDF2 - takes an input, gives an output. Mainly used for producing keys from passphrases for use in other crypto algorithms and storing login credentials (with salt). :salt:  

### Symmetric cryptography :suspect:
**Encryption** - used for hiding content (like messages)  
**Symetric key crypto functions** have:
- keygen function (randomised)  
- encrypt function (takes plaintext + key, returns ciphertext)  
- decrypt function (takes ciphertext + key, produces plaintext)  

You can encrypt your files before you save them to the cloud and use the key to decrypt them after you retrieve them :) The keygen function has high entropy, so unless someone knows your keys it would be difficult to decrypt it. It's easy to forget the key, so you can pass a passphrase through a KDF and create a key that way. You don't need to remember the key - you can reconstruct it from the KDF.

#### Encryption example
You can encrypt a file with AES encryption, using [OpenSSL](https://www.openssl.org/): `openssl aes-256-cbc -salt -in {input filename} -out {output filename}`  
Decrypt it with `openssl aes-256-cbc -d -in {input filename} -out {output filename}`   

> Side note: `cmp` is a neat tool for comparing the files - `cmp FILE_1 FILE_2` + `echo $?`  





