# SSH

What happens when you ssh into a server

<!-- toc -->

## Concepts

NOTE: most of this section is directly from [digitalocean's community tutorial](https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process)

### SSH

Secure Shell is a cryptographic network protocol for operating network services securely over an untrusted network[^1].

### Symmetric Encryption

- One key can be used for both encypt and decrypt.
- Symmetric keys are used by SSH in order to encrypt the entire connection.
- The key is created through a **key exchange algorithm**.
- The key is created prior to authenticating a client, once it is created, rest communication are all encrypted using this key.
- AES, Blowfish, 3DES etc. different symmetrical cipher systems can be used for the symmetric encryption.

### Asymmetric Encryption

This part is mainly from [^2]

- **public/private asymmetrical key pairs that can be created are only used for authentication, not the encrypting the connection**.
- public key, can be freely shared with any party.
- private key, can be encrypted using a passpharse.
- public key encrpyt messages that can only be decrypted by the private key.

More about public/private key can be found in https://en.wikipedia.org/wiki/Public-key_cryptography#Description ,
you can use private key to signature the message.

#### Key exchange: used to setup the symmetric encryption

- TODO： But even though it uses the same underlying principles as public key cryptography, this is not asymmetric cryptography because nothing is ever encrypted or decrypted during the exchange. [^4]
- both client and server generate temporary key pairs and exchange public key in order to produce the shared secret.

#### SSH key-based authentication

- client creates a key pair and add the public key to server's `~/.ssh/authorized_keys`.
- after symmetric connection is established, server use the public key in `authorized_keys`.
to encrypt a challenge message to the client, if the client can prove it was able to decrypt the
message, it has demonstarted that it owns the associated private key.
  - FIXED: how does the server know which public key to use? ~~I think client send its public key to the server?~~ It tries every public key.
  - `Turns out, it tries a bunch of keys that it finds in the .ssh dir. Basically tries whatever's in there` [^3].
  - TODO: The client begins by sending an ID for the key pair it would like to authenticate with to the server [^2].

### Hashes

This part is mainly from [^2]

- Using the same hashing function and message should produce the same hash; modifying any portion of the data should produce an entirely different hash
- **hashes are mainly used for data integrity** purposes and to verify the authenticity of communication
- The main use in SSH is with **HMAC**, or **hash-based message authentication codes**
- Researchers generally recommend this method of encrypting the data first, and then calculating the MAC.

## Procedure

- Establish connection
- Key exchange -> Encrypt following message
- User authentication
- Spawn environment

### Establish connection

- same as how browser establish TCP connection, see [TCP three-way handshake in request life cycle](./request-life-cycle.md)
- after TCP connection establish, server reponds with the protocol version it supports [^2],
if client support one of it, then the connection continues.
- server also provide its public host key, which is why you see a warning ask you [Y/N]
for trust the host and add it to `known_hosts`
- TODO: how to disable the warning for host in Ansible

````
The authenticity of host '[dboubi.me]:3022 ([192.102.16.31]:22)' can't be established.
ECDSA key fingerprint is SHA256:lES9TW/cF59/sH2oCnn0ZSCo+2Ezmd0Z/T/Gd2BiaFs.
ECDSA key fingerprint is MD5:9d:db:e1:c7:5a:c6:7e:a4:ae:57:c7:54:3c:54:00:38.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[dboubi.me]:3022, ([192.102.16.31]:22' (ECDSA) to the list of known hosts.
````

### Key exchange

Notation: C for Client, S for server

> At this point, both parties negotiate a session key using a version of something called the Diffie-Hellman algorithm
> This algorithm (and its variants) make it possible for each party to combine their own private data with public data from the other system to arrive at an identical secret session key

The classic **Diffie-Hellman** Algorithm

NOTE: [^2] says Key exchange is asymmetric encryption while [^4] says it is NOT asymmetric cryptography because nothing
is ever encrypted or decryptted during the exchange

> But even though it uses the same underlying principles as public key cryptography, this is not asymmetric cryptography because nothing is ever encrypted or decrypted during the exchange. It is, however, an essential building-block, and was in fact the base upon which asymmetric crypto was later built.

NOTE: the explaination in [^2] does not explain why the generated key is same for both ends,
while [^4] does.

TODO： TBD

### User Authentication

- publikc key is tried before fallback to password

#### Password

weak for brutual force attack, so it's recommend to disable password login

````
# /etc/ssh/sshd_config
PasswordAuthentication no
````

#### SSH key pairs

- Client put its public key into Server's `~/.ssh/authorized_keys`
- The client begins by sending an ID for the key pair it would like to authenticate with to the server.
  - TODO: this violate [^3], it says the server just try every one....
  - TODO: my `ssh -vvv` shows client public key is sent to the server
- Server use the public key to encrypt a message and sent to client
- Client decrypt the message using its private key, and sent hash of the message encrypted using the shared session key
- Server calculate the hash and compare, if it matches, then the client is authenticated

````
debug1: Offering RSA public key: /home/at15/.ssh/id_rsa
debug3: send_pubkey_test
debug3: send packet: type 50
debug2: we sent a publickey packet, wait for reply
debug3: receive packet: type 60
````

### Spawn Environment

- some (not all) current user's environment variables are sent to the server

````
debug1: Sending env LC_CTYPE = en_US.UTF-8
debug2: channel 0: request env confirm 0
debug3: send packet: type 98
debug3: Ignored env LSCOLORS
debug3: Ignored env VAGRANT_DEFAULT_PROVIDER
````

## Related readings

- [Writing an SSH server in Go](https://blog.gopheracademy.com/advent-2015/ssh-server-in-go/)

## References

[^1]: https://en.wikipedia.org/wiki/Secure_Shell
[^2]: https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process
[^3]: http://superuser.com/questions/449621/how-does-ssh-know-which-key-to-use
[^4]: http://security.stackexchange.com/questions/45963/diffie-hellman-key-exchange-in-plain-english
