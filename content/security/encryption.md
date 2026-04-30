---
title: "Cryptography fundamentals"
tags: ["cryptography", "security"]
draft: true
---


# Core concepts

| Concept                 | Meaning                                  | Example                             |
| :---------------------- | :--------------------------------------- | :---------------------------------- |
| Confidentiality         | Only authorized identities can read data | AES encryption for files            |
| Integrity               | Ensure data has not been altered         | SHA-256 hash for verifying download |
| Authentication/Identity | Verify who created/sent data             | Digital Signatures                  |
| Non-repudiation         | Sender cannot deny sending data          | Signing with private key            |

# Fundamental Cryptographic Classes


| Class                | Purpose                 |
| :------------------- | :---------------------- |
| Hashing              | One-way integrety proof |
| Symmetric encprytion | Fast secrecy            |
| Asymmetric encry     | Identity and Trust      |
| Digital Signatures   | Authenticity            |

## Hashing - Integrity

> Irreversible fingerprint


| Algorithm       | Use                         |
| :-------------- | :-------------------------- |
| SHA-256         | JWT certificates blockchain |
| SHA-512         | password hashing            |
| SHA-3           | New generation              |
| BLAKE2 / BLAKE3 | High‑speed hashing          |
| MD5             | x-obsolete                  |
| SHA-1           | x-broker                    |

Used for:
- password storage
- data integrity
- digital signatures
- blockchain
- proof-of-work

## Symetric Encryption - Confidentiality

> The same key encrypt and decrypts

Algorithm, Use
AES-128/256, World Standard
ChaCha20, Mobile & TLS
3DES, x-obsolete
Blowfish, legacy
Twofish, alternative

Used for:
- Disk encryption
- HTTPS payload
- VPN tunnels
- Databases

## Asymmetric encryption ( public/private keys )

> Public encrypts, private decrypts

| Algorithm      | Use                |
| :------------- | :----------------- |
| RSA-2048/4096  | TLS, certificates |
| ECC(Curve25519, P-256) | Modern TLS
| Diffie-Hellman | Key exchange       |

Used for:

- HTTPS handshake
- Certificates
- Secure messaging
- Software signing

## Digital Signatures - Integrity + Authentication + Non-repudiation

> Proves who created data & that it wasn't changed

| Algorithm   | Use            |
| :---------- | :------------- |
| RSA-PSS     | Certificates   |
| ECDSA       | Bitcoin.TLS    |
| Ed25519     | Modern Signing |
| HMAC-SHA256 | JWT HS256      |

Used for:

- JWT
- Software updates
- Blockchain
- Certificates
- Secure APIs

# Encryption vs Signing vs Hashing


| Feature          | Encrypt | Sign | Hash |
| :--------------- | :------ | :--- | :--- |
| Hides data       | Yes     | No   | No   |
| Proves author    | No      | Yes  | No   |
| Tamper detection | Partial | Yes  | Yes  |
| Reversible       | Yes     | No   | No   |



# Real world example
### HTTPS


- RSA/ECC → identity
- Diffie‑Hellman → session keys
- AES/ChaCha20 → encrypt traffic
- SHA‑256 → integrity