# Certificate-Based User Authentication

A user certificate includes:

* the user's public key
* the digital signature of the certificate, computed with the CA's private key

To authenticate with a user certificate, the authentication server (FortiGate) must have the certificate of the CA that signed the user's certificate
