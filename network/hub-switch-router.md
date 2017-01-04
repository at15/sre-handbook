# Hub vs Switch vs Router

## Hub

**Hubs are now largely obsolete, having been replaced by network switches except in very old installations or specialized applications.**
Historically, the main reason for purchasing hubs rather than switches was their price. [^2]

- Hub also know as repeaters operate on physical layer only.
Can NOT hadnle higher layer like MAC and IP.
- Hub can NOT process data, all it does is transfers data to every port excluding
the port from where data was generated.
- Hub work only in half duplex mode. i.e. a device connected to a hub can either
send or receive data at a given time, but not both.
- Hub only allow one device to send out data at one time,
otherwise data collisions happen, in this case,
it rejects all devices and tell them to send again, usually device use a random timer

## Switch

- Inteligent hub

## Router

- Inteligent switch

## TODO

- [x] so where is hub actually in our daily life? Obsoleted.
- what is the modem we use
- what is the wifi router we have
- [ ] Switches can avoid loops through the use of **spanning tree protocol**

## References

[^1]: https://s905060.gitbooks.io/site-reliability-engineer-handbook/content/hubs_vs_switches_vs_routers__networking_device_fundamentals.html
[^2]: https://en.wikipedia.org/wiki/Ethernet_hub
<!-- [^2]: http://www.escotal.com/networkdevices.html -->
