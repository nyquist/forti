# SSL to a Web Server

1. Internal Client initiates a connection to a SSL-enabled web server and sends a Client Hello, waiting for the server to respond
2. FortiGate sends the intial message to the server, but intercepts the response and reads the server certificate.
3. FortiGate internal CA generates a new key pair and a certificate with the Subject name set to the DNS name fo the website.&#x20;
4. The FortiGate sends the generated certificate signed by its own CA to the client
5. Follwing normal setup, a secure channel is established between client and FortiGate
   1. At the same time, FortiGate initiates a secure session with the web server.
6. At this point the FortiGate can decrypt data sent from both client and server in order to scan the data before re-encrypting it and sending it to the destination. This  is essentially a MITM attack
