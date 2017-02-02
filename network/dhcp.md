# DHCP

Dynamic Host Configuration Protocol

<!-- toc -->

## Where

TODO: not precise and no references

- Your home's Wifi Router/ Your ISP's (Internet Service Provider, i.e. Xfinity, China Telecom) Modem
  - [ ] TODO: does Modem have DHCP? it seems one home would have just one public ip (and it is not fixed?)

## Provides

- lease duration
- IP Address
- Router (Default gateway)
  - [ ] TODO: which layer is this? link?
- DNS Servers
- DNS Domain Name [^1]

## Procedures

Client obtain a DHCP lease for an IP address, subnetmask, dns server etc.
If the lease expired, it need renewal

UDP is used, 67 for server, 68 for client

DORA, discovery, offer, request and acknowlegement [^1] [^2]

- discover, client broadcast request for a DHCP server (255.255.255.255)
- offer, DHCP server offer an address to client
  - [ ] TODO: it seems it is also using broadcast
- request, client broadcast a request to lease an address from one of the offering DHCP servers
- ack, the DHCP server that the client responds to acknowleges the client, assign it any configurated
DHCP options and update its DHCP database

renewal just need request and ack, and it can use unicast for request since it already has an IP address [^1]

## Softwares

- https://www.isc.org/downloads/dhcp/

## References

[^1]: https://technet.microsoft.com/en-us/library/dd145320(v=ws.10).aspx
[^2]: http://www.thenetworkencyclopedia.com/entry/dynamic-host-configuration-protocol-dhcp/
