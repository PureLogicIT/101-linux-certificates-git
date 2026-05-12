# Creating a Certificate Authority (CA) Using OpenSSL

This guide introduces the basics of a Certificate Authority (CA) and provides instructions for setting up and installing a CA using OpenSSL.

---

## What is a Certificate Authority (CA)?

A **Certificate Authority (CA)** is a trusted entity that issues digital certificates, which are used to verify the authenticity of websites, software, and other digital entities. A CA signs certificates for end users, servers, or other entities, enabling secure communication via SSL/TLS. Organizations can create their own CA to issue and manage certificates for internal use, ensuring secure connections within private networks.

Use Cases:

- **Internal Networks**: For signing certificates used on internal applications.
- **Development & Testing**: For generating certificates in test environment without relying on an external CA.
- **Client Certificate Authentication**: For generating and signing certificates used in mutual (two-way) SSL/TLS authentication.

---

## Creating a Certificate Authority (CA) with OpenSSL

Follow these steps to set up a simple Certificate Authority using OpenSSL.

Start by connecting via SSH to `pki-server`.

### 1. Set Up a CA Directory Structure

It’s a best practice to organize your CA files into a directory structure. This example assumes a folder named `my-ca` to store files.

```bash
mkdir -p my-ca/{certs,crl,newcerts,private}
chmod 700 my-ca/private
touch my-ca/index.txt
echo 1000 > my-ca/serial
```

`chmod 700` is done so that the private keys can only be accessed by the user, and no one else. 

Explanation of the Structure:

- `certs/`: Directory to store issued certificates.
- `crl/`: Directory for certificate revocation lists (optional).
- `newcerts/`: Directory for new certificates.
- `private/`: Directory to store the CA’s private key (restricted access).
- `index.txt`: File that tracks issued certificates.
- `serial`: File that holds the serial number for certificates (starting at 1000).

### 2. Generate the Private Key for the CA

```bash
openssl genpkey -algorithm RSA -out my-ca/private/cakey.pem -aes256
```

- `-algorithm RSA`: Specifies the algorithm for the private key.
- `-out my-ca/private/cakey.pem`: Defines the output file for the private key.
- `-aes256`: Encrypts the private key with AES-256 encryption for security.

The RSA algorithm is what creates the key pair, and are what will be used to sign certificates. AES-256 is the algorithm used to protect the keys themselves, and is what uses your password you entered to encrypt and decrypt your keys. 

You will be prompted to create a passphrase, which should be securely stored.

**Note: This private key will be used to sign certificates and must be protected carefully.**

### 3. Generate the Self-Signed CA Certificate

```bash
openssl req -new -x509 -key my-ca/private/cakey.pem -out my-ca/cacert.pem -days 365
```

- `-new`: Generates a new certificate request.
- `-x509`: Creates a self-signed certificate.
- `-key my-ca/private/cakey.pem`: Specifies the CA’s private key.
- `-out my-ca/cacert.pem`: The output file for the CA certificate.
- `-days 365`: Specifies the certificate’s validity period (1 year in this case).

You will be prompted to fill in certificate details like country, organization name, and common name (e.g., "My Company CA"). These fields identify your CA.

Fill the fields with the following info:

- Country Name (2 letter code) [AU]:`CA`
- State or Province Name (full name) [Some-State]:`Ontario`
- Locality Name (eg, city) []:
- Organization Name (eg, company) [Internet Widgits Pty Ltd]:`PL workshop`
- Organizational Unit Name (eg, section) []:
- Common Name (e.g. server FQDN or YOUR name) []:`pl workshop CA`
- Email Address []:

## Distributing the CA Certificate

Now that the `cacert.pem` file has been generated, we want to distribute it to the machine that will need to trust our CA. 

**Note: Never share the `private/cakey.pem` file, only the `cacert.pem` file should ever be shared.**

On `workstation`, copy the certificate from `pki-server` to `webserver`, and to `workstation`. 

```bash
ssh webserver "mkdir ~/my-ca"
scp pki-server:my-ca/cacert.pem webserver:~/my-ca/
mkdir ~/my-ca
scp pki-server:my-ca/cacert.pem ~/my-ca
```

Now that the file is copied off of `pki-server`, you can install it on `webserver` and `workstation`. 

## Installing a New CA Certificate

To use the newly created CA to sign certificates, install the CA certificate on `workstation` and `webserver`. The process varies by operating system.

### For Linux Systems

Copy the CA Certificate to the system’s certificate directory:

#### Debian/Ubuntu

```bash
sudo cp my-ca/cacert.pem /usr/local/share/ca-certificates/my-ca-cert.crt
```

**Note:** The file extension must be `.crt` and the file must be in `PEM` format. 

Update the Certificate Store:

```bash
sudo update-ca-certificates
```

The output of the command should contains the line `1 added, 0 removed; done.`

#### Fedora/Red Hat

```bash
sudo cp my-ca/cacert.pem /etc/pki/ca-trust/source/whitelist/ca-cert.pem
```

Update the Certificate Store:

```bash
sudo update-ca-trust
```

This command adds the new CA certificate to the list of trusted CAs on the system. After completion, applications will recognize certificates signed by this CA as valid.

**Note:** Some Linux distributions may have slightly different directories for storing CA certificates. Refer to your system documentation if needed.

## For Windows Systems

Open the Certificate Manager by running certmgr.msc.
Import the CA Certificate:

- Go to Trusted Root Certification Authorities > Certificates.
- Right-click and select All Tasks > Import….
- Choose your CA certificate file (e.g., my-ca/cacert.pem).
Complete the import wizard to add the CA certificate to the trusted root authorities.

## For MacOS Systems

Open Keychain Access (Applications > Utilities > Keychain Access).\
Import the CA Certificate by selecting File > Import Items.\
Set the CA as “Always Trust” under the Trust settings for the imported certificate.
