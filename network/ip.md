# IP

<!-- toc -->

## IP Address

An **Internet Protocol address** is a numerical label assigned to each device. [^1]
Its two main functionality:

- host or network interface indentification
- location addressing

TODO: what's the difference between those two

- IPv4: 32-bit number
- Ipv6: 128-bit number

## IPv4

IPv4 has 32-bit, IPv4 addresses are canonically represented in dot-decimal notation, which consists of four decimal numbers, each ranging from 0 to 255, separated by dots, e.g., 172.16.254.1. Each part represents a group of 8 bits (octet) of the address.[^1]

### Subnetting

A subnetwork or subnet is a logical subdivision of an IP network.
The practice of dividing a network into two or more networks is called subnetting.[^4]

- (obsolete) In early stage, highest 8 bits is network number and rest are host identifier
- (obsolete) Then it comes the **Classful network** architecture in 1981
- After that **Classless Inter-Domain Routing (CIDR)** was introduced in 1993

#### Classful network

Classful network divides the address space for Internet Protocol Version 4 (IPv4) into five address classes by address range and is replaced by CIDR.
Since its discontinuation, remnants of classful network concepts remain in practice only in limited scope in the default configuration parameters of some network software and hardware components, most notably in the **default configuration of subnet masks**. [^3]

- Class A, B, C for three different sizes
- Class D for multicast
- Class E is reserved

| Class | Leading bits | Size of *network number* bit field | Size of *rest* bit field | Number of networks | Addresses per network | Total Addresses in Class | Start address | End address |
| -- | --- | -- | -- | ------------- | ---- | ---- | --------- | --------------- |
| A  | 0   | 8  | 24 | 128 = 2^(8-1) | 2^24 | 2^31 | 0.0.0.0   | 127.255.255.255 |
| B  | 10  | 16 | 16 | 2^(16-2)      | 2^16 | 2^30 | 128.0.0.0 | 191.255.255.255 |
| C  | 110 | 24 | 8  | 2^(24-3)      | 2^8  | 2^29 | 192.0.0.0 | 223.255.255.255 |
| D  | 1110| N  | N  | N             | N    | 2^28 | 224.0.0.0 | 239.255.255.255 |
| E  | 1111| N  | N  | N             | N    | 2^28 | 240.0.0.0 | 255.255.255.255 |

- The number of addresses usable for addressing specific hosts in each network is always **2^N - 2**
  - all-bists-zero host porition for network address  ~~a.k.a the first IP address in a network is the network address~~ (this is for CIDR)
  - all-bits-one host portion for broadcast address ~~a.k.a the last IP address is always the broadcast address~~ (this is for CIDR)

#### Classless Inter-Domain Routing (CIDR)

Classless Inter-Domain Routing is a method for allocating IP address and IP routing.
Classful network's different classes have very huge gap, make it not realisitic.

- **VLSM** variable-length subnet masking
- **CIDR notation**, an address or routing prefix is written with **a suffix indicating the number of bits of the prefix**, such as `192.168.2.0/24` for IPv4.
  - The suffix number is the count of leading 1 bits in the **routing mask**, traditionally called the **network mask**
  - NOTE: it is different from subnet mask, since IP addresses are almost always allocated in contiguous blocks, a subnet mask has no practical advantage over CIDR notation.
- An address can match multiple CIDR prefixes of different lengths.
- Assigment: IANA -> RIR (regional Internet registries) -> LIR (local Internet registries)

In routed subnets larger than /31 or /32, the number of available host addresses is usually reduced by two, namely the largest address, which is reserved as the **broadcast address**, and the smallest address, which identifies **the network itself**

#### Subnet mask vs CIDR notation

- subnet mask only applies to IPv4, while CIDR applies to both
- In CIDR notation, the prefix bits are always contiguous, whereas subnet masks may specify non-contiguous bits. However, since IP addresses are almost always allocated in contiguous blocks, a subnet mask has no practical advantage over CIDR notation. [^5]
- The result of the bitwise AND operation of IP address and the subnet mask is the network prefix . [^4]

#### Private network

These addresses are not routed on the Internet and thus their use need not be coordinated with an IP address registry. [^1]
Today, when needed, such private networks typically connect to the Internet through network address translation (NAT).

IANA-reserved private IPv4 network ranges

| bit | Start | End | No. of addresses |
| ---------------------------------- | ---------- | --------------- | ---- |
| 24-bit block (/8 prefix, 1 x A)    | 10.0.0.0   | 10.255.255.255  | 2^24 |
| 20-bit block (/12 prefix, 16 x B)  | 172.16.0.0 | 172.31.255.255  | 2^20 |
| 16-bit block (/16 prefix, 256 x C) | 192.168.0.0| 192.168.255.255 | 2^16 |

### IPv6


## Common IP Address

<!-- TODO: may use a table -->
- IPv4 Broadcast: `255.255.255.255` [^2]

## Common Port

- FTP-Data: 20
- FTP: 21
- SSH: 22
- Telnet: 23
- SMTP: 25
- DNS: 53 (Most UDP, use TCP when large, i.e. in IPv6)
- HTTP: 80, 8080
- HTTPS: 443

## TODO

- [ ] IPv6
- [ ] NAT
- [ ] Congest Control

## References

[^1]: https://en.wikipedia.org/wiki/IP_address
[^2]: https://golang.org/pkg/net/#pkg-variables
[^3]: https://en.wikipedia.org/wiki/Classful_network
[^4]: https://en.wikipedia.org/wiki/Subnetwork
[^5]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
