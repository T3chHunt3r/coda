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

This edition has the full set of tools needed for enterprises to encrypt and decrypt data as they pass through the database, unlike the Community Edition, they have the capability to manage the downsides of database-level encryption. Below are a list of tools available to Enterprise Edition users.

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


**Encrypting Columns** <br/>

In this example, we will walk through with encrypting individual columns in the sample database. Note that this is just an overview of what functions are available in the Community Edition, specific encryption solutions and policies will depend on the company use case.

1. In this example, we will encrypt the 'customerName' column in the 'customers' table for the 'classicmodels' database. So let's navigate to that database with the query `USE classicmodels;` and show 10 rows of the customers table, focusing on the customerName only with `SELECT customerName FROM customers LIMIT 10;`.

2. What we will do is first create a new column to store the encrypted versions of the customerName values, we can do this with `ALTER TABLE customers ADD encCustomerName blob;`. This statement creates a new table with the 'blob' data type, to store our binary encrypted values.

3. Before populating the new column with values, we need to set up some things such as specifying block encryption mode and creating a hash of our chosen password. We can do this with `SET block_encryption_mod = 'aes-256-cbc'; SET @key_str = SHA2('class', 512); SET @init_vector = random_bytes(16);`. In this example, we are setting the decryption password as 'class'.

4. Now we can populate the column by encrypting all customerNames with `UPDATE customers SET encCustomerName = aes_encrypt(customerName, @key_str, @init_vector);`. You can see the result with this statement `SELECT customerName, encCustomerName FROM customers LIMIT 10;`

5. To decrypt the column, use the statement `SELECT customerName, convert(aes_decrypt(encCustomerName, @key_str, @init_vector) using utf8) FROM customers LIMIT 10;`
