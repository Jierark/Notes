Useful Terms, Tools, and the Likes

# MAC Address
A physical address tied to network interfaces on a device. Can be used to uniquely distinguish devices on a network, or to distinguish between different network interfaces.

# IP Address
An address assigned to devices on a network to route packets to the correct destinations. There are two forms of IP addresses in use:
- IPv4 uses 32 bits, separated into 4 bytes
- IPv6 uses 128 bits, separated into 8 groups of 4 hex digits
# DNS
DNS provides a seamless way to map human-legible domain names to corresponding IP addresses. Essentially, each part of a domain name (ie. mail.google.com) can be successively looked up in DNS records to find the final destination's IP address. There are many types of DNS records, ranging from email servers to records that map domain names to other domain names.

# WHOIS
Provides information about a domain name, such as the domain owner, creation date, and update date.

# HTTP(S)
Protocol for website communication. The S indicates that TLS is being used on top of HTTP, providing security. Typically uses TCP ports 80 (443 for HTTPS).
Commonly used methods/commands:
## GET
Retrieves data from a server 
## POST
Submits data to the server, such as a form or a file
## PUT
Create a new resource on the server or overwrite existing information
## DELETE
Delete a specific file or resource on the server

# FTP
Protocol for file transfer. Listens on TCP port 21 by default.
# SMTP
Protocol for sending email, or for communication between email servers. Listens on TCP port 25 by default.
# POP3
Protocol for downloading email onto local clients. Listens on TCP port 110 by default.
# IMAP
Protocol for synchronizing mail across multiple devices. Listens of TCP port 143 by default.
# TLS
Transport Layer Security adds a level of security by ensuring that a third party listening on a network cannot read or edit the application data of a packet. Since it operates at the transport layer, this adds security for any protocols operating above it (for example, HTTPS, FTPS). Each "secure" version, by default, operates on different ports than its insecure version (probably for legacy reasons)
# SSH
Protocol to access remote systems, while ensuring security of the connection. Most commonly seen as OpenSSH, an open-source implementation by OpenBSD developers. Replaces the insecure telnet and listens on TCP port 22.
# VPN
Virtual Private Network establishes a "secure channel" between two networks, such that a user on one network can operate as if they were operating on the other network. Commonly used for remote workers to access company resources, to "change" geographical locations or to prevent ISPs from seeing traffic.

[[Networking Fundamentals]] 

# RSA
Public key encryption algorithm, relying on the difficulty of factoring large numbers to stay secure
Procedure:
- Choose two prime numbers p, q and calculate their product n = p x q
- Calculate ϕ(n) = n - p - q + 1, and choose e such that e is relatively prime to ϕ(n) (meaning they share no common factors except 1). Choose d such that e x d = 1 mod ϕ(n)
	- d is considered the multiplicative inverse of e, mod ϕ(n)
- The public key is (n,e), and the private key is (n,d)
- To send a message m, compute c = m^e mod n
- To decrypt a ciphertext c, compute m = c^d mod n
- There's a giant proof for why it works that I can't be bothered to recall but it's very cool to read.

# Diffie-Hellman Key Exchange
An asymmetric key exchange where both parties then agree on a shared secret key.
Procedure:
- A & B agree on a prime number p and generator g where 0 < g < p.
- A & B choose a private integer a & b that is kept secret.
- A sends g^a mod p, while B sends g^b mod p (Key exchange)
- A receives the message from B and computes (g^b)^a mod p, and B similarly computes (g^a)^b mod p, which are equal.
