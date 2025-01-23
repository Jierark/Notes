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

## Layer 7: Application

# TCP/IP Model

# IP Addressing

# Encapsulation





[^1]: I've heavily simplified its history here. Though, I am kind of interesting in reading more about the history of the early Internet.