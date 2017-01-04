# Model

<!-- toc -->

## OSI model

see [^2] for detail

- Application
- Presentation
- Transport
- Newtork
- Data Link
- Physical

## TCP model

4 layers model [^3] [^4]

- Application: HTTP, FTP
- Transport: TCP, UDP, Handles data-consistency functions
- Internet (Network): IP, network addressing and routing
- Network (Data Link): Ethernet, token-ring, Fiber Distributed Digitial Interface
- Physical: Not really part of the model, since TCP and IP, as protocols, deal with software rather than hardware. This layer is generally thought of as referring to all hardware under the Network Layer.

more about TCP can be found in [what happens when you type a url in browser](../questions/what-happens-when-you/request-life-cycle.md)

[^2]: https://en.wikipedia.org/wiki/OSI_model
[^3]: https://www.ischool.utexas.edu/~l38613dw/readings/NotesOnInterconnection.html
[^4]: http://www.comptechdoc.org/independent/networking/protocol/protlayers.html
