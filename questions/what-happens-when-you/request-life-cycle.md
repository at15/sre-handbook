# Request life cycle

What happens when you enter URL in browser

<!-- toc -->

## Concepts

### Browser

- Cache
  - Server can specify the cache control policy using the `cache-control` header,
  but things may not like developer expected, i.e. when user click the back button,
  browser may load everything from cache without any request [^2]
  - it is said that `Cache-Control: no-cache, max-age=0, must-revalidate, no-store`
  can solve the back button problem [^3]
  - [ ] TODO: forward and backward cache
  - [ ] TODO: push state, I think I have encountered the term when using angularjs
- Connection
  - After a http request is finished, the browser can kept the opened connection for later requests.
- Port
  - [ ] TODO: does browser use a new local port when it open a new connection to the server

### Server

- Cache
  - Server can check the header by browser (i.e. `ETag`[^4]) to determine if browser
  has valid cache, if so, it returns `304` with empty body

- TODO: ethernet address (also named a MAC address)

## Procedure

### DNS lookup

This part is mainly from [^1]

If user enter ip address, then there is no need for domain name lookup

- browser check its cache i.e. `chrome://net-internals/#dns`
- If not found, the browser calls `gethostbyname` library function (varies by OS) to do the lookup
- If not found, first lookup the `hosts` file i.e. `/etc/hosts`
- If still not found, it goes to DNS server, by default it is configured by DHCP, but can manually configured
by user to avoid certain level of DNS pollution [^5], i.e. use `8.8.8.8` [^6]
- [ ] TODO: If the DNS server is on the same subnet the network library follows the ARP process below for the DNS server
- [ ] TODO: If the DNS server is on a different subnet, the network library follows the ARP process below for the default gateway IP

TLS and SSH key exchange is different because TLS use public/private key and
invlove trused CA

### ARP process

The Address Resolution Protocol (ARP) is a telecommunication protocol used for
resolution of Internet layer addresses into link layer addresses [^9].
<!-- NOTE: in [^9] it's named a, but I think it should be named as -->
i.e. From IPv4 addresses to a physical address like an **Ethernet address** (also named as **MAC address**)

#### IPv4

- Check ARP Cache, if target IP is found, following step are skiped
- Check route table, to see if the Target IP address is on any of the subnets on the local route table. If it is, the library uses the interface associated with that subnet. If it is not, the library uses the interface that has the subnet of our default gateway. [^1]
- Look up the MAC address of the selected network interface [^1]
- Sends a Layer 2 (data link layer of the OSI model) ARP request:

````
Sender MAC: interface:mac:address:here
Sender IP: interface.ip.goes.here
Target MAC: FF:FF:FF:FF:FF:FF (Broadcast)
Target IP: target.ip.goes.here
````

Router gives ARP reply

````
Sender MAC: target:mac:address:here
Sender IP: target.ip.goes.here
Target MAC: interface:mac:address:here
Target IP: interface.ip.goes.here
````

- [ ] How does the router come up with reply, it has to ask other router if this is outside its network?
<!-- - TODO: Computer 1 has to send a broadcast ARP message (destination FF:FF:FF:FF:FF:FF MAC address, which is accepted by all computers) requesting an answer for 192.168.0.55
  - this didn't conisder gateways or routers -->

- ARP probe, test if this address is already used by someone else
  - [ ] TODO: what if some bad guy always say it is in use?

#### IPv6

For IPv6, functionality of ARP is provided by the [Neighbor Discovery Protocol (NDP)](https://en.wikipedia.org/wiki/Neighbor_Discovery_Protocol)

### TCP three-way handshake

#### Flags

- SYN: synchronize sequence number, Only the first packet sent from each end should have this flag set. Some other flags and fields change meaning based on this flag, and some are only valid for when it is set, and others when it is clear. [^7]
- ACK: indicates that the Acknowledgment field is significant. All packets after the initial SYN packet sent by the client should have this flag set [^7]

#### Values

- Sequence number
  - ISN: initial sequence number If the SYN flag is set (1)
  - SEQ: If the SYN flag is clear (0), then this is the accumulated sequence number of the first data byte of this segment for the current session
- Acknowledgment number: if the ACK flag is set then the value of this field is the next sequence number that the sender is expecting. This acknowledges receipt of all prior bytes (if any). The first ACK sent by each end acknowledges the other end's initial sequence number itself, but no data.

#### Procedure

- SYN: Client
  - client choose ISN, A
  - set the SYN bit to indicate it is setting the ISN
  - send the packet to the server
- SYN-ACK: Server
  - server choose ISN, B
  - set the SYN bit to indicate it is setting the ISN
  - set the ACK bit to indicate it is acknowledging receipt of the first packet
  - set ACK as A+1
  - send the packet to the client
- ACK: Client
  - set SEQ to A+1
  - set ACK bit
  - set ACK as B+1
  - send the packet to the server

### TLS handshake

Transport Layer Security (TLS) and its predecessor, Secure Sockets Layer (SSL), both frequently referred to as "SSL", are cryptographic protocols that provide communications security over a computer network. [^8]

The following is a mix of [^1] and [^8]

- client ask the server to use TLS (i.e. use https port 443)
- client provide the server a list of cipher suites it supports
- server pick one cipher suites it also support, send it back with its public key signed by a CA (Certificate Authority)
- client verify the digital certificate against its list of trusted CAs before proceeding
- generate the symmetric key for encrypt following communication, either
  - **use Diffie-Hellman as SSH** does, if the server's private key is disclosed in the future, it can not be used to decrypt the current sessions (recorded by third party)
  - client **use server's public key to encrypt a random number** and send to server
  - server decrypt using its private key to get the random number and generate its own copy of symmetric key
  - client generate hash of its own copy of symmetric key, send `Finish` message with its hash to server encrypted using the symmetric key
  - server generate hash of its own copy of symmetric key and compare with decrypted client-sent hash.
  if matches, server send `Finish` message to client encrypted using the symmetric key
- encypt the following communication using the symmetric key


- More detail can be found in [wiki](https://en.wikipedia.org/wiki/Transport_Layer_Security#Basic_TLS_handshake)
- If CA is compromised, then using server's public key to generate the symmetric is not safe.

### HTTP



## TODO:

- [ ] HTTP
- [ ] HTTP2, how do brower know server support HTTP2
- [ ] Encapsulation and Decapsulation
- [ ] The stackoverflow one is just way too simple http://stackoverflow.com/questions/2092527/what-happens-when-you-type-in-a-url-in-browser
- [ ] SSL != TLS https://en.wikipedia.org/wiki/Public-key_cryptography#Weaknesses
- [ ] When I use `arp` I only found two entry, me and the router. But other people do have multiple entries.

## References

[^1]: https://github.com/alex/what-happens-when
[^2]: https://madhatted.com/2013/6/16/you-do-not-understand-browser-history
[^3]: http://blog.55minutes.com/2011/10/how-to-defeat-the-browser-back-button-cache/
[^4]: https://en.wikipedia.org/wiki/HTTP_ETag
[^5]: https://en.wikipedia.org/wiki/DNS_spoofing
[^6]: https://developers.google.com/speed/public-dns/
[^7]: https://en.wikipedia.org/wiki/Transmission_Control_Protocol
[^8]: https://en.wikipedia.org/wiki/Transport_Layer_Security
[^9]: https://en.wikipedia.org/wiki/Address_Resolution_Protocol
