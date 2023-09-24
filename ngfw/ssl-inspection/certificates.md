# Certificates

Certificates are used in different situations:

* traffic inspection - when inspecting SSL encrypted connections using a "man-in-the-middle" approach
* privacy - when connecting oever SSL encrypted connections to other devices
* authentication - when certificate are used to authenticated users that request access to the network (VPN) or to the management interface (2FA)

A digital certificate is a document produced and signed by a CA. It identifies an entity (person, device, etc). Fortigate supports the X.509v3&#x20;

The certificates contains a list of fields. :

* **Subject** field - expressed as&#x20;
* **SAN (Subject Alternative Name)** field
* **Subject Key Identifier**
* **Authority Key Identifier**

Fortigate checks the following before trusting a certificate:

* Checks the CRLs locally to verify if the certificate has been revoked by the CA
* Reads the Issuer field and checks if it has the Issuer's certificate. FortiOS uses the Mozilla CA certificate store
* Verifies that the certificate validity by comparing the date with the **Valid From** and **Valid To** fields
* Verifies the signature of the certificate
  * The **signature** on the certificate is the result of encrypting the **original hash result** of a certificate with the private key of the CA. The **original hash result** is a hash of the content of the certificate.
  * Fortigate uses the same hashing function on the content of the certificate producing a new hash result
  * Then it decrypts the signature of the certificate using the public key of the CA and compares the decrypted value (should be the original hash result) to the new hash result. If the two values are identical, then the signature is verified

## Self Signed SSL Certificates

By default, FortiGate uses a self-signed SSL certificate. but because these certificates do not come pre-installed in client certificate stores, end users get a securty warning when accessing resources presenting a self-signed certificate.



