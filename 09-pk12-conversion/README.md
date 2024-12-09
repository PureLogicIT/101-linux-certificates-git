# Working with PKCS #12 (.p12) Files

## What is PKCS #12?

PKCS #12 (Public-Key Cryptography Standards #12), often referred to by its file extension `.p12` or `.pfx`, is a binary format for storing cryptographic objects. It is primarily used to bundle a private key with its corresponding public key certificate, often including intermediate certificates as well.

- **Common Use Cases**: PKCS #12 files are commonly used in applications that require both the certificate and private key, such as for setting up HTTPS on a web server, VPNs, and email encryption (S/MIME).


## Converting to PKCS #12

To create a PKCS #12 file from a certificate and private key, you can use the `openssl` cli.

### Converting to PKCS #12

```bash
openssl pkcs12 -export -out myapp-cert.p12 -inkey myapp-key.pem -in myapp-cert.pem -certfile my-ca/ca-cert.pem
```

**Explanation of Arguments**
- export: Specifies that OpenSSL should export a new PKCS #12 file.
- out certificate.p12: Defines the output file (in this case, certificate.p12).
- inkey privateKey.key: Points to the private key file that corresponds to the certificate.
- in certificate.crt: Specifies the certificate file to include in the .p12 file.
- certfile intermediate.crt: Adds an intermediate certificate to complete the chain of trust.

This command will prompt you to set a password for the `.p12` file, which will be required when importing the file into other systems.

## Converting from PKCS #12
To extract the certificate, private key, and any intermediate certificates from a .p12 file, use the following commands.

```bash
openssl pkcs12 -in myapp-cert.p12 -nocerts -out myapp-key3.key -nodes
openssl pkcs12 -in myapp-cert.p12 -clcerts -nokeys -out myapp-cert3.crt
```

- in certificate.p12: Specifies the input .p12 file.
- nocerts: Excludes certificates from the output (only the private key will be extracted).
- out privateKey.key: Defines the output file for the private key.
- nodes: Prevents the private key from being encrypted, allowing it to be saved as plain text.

If the `.p12` file contains intermediate certificates, they can be extracted using:

```bash
openssl pkcs12 -in myapp-cert.p12 -cacerts -nokeys -out ca.crt
```

