# Encrypting the Database

It is important to have a solution for encrypting data at rest. This is a common requirement for HIPAA and the PCI-DSS as well as other compliance regulations. Though it is possible to use **Full-disk encryption**, it is almost always the flimsiest method, only being able to protect the data from being read after physically stealing a hard disk. **Application-level encryption** is considered to be the most versatile and strongest, however, database administrators often do not have development control of the application stack that a company uses, so we are left with **Database-level encryption.** This is often seen as the middle-ground in terms of security, however there are clear downsides to using this level of encryption.

**Pros of DB-level Encryption**<br/>

- Full power of DBMS available
- Easy to implement
- Only databases can see the unencrypted data
- Per-table encryption (w/ per-table keys)

**Cons of DB-level Encryption**<br/>

- No per-user assignments available
- No mitigations against a malicious or hacked root user
- Possible performance issues (depending on application stack)

**MySQL Enterprise Edition**<br/>

This edition has the full set of tools needed for enterprises to encrypt and decrypt data as they pass through the database, below are a list of tools available to Enterprise Edition users.

- Asymmetric Public Key Encryption (RSA)
- Asymmetric Private Key Decryption (RSA)
- Generate Public/Private Key (RSA, DSA, DH)
- Derive Symmetric Keys from Public and Private Key pairs (DH)
- Digitally Sign Data (RSA, DSA)
- Verify Data Signature (RSA, DSA)
- Validation Data Authenticity (RSA, DSA)

**MySQL Enterprise Encryption allows your enterprise to**

- Secure data using combination of public, private, and symmetric keys to encrypt and decrypt data.
- Encrypt data stored in MySQL using RSA, DSA, or DH encryption algorithms.
- Digitally sign messages to confirm the authenticity of the sender (non-repudiation) and the integrity of the message.
- Eliminate unnecessary exposure to data by enabling DBAs to manage encrypted data.
- Interoperate with other cryptographic systems and appliances without changing existing applications.
- Avoid exposure of asymmetric keys within client applications or on disk.

**Database Encryption Key Management** <br/>

Following PCI-DSS, databases containing important Credit Card information must be encrypted, moreover, there must be facilities to generate secure keys, as well as encrypting the decryption keys for these databases, with a key life cycle period. In addition, the storage of key-encryptions must be separated from the database of interest.


