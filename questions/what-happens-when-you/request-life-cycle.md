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

The so one is just way too simple

## TODO:

- [ ] http://stackoverflow.com/questions/2092527/what-happens-when-you-type-in-a-url-in-browser
- [ ] remember to talk about HTTPS, TCP
- [ ] SSL != TLS https://en.wikipedia.org/wiki/Public-key_cryptography#Weaknesses

## References

[^1]: https://github.com/alex/what-happens-when
[^2]: https://madhatted.com/2013/6/16/you-do-not-understand-browser-history
[^3]: http://blog.55minutes.com/2011/10/how-to-defeat-the-browser-back-button-cache/
[^4]: https://en.wikipedia.org/wiki/HTTP_ETag
[^5]: https://en.wikipedia.org/wiki/DNS_spoofing
[^6]: https://developers.google.com/speed/public-dns/
