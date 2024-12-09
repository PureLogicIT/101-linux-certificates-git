# Creating a Key Pair and Certificate Signed by a CA

This guide covers generating a key pair and certificate request that can be signed by a Certificate Authority (CA), as well as recommendations on using Subject Alternative Names (SAN), wildcard certificates, and setting appropriate expiration dates.

---

## Generating a Key Pair and Certificate Signing Request (CSR)

To obtain a certificate from your CA, you first need to generate a private key and a CSR. The private key is kept secure, while the CSR is sent to the CA for signing.

### Generate the Private Key

```bash
openssl genpkey -algorithm RSA -out myapp-key.pem -aes256
```

-algorithm RSA: Specifies the RSA algorithm for the key.
-out myapp-key.pem: Defines the file where the private key will be stored.
-aes256: Encrypts the private key with AES-256. You will be prompted to enter a passphrase for security.

### Generate the CSR

```bash
openssl req -new -key myapp-key.pem -out myapp-csr.pem -subj "/C=CA/ST=Ontario/L=Kanata/O=Company/OU=Department/CN=myapp.example.com"
```

- new: Creates a new CSR.
- key myapp-key.pem: Specifies the private key to use.
- out myapp-csr.pem: Specifies the file where the CSR will be saved.
- subj: Allows you to specify the subject fields in a single command.

|Option|Description              |Required              |
|------|-------------------------|----------------------|
| C    | Country (2 Letter Code) | No (but recommended) |
| ST   | State or Province       | No                   |
| L    | Locality ex: City       | No                   |
| O    | Orginization            | No (but recommended) |
| CN   | Common Name             | Yes                  |

### Submit the CSR to the CA

The CSR can be sent to your CA for signing. If you're using an internal CA created with OpenSSL, the CA can sign the CSR as follows:

#### Use the CA from the previous step

To use the CA from the previous step, we'll need to modify the openssl config file to tell it where to find the CA
```bash
sudo vi /usr/lib/ssl/openssl.cnf
```

Under the `[ CA_default ]` header modify the `dir`

```
####################################################################
[ ca ]
default_ca      = CA_default            # The default ca section

####################################################################
[ CA_default ]

dir             = ./my-ca               # Where everything is kept
certs           = $dir/certs            # Where the issued certs are kept
crl_dir         = $dir/crl              # Where the issued crl are kept
```

### Signing the CSR

```bash
openssl ca -in myapp-csr.pem -out myapp-cert.pem -days 365
```

- in myapp-csr.pem: Specifies the CSR file.
- out myapp-cert.pem: Defines the output file for the signed certificate.
- days 365: Sets the certificate to be valid for 1 year.

## Subject Alternative Names (SAN) vs. Common Name (CN)

The Common Name (CN) field traditionally holds the primary domain name for the certificate, but today, best practices recommend using Subject Alternative Names (SAN) for flexibility and security.

**When to Use SAN**
- Multiple Hostnames: If the certificate needs to be valid for multiple hostnames, SAN can hold multiple entries like example.com, www.example.com, api.example.com.
- Compatibility: Most browsers and services prefer SAN over CN for certificate validation.

### Adding SAN to the CSR

To include SAN in the CSR, you can use a configuration file or include the additional hostnames in the command line. 

To create with a configuration file first create a file, in this case we named it san.cnf:

```ini
[ req ]
distinguished_name = req_distinguished_name
req_extensions = req_ext
prompt = no

[ req_distinguished_name ]
CN = myapp.example.com

[ req_ext ]
email_in_dn     = no            # Don't concat the email in the DNsubjectAltName = @alt_names

[alt_names]
DNS.1 = myapp.example.com
DNS.2 = www.myapp.example.com
DNS.3 = api.myapp.example.com
```

#### Generate the CSR with SAN using config file:

```bash
openssl req -new -key myapp-key.pem -out myapp-csr.pem -config san.cnf
```

Alternatively a CSR with SAN can be created from command line arguments. 

```bash
openssl req -newkey rsa:4096 -sha256 -nodes \
  -keyout example.key -out example.crt -subj '/CN=example.com' \
  -addext 'subjectAltName=DNS:example.com,DNS:example.net,IP:1.2.3.4'
```

## Wildcard Certificates Best Practices

Wildcard certificates allow you to secure multiple subdomains under a single certificate (e.g., `*.example.com`). While convenient, they should be used with caution due to security implications.

**When to Use Wildcards**
- Internal Applications: Wildcard certificates are useful for environments where subdomains frequently change (e.g., development or staging).
- Trusted Network: Use wildcards only on internal networks or private services where risk is minimized.

**Security Considerations**
- Limit Access: Restrict access to the private key of a wildcard certificate to avoid compromising all subdomains.
- Avoid for External-Facing Services: For public applications, it’s generally better to use specific SAN entries rather than wildcards for each service.

## Expiry Date Considerations

The expiration date of a certificate impacts how often it must be renewed. Choosing the right validity period helps balance security and administrative overhead.

### Expiration Period Recommendations
- Short-Term Validity (90 days): Common for public-facing web certificates (e.g., Let’s Encrypt), which encourages frequent rotation and enhances security.
- Mid-Term Validity (1 year): Good for most internal certificates, balancing security with a reasonable renewal period.
- Long-Term Validity (up to 5 years): Suitable only for certificates used in controlled, secure environments.

### Setting the Expiration Date with OpenSSL

You can specify the expiration period when signing the certificate:

```bash
openssl ca -in myapp-csr.pem -out myapp-cert.pem -days 365
```

- days 365: Sets the certificate to expire in 1 year. Adjust this value based on your organization’s policies.
