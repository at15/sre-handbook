# SSH

What happens when you ssh into a server

## Concepts

NOTE: most of this section is directly from [digitalocean's community tutorial](https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process)

### Symmetric Encryption

- One key can be used for both encypt and decrypt
- Symmetric keys are used by SSH in order to encrypt the entire connection
- The key is created through a **key exchange algorithm**
- The key is created prior to authenticating a client, once it is created, rest communication are all encrypted using this key
- AES, Blowfish, 3DES etc. different symmetrical cipher systems can be used for the symmetric encryption,

### Asymmetric Encryption

- **public/private asymmetrical key pairs that can be created are only used for authentication, not the encrypting the connection**
- public key, can be freely shared with any party
- private key, can be encrypted using a passpharse
- public key encrpyt messages that can only be decrypted by the private key

Key exchange: used to setup the symmetric encryption

- both client and server generate temporary key pairs and exchange public key in order to produce the shared secret

SSH key-based authentication

- client creates a key pair and add the public key to server's `~/.ssh/authorized_keys`
- after symmetric connection is established, server use the public key in `authorized_keys`
to encrypt a challenge message to the client, if the client can prove it was able to decrypt the
message, it has demonstarted that it owns the associated private key
  - [x] FIXED: how does the server know which public key to use? ~~I think client send its public key to the server?~~ It tries every public key
  - http://superuser.com/questions/449621/how-does-ssh-know-which-key-to-use `Turns out, it tries a bunch of keys that it finds in the .ssh dir. Basically tries whatever's in there`

### Hashes



## Ref

- https://www.digitalocean.com/community/tutorials/understanding-the-ssh-encryption-and-connection-process
