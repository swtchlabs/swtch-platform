# Architecture

## Platform Architecture
The SWTCH Platform is organized across multiple logical contexts.

### Core Contexts

- File Format 
- Encryption Standards
- SWTCH Protocol
- Multi-Chain SDK
- Verifiable Proof of Service
- Decentralized Network Infrastructure Components

## Platform Architecture Diagram
The SWTCH Platform Diagram 
[INSERT HERE]

## File Format
A new file structure and packaging format has been created to suit our decentralized network for file storage and sharing.

#### SWTCH File Format
A standard SWTCH file structure is composed of the following key properties:

- version: The version of the file format.
- ownership: Stores the owner(s) of the file as hexadecimal hashes generated using Secp256k1.
- data: Splits the data into smaller chunks for easier distribution across the network.
- signature: Contains a digital signature of the file contents for verifying authenticity.
- nonce: A unique value to prevent replay attacks, ensuring each file instance is unique.
- metadata: An optional field for storing additional metadata about the file.
- access_control: Lists the public keys of users who have access to the file.
- hash: A cryptographic hash of the file's contents for data integrity checks.
permissions: An optional field specifying permissions associated with the file (e.g., read, write, share).
- encryption_info: An optional field detailing the encryption method used for the file.
- vec: Stores the vector representation of the file's data for search capabilities in a vector database.
- vec_info: Describes how the vector embeddings were created (e.g., method like word2vec, tokenizer used).
- merkle_root: The root hash of the Merkle tree, used for verifying the integrity of the data chunks.
- modified: Records the timestamp of when the file was last modified.

## Encryption Standards
SWTCH networks utilize end-to-end (E2E) encryption and secure all data at rest.

The SWTCH Platform employs two primary encryption methods:

- ECIES: This method leverages the efficiency and compact key sizes of Elliptic Curve Cryptography (ECC). ECIES is popular in modern applications such as blockchain technology, secure messaging, and IoT devices. It combines ECC with symmetric encryption mechanisms to enhance security while maintaining performance.

- Quantum Resistant: This method uses hybrid cryptography, combining post-quantum cryptographic algorithms with traditional public key algorithms (such as RSA or elliptic curves). Our hybrid approach ensures the encryption is resistant to both classical and potential future quantum computer attacks, providing a security level at least equivalent to existing traditional cryptographic methods.

#### ECIES Keypair
Keypair generation involves the use of ECIES (Elliptic Curve Integrated Encryption Scheme).

- Algorithm Basis: ECIES is based on elliptic curve cryptography (ECC), which leverages the mathematics of elliptic curves over finite fields. It integrates public-key cryptography for secure encryption.
- Key Generation: In ECIES and ECC, key generation involves selecting a point on an elliptic curve and choosing a private key, which is a random number. The public key is derived by multiplying this private key by a generator point on the curve. The difficulty of ECC is ensured by the elliptic curve discrete logarithm problem (ECDLP).
- Key Sizes: Elliptic curve keys can be much smaller than RSA keys while providing comparable security. For example, a 256-bit ECC key is considered roughly equivalent in security to a 3072-bit RSA key.
- Performance: ECC offers better performance and lower resource consumption than RSA, making it suitable for environments with limited computational power, storage, or bandwidth. This is one reason why ECC, and by extension ECIES, is favored for mobile devices and IoT applications.

#### Quantum KEM Keypair
Quantum KEM keypair generation offers a suite of available quantum encryption algorithms:

- BikeL1
- BikeL3
- BikeL5
- Kyber512
- Kyber768
- Kyber1024
- NtruPrimeSntrup761
- FrodoKem1344Aes
- FrodoKem1344Shake
- ClassicMcEliece348864
- ClassicMcEliece348864f
- ClassicMcEliece460896
- ClassicMcEliece460896f
- ClassicMcEliece6688128
- ClassicMcEliece6688128f
- ClassicMcEliece6960119
- ClassicMcEliece6960119f
- ClassicMcEliece8192128
- ClassicMcEliece8192128f

#### Quantum Cipher Suites
SWTCH uses AES (Advanced Encryption Standard), ChaCha20, and XChaCha20 cryptographic cipher suites to secure data. Each has unique characteristics and use cases. Here's a brief overview:

###### AES (Advanced Encryption Standard):
- Type: Symmetric key cipher
- Key Sizes: 128, 192, or 256 bits
- Block Size: 128 bits
- Usage: AES is widely used in security protocols such as SSL/TLS for securing websites, Wi-Fi encryption (WPA2 and WPA3), and more. AES has been extensively analyzed and is regarded as secure against all attacks except for brute force, which is mitigated by using larger key sizes.

###### ChaCha20:
- Type: Stream cipher
- Key Size: 256 bits
- Nonce Size: 64 bits traditionally (32 bits used in practice for protocols like TLS)
- Usage: ChaCha20 is known for its high speed and strong security profile, especially in software implementations where AES-NI (hardware acceleration for AES) is not available. It is used in applications like TLS encryption (from version 1.3 onwards) and for securing VPNs (e.g., Google's use in Chrome's HTTPS connections). Designed by Daniel J. Bernstein, it is favored for its efficiency and security.

###### XChaCha20:
- Type: Stream cipher
- Key Size: 256 bits
- Nonce Size: 192 bits
- Usage: XChaCha20 extends ChaCha20 to support a larger nonce size, beneficial for applications requiring non-repeating nonces over a greater number of messages without frequent re-keying. This makes it suitable for high-volume applications or situations where unique nonces cannot be practically guaranteed. XChaCha20 is used in various cryptographic libraries and applications focused on privacy and security.