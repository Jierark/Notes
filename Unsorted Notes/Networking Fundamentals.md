This page will cover the fundamentals ideas of how computers can talk to each other, whether they are directly connected or across the entire world.

# The OSI Model
As the Internet was being developed in the early 1970s, it became clear that there needed to be a standard for computer networks if the Internet was to be deployed globally. Thus was born the OSI model, a conceptual model developed by the International Organization for Standardization.[^1] This model describes a general framework for constructing practical computer networks, and it is a common lecture (or set of lectures) across many network classes. Here are the seven layers and their operation:

## Layer 1: Physical
The bottom layer of the OSI model looks at how computers connect to each other. As the name may imply, the easiest way to connect some computers is with physical wires, such as ethernet or optical fiber. Additionally, one has to consider how to transmit the binary digits of "0" and "1" using these different mediums. The physical layer also includes optical and wireless signals, transmitted through data antennae via the various wifi bands (radio, 2.4GHz, 5GHz, 6GHz)
## Layer 2: Data Link 
One step above the physical layer, we now consider how multiple computers, using a shared medium, can communicate each other. This is the purpose of Data Link, describing a common protocol for systems within a network segment. Ethernet and Wifi protocols are examples of implementations of this layer.
## Layer 3: Network
From a small group of local computers to many of these groups, the next layer, aptly named the "Network" Layer, focuses on sending data between multiple networks. IP, ICMP, and VPN protocols are examples of this layer.
## Layer 4: Transport
OK, now we have a basic framework for just sending packets of data between computers, whether they are locally connected or across a larger network. We can now move to more advanced connection methods. The Transport Layer considers how two applications, running on different hosts, can communicate to each other. TCP and UDP are the most common protocols associated with these
### UDP
User Datagram Protocol will send packets between two computers, but it has no guarantees on whether packets reach their destination, or if they arrive in order. The tradeoff is speed, so it is useful in time-sensitive applications, such as video streaming.
### TCP 
Transmission Control Protocol will establish a connection before sending packets. It is slower, but it contains multiple checks to ensure that packets arrive at their destination and in order.
## Layer 5: Session
The Session Layer is responsible for opening, maintaining, and synchronizing communication between applications. NFS and RPC protocols are a few examples.
## Layer 6: Presentation
The data within a packet can be interpreted in many different ways. It could be a form from a POST request, an image, a document, or many other kinds of files. This is what the Presentation Layer deals with: making sure the Application Layer knows what kind of data was just received. This layer deals with data encoding, compression, and encryption. Examples of this include images being recognized with .jpg/.png extensions.
## Layer 7: Application
The top-most layer of the OSI model is what applications interact with (hence, the name). At this point, all the underlying requirements have been fulfilled by the previous layers, easing the burden on the application. Most common protocols exist in this layer, such as HTTP(S), FTP, DNS, SMTP, and POP3.

[^1]: I've heavily simplified its history here. Though, I am kind of interesting in reading more about the history of the early Internet.
# TCP/IP Model
One implementation of the OSI model is the TCP/IP model. This condenses the 7 layers of the OSI model into 5: 

| TCP/IP Layer | Corresponding OSI Layer(s)         |
| ------------ | ---------------------------------- |
| Application  | Application, Presentation, Session |
| Transport    | Transport                          |
| Internet     | Network                            |
| Link         | Data Link                          |
| Physical     | Physical                           |

## Encapsulation
What is the best way to construct a packet of data? Operating off the TCP/IP model, we need to add some data in order to traverse each layer, but what's the best way to add this information? This is where encapsulation comes into play. To put it simply, to construct an outgoing packet, each layer successively adds on a header (or tailer) to the packet and sends it to the next layer, repeating until the full packet, or frame, is completed. Here's how it works:

- The Application layer starts the frame by adding its data, and then passes it onto the Transport layer
- The Transport layer adds the required TCP or UDP header (called a segment or datagram, respectively) and passes it down.
- The Internet layer adds an IP header.
- The Link layer adds a Ethernet/Wifi header and tailer, completing the frame.
- The physical layer just transmits those bits
The destination device will then work backwards, unfolding each layer until it extracts the application data.
# DHCP
Connecting to some network requires configuration of three properties:
- an IP address and subnet mask, 
- a router/gateway,
- a DNS server. 
For a stationary server, these can be set manually, but for most devices which may switch between different networks, this can be a painful endeavor. Enter Dynamic Host Configuration Protocol (DHCP). This protocol runs over UDP and resolves much of the headaches that can occur from having to set up these properties.

DHCP operates on 4 steps:
1. Discover - The client broadcasts a DHCPDiscover packet to connect to a local DHCP server if it exists
2. Offer - The server responds with DHCPOffer, giving an open IP address for the client
3. Request - The client responds with DHCPRequest, accepting the IP address provided.
4. Acknowledge - The server responds with DHCPAcknowledge, acknowledging that the client has accepted the previously provided IP address.
Below is an example of this process (credit: tryhackme.com)
```
user@TryHackMe$ tshark -r DHCP-G5000.pcap -n 
1 0.000000 0.0.0.0 → 255.255.255.255 DHCP 342 DHCP Discover - Transaction ID 0xfb92d53f 
2 0.013904 192.168.66.1 → 192.168.66.133 DHCP 376 DHCP Offer - Transaction ID 0xfb92d53f
3 4.115318 0.0.0.0 → 255.255.255.255 DHCP 342 DHCP Request - Transaction ID 0xfb92d53f
4 4.228117 192.168.66.1 → 192.168.66.133 DHCP 376 DHCP ACK - Transaction ID 0xfb92d53f
```
# ARP 
#TODO: fill this out when I feel like it
# ICMP
ICMP is an important protocol in troubleshooting networks. It is most commonly used in the ```ping``` and ```traceroute``` commands. ```ping``` will send packets to a destination and measure the time it takes to hear a reply, while ```traceroute``` will discover the route a packet takes from source to destination.
# Network Address Translation (NAT)
One of the problems with IPv4 is that it can only support around 4 billion unique addresses. While this was alright in the early days of the internet, today's world has a lot more devices that need to maintain an internet connection (see IoT). In order to continue using IPv4 addressing for some time longer, NAT was developed. The key idea is that multiple devices can maintain a single public IP address, while having different private IP addresses.

Routers with NAT maintain a table to translate public facing IP addresses and ports to private IP addresses and ports, moving incoming packets to their correct destinations. In general, the internal network will utilizes private IP addresses, (10.x.x.x \[10/8\], 172.16.x.x - 172.31.x.x \[172.16/12\], 192.168.x.x \[192.168/16\]).