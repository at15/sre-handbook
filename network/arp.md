# ARP

Resolution if internet layer address into link layer address

## Packet

- [ ] TODO: I think I have wrote sth about ARP before
- sender MAC
- sender IP
- receiver MAC
- receiver IP

## Example

Localnetwork (without gateway or router)

- check local ARP cache
- if found, broadcast an Ethernet frame with found destination address
- if not found, broadcast ARP message with destination as `FF:FF:FF:FF:FF:FF:FF` MAC
address, reuesting anwser for the IP. The computer holding that IP responds and both
update their cache

If connected to router directly, router have ARP reply
