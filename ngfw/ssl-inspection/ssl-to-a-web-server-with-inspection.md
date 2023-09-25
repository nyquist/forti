# SSL to a Web Server

1. Client sends a hello message to the server
   1. includes SSL version and the cryptographic algortihms that it supports
   2. In the client hello, the SNI (Server Name Indication) field includes the hostname of the SSL server, but this field is not always sent
2. Server responds with the chosen SSL version and cipher suite, along with its certificate
3. Client validates the server certificate and if it is valid, the SSL handshake continues
4. &#x20;Client generates a premaster secret and encrypts it with the server's public key before sending it to the server. This info can only be decrypted with the server's private key which should only be known to the server.
5. When the server receives the message, it will decrypt it with it's private key and now the client and the server know the value of the premaster secret.
6. From the premaster secret, the master secret is derived by both client and server
7. From the master secret, both client and server generate the session keys which are used for symmetric encryption of data exchanged between them. Symmetric encryption is much more efficient for large ammounts of data than asymmetric (PKI) encryption.
8. As a last step, server and client exchange a summary (digest) of the messages sent so far, encrypted with the session key. This ensures that none of the messages exchanged have been intercepted or replaced, so if the digests match, the secure communication channel is established.
