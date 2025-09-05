# Verifying SSL Certificates Using OpenSSL

This guide covers key commands for verifying SSL certificates using OpenSSL. You’ll learn how to view certificate details, check a certificate’s validity against a CA and a private key, and work with Base64-encoded certificates.

---

## Viewing Certificate Information

To view basic information about a certificate, such as its name, expiry date, and issuing authority, use the following command:

```bash
openssl x509 -in certificate.crt -text -noout
```

**Explanation of Arguments**
- in certificate.crt: Specifies the certificate file to read.
- text: Outputs the certificate information in a human-readable format.
- noout: Prevents printing of the raw certificate data.

**Example Output:** You’ll see details like:

|Keys       | Description                                                                |
|-----------|----------------------------------------------------------------------------|
|Subject    | The entity the certificate is issued to (common name, organization, etc.). |
|Issuer     | The certificate authority that signed the certificate.                     |
|Validity   | The start and end dates for the certificate’s validity period.             |
|Public Key | The public key associated with the certificate.                            |

## Checking Certificate Validity Against a Certificate Authority (CA)

To verify if a certificate is signed by a specific Certificate Authority (CA), you can use the following command:

```bash
openssl verify -CAfile ca-cert.pem certificate.crt
```

**Explanation of Arguments**

- CAfile ca-cert.pem: Specifies the CA certificate file to use for verification.
- certificate.crt: The certificate to verify against the CA.

This command will return an output indicating whether the certificate is valid and was issued by the CA specified.


## Checking Certificate Validity Against a Private Key

To verify if a certificate matches a specific private key, you can compare the modulus of both. If the modulus values are identical, the certificate and key match.

1. Get the md5 value of the cert
```bash
openssl x509 -noout -modulus -in myapp-cert.pem | openssl md5
```


2. Get the md5 value of the key
```bash
openssl rsa -noout -modulus -in myapp-key.pem | openssl md5
```

If the MD5 hash outputs are identical, the private key and certificate are paired. If not, they do not match, and you may need to find the correct certificate or key file.

## Converting and Using Base64-Encoded Certificates

Certificates are often stored in Base64-encoded formats like .pem. To convert a DER-encoded certificate to Base64 (PEM) format and vice versa, use the commands below.

### Converting PEM to DER

```bash
openssl x509 -inform pem -in myapp-cert.pem -outform der -out myapp-cert.der
```

### Converting from DER to PEM

```bash
openssl x509 -inform der -in myapp-cert.der -outform pem -out myapp-cert2.pem
```


