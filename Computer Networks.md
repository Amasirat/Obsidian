A series of interconnected computers are called a **Network**. It is a wide and fascinating topic and many systems and additional protocols are needed to make this network feasible and usable. The material here is a summarized version of the book *Computer Networks: A System's Approach*.

# Chapter 1: Foundation

A Network has many parts in its existence:
* Users: Those who use applications that are placed on a network
* Application Programmers: Those who develop applications that run on the network
* Managers and Operators: Those who manage the network
* Builders: Those who build the architecture and protocols of the internet

Understanding the underlying principles of networks helps programmers and managers to operate more efficiently and make more optimal programs.

## Classes of Applications

The World Wide Web, is often confused with the internet but it is more an **application** running on the internet, although it is a stretch to call it just a single application.

The Web is a simple interface that provides a graphical view of the resources you have access to on the internet. Each resource or object is tied to an identifier or (URL: Uniform Resource Locator) that gives a way of identifying all possible objects on the internet. It constitutes of:

http://www.example.com/example

*  **http**: indicates the Hypertext Transfer Protocol, it is to be used to download the page
* www.example.com: is a domain name or name of the machine that hosts the page, this is then translated to an IP address using a DNS lookup server.
* /example is a subdirectory inside that server and gives access to further pages.
## Requirements

In order to make the idea of a computer network a reality, there are some basic requirements that need to be met.

### **Scalable**
A system that is designed to support growth to an arbitrarily large size is said to scale. 

A network also needs to be able to scale as it grows wider. For this, we can't just connect every computer to each other directly. 
* We need a mechanism to send packets to computers that are indirectly connected (**Packet Switching**) 
* and we also need a systematic method for knowing how to forward packets to computers called **routing**. 

A network is also recursive, each network can be connected to other networks to create larger networks.
Some terminology:

**Node**: An entity that is connected on a network. It's the abstracted form of a computing device, this can be hardware or even software in some cases.

**Swtiches**: They are basic computers that do the job of recieving packets, storing them and then forwarding them in the network. This way, client computers can simply be connected instead of also taking on the burden of packet forwarding, these computers are called **hosts**.

Switches connect computers **within** a network.
**Routers** connect networks to **outside** networks.
### Cost-Effective Resource Sharing

Packets need to be forwarded through the network in as efficient a manner as possible.

Here we run into the idea of **multiplexing**. It means combining the flow of three data sources (hosts) into one, and **demultiplexing** back into seperate flows.

Three methods for this are:
* STDM: Synchronous time-division multiplexing, similar to how roundrobin handled processes inside an operating system. The idea is to send packets from one data source in a specific time quantum and then switch to a different one once it runs out.
* FDM: Frequency-division multiplexing: The idea is to transmit each flow over a different frequency similar to TV and Radio.
* Both of the previous methods are flawed. 
	* If one of the flows has no data to send, the network will stay idle at that time quantum or frequency channel.
	* They're limited to situations where maximum number of flows is known.
	Statistical multiplexing addresses both of these shortcomings. Similar to STDM, it transmits data in parts however it's not turn-based but **demand-based**, and a transmission does not need to wait for its time quantum to arrive. To avoid one transmission hogging the flow, an upper bound on the size of the data is considered, this block of limited data is called a **packet**.
Basically, each physical link contains packets and decision is made in what packets are sent on a packet-to-packet basis.

**Most messages are not able to fit into only one packet, so a computer recieves a bunch of different packets and does the job of reassembling them into one form of data**

![[2025-02-17_14-15.png]]

If a switch (shown above) receives packets faster than it can send for an extended period of time, the switch will then run out of memory. It is then said to be **congested**.

Key challenges for cost effective resource sharing are:
* Dealing with **congestion**
* Fairly allocating link capacity to different flows
### Support for Common Services

Packet sending is not all that a network needs to do. In order to have rich communication with different computers, some ground rules or **protocols** need to be introduced to establish how these packets are sent and recieved.

We have two choices, either we define this high-level semantics in the switch-level, or keep the switch interconnected network simple and make the host to host connections define these rules for themselves. 

*Fun note, the former version is what was decided for phone lines, and that is why they can afford to be so simplistic as devices compared to computers. Of course in the case of computer networks, it was decided to keep the interconnected networks as simple as possible.*

In that end, different *channels* are developed to define rules for how to send and recieve packets. 
* Request/Reply channel or TCP: Used when the integrity of packets is important like text files.
* Message Channel or UDP: Used when speed is more important like video or audio, and some  slight corruptions are tolerated.

**There are three general classes of failures**
* **bit errors**: After transmission some bits get flipped accidentally.
* **Packet errors**: A packet is for whatever reason lost, either due to bit errors or if a switch gets overloaded and has to drop the packet mid-network. Less commonly, the software in switches could have made a mistake and forwarded the packet to the wrong node.
* **Node or Link level errors**: a physical link is cut or a computer in the middle of the way crashes.

These failures can have massive impacts on networks however it is possible to circumvent some of these problems. For example, to route around a failed node or use error detection algorithms for bit errors.
### Manageability

A network needs to be managed. So another requirement of a network is to make it so that it remains simple as the network scales.
## Network Architecture

Abstractions are critical to understanding network architectures, it is a tool for designers to manage complexity while designing systems. Abstraction usually leads to a layered approach to design. The benefits of this includes:
* Manageability: Decomposes a complex problem into small manageable problems.
* Modularity and Separation of concerns: Makin it so that you can easily change technologies related to one layer of the design without impacting the lower layers.

Abstract objects that make up the layers of a network architecture are called **Protocols**.

A **protocol** provides a communication service that higher level objects can use to send data through the network.

Each protocol defines 2 interfaces:
* **Service Interface**: Provides an interface to other objects on the **same** computer that want to communicate with the network
* **Peer Interface**: An interface for other computers, or peers that define the form and meaning of messages they send.

One system can have multiple protocols that handle different types of communication. A protocol graph is used to list and outline their relationships to each other.

## Encapsulation

A protocol usually encapsulates the data it is sending with a header of its own that defines some information related to the data and how it should be read. (Sometimes its added at the end which then is called a trailer). The data itself is called the **body** or **payload** of the message.

This encapsulation step is repeated at each level of the protocol graph.

Once the message is sent and recieved the data is stripped of its headers that concern the same protocol on the other device, until the body of the message is shown.

## Multiplexing and Demultiplexing

The idea of multiplexing plays a similar role as the physical link layer in the protocol layer. The header of the message usually contains a **demux** key that gives information on which application the message belongs to in the system. Once the header is read by the protocol in the other system, the message is demultiplexed into its appropriate application.

## The 7-Layer OSI Model

This is a general 7-layer model defined by network designers called Open System Interconnection (OSI). It defines a general partitionaing of network capabilities where each protocol implements the functionality defined by one layer.

* **Physical Link**
* **Data Link**
* **Network**
* **Transport**
* **Session**
* **Presentation**
* **Application**

![[2025-02-19_19-15.png]]

## Internet Architecture

Also sometimes called TCP/IP, is defined by the IETF organization.

While the 7-layer OSI model can theoratically be applied to the internet, the 4 layer Internet Architecture is used instead.

![[2025-02-19_19-17.png]]

* 1. At the bottom-most level, there's the subnetwork layer which contains protocols needed for basic network connectivity like Ethernet, Wireless connections, etc. 
* 2. The second layer is only comprised of one protocol, the Internet Protocol (IP), It supports the interconnection of multiple technologies into a single logical internetwork.
* 3. Comprised of mainly two protocols, **TCP** and **UDP**, They provide logical channels for communication for applications. TCP provides a relaiable byte stream channel. UDP provides unreliable datagram delivery channel. 
* 4. A range of application protocols that use TCP or UDP. HTTP, FTP, SMTP or Simple Mail Transfer Protocol are some examples of application level protocols.

Application Layer relates to layer 7 of the OSI model. 
Transport layer relates to layer 4.
IP is layer 3.
And the subnet layer is layer 2 and 1.

Internet Architecture has three features:
* Does not imply strict layering, meaning an application is free to bypass the defined transport layer (TCP and UDP), and to directly use IP.
* IP serves as the focal point of the architecture. This creates an hourglass shape in the protocol graph (Shown above)
* For a new protocol to be officially recognized, there must be both a **specification** and the existence of at least one or two **implementations**.
## Socket Programming

The architecture of the internet has made it available to create APIs or abstractions on top. That way we are able to connect with the internet with just a few lines of programming.

Each protocol provides a certain set of *services*, but the point of an API is to provide a *syntax* by which those services can be invoked.

There have been many APIs that different Operating Systems exposed for network programming, however nowadays a particular network API has gained popularity. That is the **Socket** API.

The Socket API abstracts the act of connecting to a network as **opening sockets**. A Socket is a point of connection for local applications to the network. This connection can be either a TCP connection `SOCK_STREAM`, a UDP connection `SOCK_DGRAM`, or a direct connection using IP and dealing with packets directly.

A client side code, does an **active** connection which uses functions such as this:

```C
#include <stdio.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <netdb.h>
#include <strings.h>
#include <string.h>
#include <unistd.h>

#define SERVER_PORT 5432
#define MAX_LINE 256

int main(int argc, char* argv[])
{
	FILE* fp;
	// A host entity
	struct hostent* hp;
	// information related to the socket address on the other host
	struct sockaddr_in sin;
	// name of the host in string (we'll get it from argv)
	char* host;
	// The buffer that we are going to send at the end
	char buffer[MAX_LINE];
	if(argc == 2)
	{
		host = argv[1];
	}
	else
	{
		fprintf(stderr, "usage: program-name host\n");
		return 1;
	}
	// It reads the string and resolves it to a host entity
	hp = gethostbyname(host);
	if (!hp)
	{
		fprintf(stderr, "Unknown host: %s\n", host);
		return 2;
	}
	// We have to set the memory of the sockaddr_in to zero before using it
	bzero((char*)&sin, sizeof(sin));
	// address family (AF_INET == IPv4)
	sin.sin_family = AF_INET;
	// Copies the IPv4 address of the desired host to the sin_addr
	bcopy(hp->h_addr_list[0], (char*)&sin.sin_addr, hp->h_length);
	// sets the destination port
	sin.sin_port = htons(SERVER_PORT);
	
	// create a socket connection
	int s = socket(PF_INET, SOCK_STREAM, 0);
	// Error checking
	if(s < 0)
	{
		perror("socket error");
		return 1;
	}
	// an active connection to the newly created socket
	if(connect(s, (struct sockaddr*)&sin, sizeof(sin)) < 0)
	{
		perror("connect error");
		close(s);
		return 1;
	}
	
	int len;
	// Reads maximum 255 characters from IO and fills our buffer
	while(fgets(buffer, sizeof(buffer), stdin))
	{
		buffer[MAX_LINE-1] = '\0';
		// length of the string we inputed
		len = strlen(buffer) + 1;
		// We then send the buffer to the socket connection
		send(s, buffer, len, 0);
	}
	return 0;
}
```

A server instead does a **passive** connection. For that the server opens a socket that waits for a connection. Once a connection has been detected, a new socket is created so that it can recieve the data from the connection.

```C
#include <stdio.h>
#include <sys/socket.h>
#include <sys/types.h>
#include <netinet/in.h>
#include <netdb.h>
#include <unistd.h>
#include <strings.h>

#define SERVER_PORT 5432
#define MAX_PENDING 5
#define MAX_LINE 256
int main()
{
	struct sockaddr_in sin;
	char buffer[MAX_LINE];
	bzero((char*)&sin, sizeof(sin));
	
	sin.sin_family = AF_INET;
	// the address of the other host can be anything so it's set to that
	sin.sin_addr.s_addr = INADDR_ANY;
	// The specific port that the server listens on
	sin.sin_port = htons(SERVER_PORT);
	// The socket connection that is meant to just listen
	int sock = socket(PF_INET, SOCK_STREAM, 0);
	if(sock < 0)
	{
		perror("socket fail");
		return 1;
	}
	// binds to the socket instead of a direct connect
	if(bind(sock, (struct sockaddr*)&sin, sizeof(sin)) < 0)
	{
		perror("bind failed");
		return 1;
	}
	// listening
	listen(sock, MAX_PENDING);
	
	int new_sock;
	socklen_t len = 0;
	while(1)
	{
		// Here the accept function returns a new socket, if a connection was 
		// established
		if((new_sock = accept(sock, (struct sockaddr*)&sin, &len)) < 0)
		{
			perror("accept failed");
			return 1;
		}
		// Then it recives the data from the new socket.
		while((len = recv(new_sock, buffer, sizeof(buffer), 0)))
			fputs(buffer, stdout);
		close(new_sock);
	
	}
}
```
## Performance

The functionality is all well and good, but if it's not performant then what's the point?

Two fundamental ways of measuring performance is:
* **Bandwidth**: or **Throughput**, is the number of bits over a period of time
* **Latency**: or **delay**, is how long it takes a message to travel from one end of a network to another. It's measured in units of time. 

You can also imagine bandwidth and a second of time as one unit of distance. For a 1-Mbps link is 1 microsecond, on a 2-Mbps link is 0.5 microsecond wide. 
That was on a physical link perspective, in logical process-to-process channels, bandwidth could be effected by how many times a software has to handle each bit of data.

**Sometimes it is important to know how long it takes to send a message from one end of a network and back, at those times the measure is the round-trip time (RTT) of the network.**

Latency has three components:

Latency = Propagation + Transmit + Queue

* Propagation: Distance / Speed of Light, this happens because nothing can go faster than the speed of light, not even bits of data.
* Transmit: Size / Bandwidth, Time to transmit a unit of data. 
* Queue: Queuing delays while switches forward packets through the network

If data is just one bit of data then Transmit and Queue have no effect.

**Data Rate**: How many bits a second can a link send in a particular time.

## Delay x Bandwidth Product

We can think of a network as a pipe. In this way, the bandwidth is the diameter and delay the length of the pipe. 

Bandwidth and delay product will give us the volume of that pipe.
You can in this way, calculate how many bits fit inside this pipe.

![[2025-02-19_22-01.png]]

It also corresponds to how many bits the sender must transmit before the first bit arrives. If a sender is waiting for a signal that its data has arrived, then the sender must have sent Bandwidth x RTT amounts of bits before hearing from the reciever.

If the reciever tells the sender to stop transmiting data, then the reciever will still be getting RTT x Bandwidth's worth of data before the sender responds.

Throughput is defined in this way:

*Throughput = TransferSize/TransferTime*
*TransferTime = RTT + 1/Bandwidth x TransferSize*


**Jitter**: The variation in latency is called Jitter. If the spacing between when packets arrive at the destination is varaible, the the delay will cause the packets to come in out of order, causing jitter.

# Chapter 2: Getting Connected

In the lowest layer of a network system, there are five general problems to solve to send bits through the network:

* **Encoding**: Finding a way to encode binary meaning on wires
* **Framing**: delineating a sequence of bits that was transmitted, these are often called **frames** so the problem is called framing.
* **Error Detection**: Errors inevitably occur when sending signals through wires.
* **Making the link appear reliable despite it creating errors half the time** (Sadly couldn't shorten this one)
* **Media Access Control**

## Classes of Links

Most practical links rely on some sort of electromagnetic radiation.

Two methods of characterizing links, **the medium** like copper wire as in Digital Subscriber Line (DSL), coaxial cable, optical fibres, or wireless links and the **frequency** mesaured in hz. 

The distance between a pair of adjacent maxima and minima of a wave is called the *wavelength*.
![[2025-03-31_12-49.png]]


**Modern long-distance links are almosqt exclusively replaced by fiber today.** They commonly use a technology called SONET. 

There are also links that you find inside a building or a campus, refered to as *local area networks (LAN)*. Ethernet is a popular example and has been dominant in these types of links.

Despite the wide variety of link types and all the complexity, different networking protocols can take advantage of that diversity and present a consistent view of the network to the higher layers.
## Encoding

We can think of the problem of encoding binary meaning on signals as two layers, the modulation where a signal like an on or off switch changes intensity. We'll think of a signal as simply having low or high signals, so we'll ignore the details. Now what's important is to find a way to encode meaning to these high and low signals.

The job of a **network adapter** is to encode and decode these types of meanings onto these signals. It contains a signaling component that encodes and decodes signals.
![[2025-03-31_13-05.png]]
## NRZ: non-return to zero

The obvious way is to correspond low with 0 and high with 1, that is NRZ. 
![[2025-03-31_13-05_1.png]]

The problem is that for long strings of 0s or 1s the signal can not change. There are two issues this causes:

* baseline wonder: the reciever keeps an avearge of the signal seen and uses it to distinguish between high and low, however consecutive 1s or 0s cause the average to change, making distinguishing them difficult.
* It is necessary for the signal to change to enable *clock recovery*. Every clock cycle the sender transmits and reciever recovers a bit, so the sender and reciever need to have precise clocks. The reciever derives the clock from the recieved signal, whenever it changes, the clock changes. With consecutive 0s or 1s, it will be difficult to decipher the clock cycle, making desynchronization more likely.

## NRZI: non-return zero inverted

A method of fixing the issues with NRZ is this. encode a 1 by transitioning from the current signal and encode a 0 as staying on the current signal. This solves consecutive 1s but obviously does nothing for consecutive 0s.

## Manchester

Another method is encoding 0 as a low-to-high transition and 1 being a high-to-low transmition. The clock can be effectively recovered at the reciever with this method.

The problem is that it doubles the rate at which signal transitions are made on the link which means the reciever has half the time to detect each pulse of the signal.

The rate at which the signal changes in a link is called the *baud rate*. In this method, the bit rate is half the baud rate so it is considered as 50% efficient.

![[2025-03-31_13-16.png]]

## 4B/5B

The idea is to break up long sequences of 1s or 0s by inserting extra bits into the stream.

Every 4 bits of actual data are encoded in a 5-bit code that is then transmitted to the reciever. The 5 bit codes are selected to have no more than one leading 0 and no more than two trailing 0s.So no pair of 5 bit codes sent will have more than 3 consecutive 0s. Then the signals are decoded using **NRZI**. Solving both the consecutive 1s and 0s problem as a result. This results in 80% efficiency.

![[2025-03-31_14-21.png]]

5 bits can represent more things because there 32 possible symbols and only 16 of them is used for the actual data.

11111 is used when the line is idle and 00000 is used when the line is dead, and 00100 is interpreted as halt. 7 of the remaining 13 codes are invalid because they invalidate the one leading and two trailing 0s rule. And the 6 others represent control symbols.

## Framing

When node A wishes to send a frame to node B, the adaptor on node B has to determine where the frame begins and ends in the signal and that is the central problem called the **Framing Problem**.


