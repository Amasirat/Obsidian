It's a field of programming to be able to write applications that have some interactions with other computers.

This document is a good resource. [Beej's Guide to Network Programming](https://beej.us/guide/bgnet/html/split/what-is-a-socket.html)

# Sockets

The idea of a socket comes from a physical socket in the wall. The idea is that in order for you to connect to the internet, you can connect to a socket which is a one way entry point to the rest of the network. It's an end point for sending and recieving data across the internet.

In Unix, you can use sockets to speak to other programs using standard Unix file descriptors.

**Unix file descriptors**: A file descriptor is an integer associated with a currently opened file inside Unix systems like Linux, BSD, etc. That file can be anything, network connection, a terminal, a real file that lives on the disk. Anything! That's why they say **Everything in Unix is a file**.

In order to connect to a socket, you have to make a call to the socket(), this system routine returns a socket descriptor that you can use to send and recieve data through ``send()`` and ``recv()``

*You can also use `read()` and `write()` however the functions described above give much more control*

There are many different types of Sockets, and one of them are Internet Sockets that can connect to the wide internet.

# Two types of Internet Sockets

There are two main types of sockets:

## Stream Socket

Reliable two way connected communication. it's generally error-free and outputs data on the other side in the exact same order as it was inputed. telnet, ssh, and HTTP use these types of sockets to operate.

They use the [[TCP]] protocol to achieve the quality in data transmission. TCP makes sure the data is transmitted in order.
## Datagram Socket 

They use the [[UDP]] protocol. These are called connectionless sockets because they don't require connections like stream socket. Packets are simply sent and forgotten about. This means that some data could fail to send although the ones that are sent are error-free. This protocol is used because it is much faster than TCP however it is not reliable. It is generally used for audio or video streaming, multiplayer games, tftp, dhcpcd and applications of that sort.

tftp is a special case in that it has a special protocol on top of UDP where when a packet is sent, the recipient has to send back an okay response (ACK packet), if it doesn't do so for five seconds, the packet will be sent again.

A Datagram packet works by encapsulating data through many layers.

![[2024-12-26_00-45.png]]
The data is wrapped by the TFTP protocol, then UDP, then the IP and then the Ethernet which is the physical layer. 
When a packet is recieved, several tools inside the computer identify these layers and take care of them so that they can reach the data within.

The OSI model is also relevant, its general layers are:

* Application
* Presentation
* Session
* Transport
* Network
* Data Link
* Physical

Although that is too general, a more specialized conception of the same idea in Unix systems is this:

* Application Layer: (telnet, ftp, etc)
* Host-to-Host Transport Layer (TCP, UDP)
* Internet Layer (IP and Routing)
* Network Acess Layer (Ethernet, wifi)

# IP Address

Internet Protocol Address is a way of giving identities to networks and making routing possible. An old version of IP is the version 4 or IPv4. It is 4 8 bit numbers seperated by a dot. 192.0.2.111 for example.

However it was not enough and [Vint Cerf] warned everyone that we might run out.

IPv6 was born to combat this problem. It is a 128 bit number which is seperated by colons. (it is also represented in hexadecimal)

2001:0db1:5a12:0122:0000:0000:0000:0121 or 2001:0db1:5a12:0122::0121

Two colons means the rest are 0s.

::1 is always used as a loopback address meaning it's "the machine I'm running on". in IPv4 that is 127.0.0.1.

In order to have IPv4 compatibility, you can show IPv4 addresses like this in IPv6: 

192.60.2.102 ======> ::ffff:192.60.2.102

Some CPUs store numbers in reverse order which is called in **little-endian**. 

The prefered way to store numbers in networks is in **big-endian** or storing it in the correct way. 

The Host-Byte order can be different to the Netwrok-Byte order, because of this difference, sometimes you need to convert between these two systems, therefore you can use functions to do this if that's necessary.

htons() for example means host to network short(). There are both short and long data types that you can convert to and from host and network byte order.

You'll want to convert numbers to network-order if they are sent out to the network and also convert them to host byte order as they come in.

# Structs

A socket descriptor is an **int** data type.

struct addrinfo is used to prep the socket address for subsequent use.

```C
struct addrinfo {
    int              ai_flags;     // AI_PASSIVE, AI_CANONNAME, etc.
    int              ai_family;    // AF_INET, AF_INET6, AF_UNSPEC
    int              ai_socktype;  // SOCK_STREAM, SOCK_DGRAM
    int              ai_protocol;  // use 0 for "any"
    size_t           ai_addrlen;   // size of ai_addr in bytes
    struct sockaddr *ai_addr;      // struct sockaddr_in or _in6
    char            *ai_canonname; // full canonical hostname

    struct addrinfo* ai_next;      // linked list, next node
};
```

It's a linked list.

You can make a call to getaddrinfo() to fill this struct out.

struct sockaddr holds socket address information for many types of sockets.

```C
struct sockaddr {
    unsigned short    sa_family;    // address family, AF_xxx
    char              sa_data[14];  // 14 bytes of protocol address
}; 
```

You can convert an IP address from dot notation and give it to a sockaddr_in.sin_addr using the below code:

```C
struct sockaddr_in sa; // IPv4
struct sockaddr_in6 sa6; // IPv6

inet_pton(AF_INET, "10.12.110.57", &(sa.sin_addr));
inet_pton(AF_INET6, "2001:db8:63b3:1::3490", &(sa6.sin6_addr));
```

