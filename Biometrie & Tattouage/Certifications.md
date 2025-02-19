To list all available commands related to `openssl x509`, you can use the following command:

```bash
openssl x509 -help
```

This will display all available options for the `x509` subcommand in OpenSSL. Below is a breakdown of some commonly used `x509` commands:

### **Basic Commands**

- `-in <file>` → Input file (certificate to read)
- `-out <file>` → Output file to save the certificate
- `-noout` → Suppress certificate output
- `-text` → Display the certificate in human-readable format
- `-serial` → Print the certificate's serial number
- `-subject` → Display the subject (owner) of the certificate
- `-issuer` → Display the issuer (CA) of the certificate
- `-dates` → Show certificate validity period (start and end dates)
- `-pubkey` → Display the public key contained in the certificate

### **Validation & Conversion**

- `-modulus` → Print the modulus of the certificate (for checking key matching)
- `-fingerprint` → Show the fingerprint of the certificate (SHA-1, SHA-256)
- `-checkend <seconds>` → Check if the certificate will expire within a given time
- `-x509toreq` → Convert a certificate into a certificate signing request (CSR)
- `-signkey <file>` → Sign the certificate using a private key
- `-CA <file>` → Use a Certificate Authority (CA) certificate to sign another certificate
- `-CAkey <file>` → Use a CA private key to sign a certificate

### **CSR & Self-Signed Certificates**

- `-req` → Indicate that input is a certificate request
- `-new` → Generate a new self-signed certificate
- `-days <num>` → Set the certificate's validity period (used with `-new`)
- `-extensions <section>` → Specify which extensions to use from the OpenSSL config file

### **Example Usage**

1. **View Certificate Details**
    
    ```bash
    openssl x509 -in cert.pem -text -noout
    ```
    
2. **Check Certificate Expiry Date**
    
    ```bash
    openssl x509 -in cert.pem -dates -noout
    ```
    
3. **Extract Public Key from Certificate**
    
    ```bash
    openssl x509 -in cert.pem -pubkey -noout
    ```
    
4. **Generate a Self-Signed Certificate (Valid for 365 days)**
    
    ```bash
    openssl req -x509 -new -nodes -key private.key -sha256 -days 365 -out cert.pem
    ```
    

---

