# Hub vs Switch vs Router

## Hub

**Hubs are now largely obsolete, having been replaced by network switches except in very old installations or specialized applications.**
Historically, the main reason for purchasing hubs rather than switches was their price. [^2]

- Hub also know as repeaters operate on **physical layer only**.
Can NOT hadnle higher layer like MAC and IP.
- Hub can NOT process data, all it does is transfers data to every port excluding
the port from where data was generated.
- Hub work only in half duplex mode. i.e. a device connected to a hub can either
send or receive data at a given time, but not both.
- Hub only allow one device to send out data at one time,
otherwise data collisions happen, in this case,
it rejects all devices and tell them to send again, usually device use a random timer

## Switch

A network switch (also called switching hub, bridging hub, officially MAC bridge)
is a computer networking device that connects devices together on a computer network,
by using packet switching to receive, process and forward data to the destination device. [^3]

- A switch is more **intelligent** than a repeater hub, which simply retransmits packets out of every port of the hub excepting the port on which the packet was received, unable to distinguish different recipients, and achieving an overall lower network efficiency
- full duplex mode
- Switches may operate at one or more layers of the OSI model, including the data link and network layers. A device that operates simultaneously at more than one of these layers is known as a multilayer switch.

## Router

A router is a networking device that forwards data packets between computer networks. Routers perform the traffic directing functions on the Internet.
A data packet is typically forwarded from one router to another through the networks that constitute the internetwork until it reaches its destination node. [^4]

- Inteligent switch
- Routing table

**Switches create a network. Routers connect networks. A router links computers to the Internet, so users can share the connection. A router acts as a dispatcher, choosing the best path for information to travel so it's received quickly.** [^5]


## TODO

- [x] so where is hub actually in our daily life? Obsoleted.
- [ ] what is the modem we use
- [x] what is the wifi router we have, it is router and has DHCP
- [ ] Switches can avoid loops through the use of **spanning tree protocol**
- [ ] have a table to demonstrate their difference
- [ ] https://en.wikipedia.org/wiki/Network_switch Layer-specific functionality part talks about the difference a lot
- [ ] http://www.webopedia.com/DidYouKnow/Hardware_Software/router_switch_hub.asp this also tells the difference

## References

[^1]: https://s905060.gitbooks.io/site-reliability-engineer-handbook/content/hubs_vs_switches_vs_routers__networking_device_fundamentals.html
[^2]: https://en.wikipedia.org/wiki/Ethernet_hub
[^3]: https://en.wikipedia.org/wiki/Network_switch
<!-- [^2]: http://www.escotal.com/networkdevices.html -->
[^4]: https://en.wikipedia.org/wiki/Router_(computing)
[^5]: https://www.cisco.com/cisco/web/solutions/small_business/resource_center/articles/connect_employees_and_offices/what_is_a_network_switch/index.html
