---
title: "Networking Fundamentals"
---

# The OSI model

<style>
.kinda-small {
  font-size: 80%;
}
.preserve-text {
  text-transform: none;
}
.
</style>

##

The OSI Model divides networking into seven distinct abstraction levels:

::::{.kinda-small .incremental}
1. Physical layer
2. Data link layer
3. Network layer
4. Transport layer
5. Session layer
6. Presentation layer
7. Application layer
::::

## network abstractions

1. Physical layer
2. Data link layer
3. Network layer
4. Transport layer

## host abstractions

4. Transport layer
5. Session layer
6. Presentation layer
7. Application layer

## Important disclaimer!

::::{.incremental}
The OSI Model is just that—a model—it doesn't perfectly describe the topology of modern networking:
[It gets the broad strokes right, which is why I think it's worth talking about,]{.fragment}
[but it's not perfect and at the end I'll talk about where it gets it wrong.]{.fragment}
::::

# physical layer

## purpose

::::{.incremental}
- The physical layer is probably the conceptually simplest,
- But one of the more complicated to implement.
- The physical layer handles transfer of unstructured data between to devices like networking cards or switches through a media
- said media is usually either: optical fiber, copper wire, or radio waves
::::

## examples

::::{.incremental}
- Cat Cable
- CAN Bus
- Optical fiber
::::

# data link layer

## purpose

::::{.incremental}
- It's subdivided into two sub-layers:
  - Medium access control (MAC)
  - Logical link control (LLC)
- The MAC layer handles transmission of "frames" between two nodes via a "network segment"
- The LLC layer handles control of the MAC layer and allows multiplexing, flow control, and automatic repeat request
::::

## examples

::::{.incremental}
- Ethernet
- IEEE 802.11 [(Wi-Fi)]{.fragment}
- Address Resolution Protocol (ARP)
::::

# network layer

## purpose

::::{.incremental}
- Networking...  *duh*
- The networking layer introduces the abstraction of a "host" a single node in the network that could have one or more networking devices
- This is also the level where "routing" is implemented,
- Layer 2 networks can route locally based on MAC addresses, at layer 3 we can query for hosts and route through gateways to distant nodes
::::

## examples

::::{.incremental}
- Internet Protocol (IP)
  - IPv4
  - IPv6
  - Open Shortest Path First (OSPF)
- Internet Control Message Protocol (ICMP)

::::

# transport layer

## purpose

::::{.incremental}
- Up until now, while some individual links may have validation and data re-requesting features,
- for the most part, we're just chucking bits into one end, and hoping they pop out the other.
- The transport layer introduces concepts like connections and protocols with reliability guarantees.
::::

## examples

::::{.incremental}
- Transmission Control Protocol (TCP)
- User Datagram Protocol (UDP)
::::

# session layer

## purpose

::::{.incremental}
- This one is kinda BS.
- We're now on the host side, so we're kinda back to OS design.
- The primary abstraction of the session layer is sockets.
::::

# presentation layer

## purpose

::::{.incremental}
- The idea behind the presentation layer is about standardizing data transfer:
- Taking application specific data and transmitting it in a common format.
::::

## examples

::::{.incremental}
- MIME types
- character encodings
  - ASCII
  - UTF-8
  - UTF-16 [(because javascript)]{.fragment}
::::

# application layer

## purpose

::::{.incremental}
- *do things*
- implement application specific behavior to accomplish some goal
::::

## examples

::::{.incremental}
- DNS
- DHCP
- HTTP(S)
- IMAP(S)
- SSH
- RTSP
- FTP
- and many more...
::::

# flaws with the OSI model

## Transport Layer Security (TLS)

::::{.incremental}
- TLS is, as the name suggests, a protocol for encrypting data at the transport layer
- it's used to add security to web protocols; it's the "secure" in HTTPS and IMAPS (and other protocols)
- but it contains characteristics of both a transport layer protocol (layer 4) and a presentation layer protocol (layer 6)
::::

## Ethernet

::::{.incremental}
- Yeah so Ethernet is like... a layer 1, 2, and kinda 3 protocol
- You'll see this problem a lot, where once a combination of protocols becomes a de facto standard the lines start getting blurred
::::

# Examples!
