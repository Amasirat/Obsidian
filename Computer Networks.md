A series of interconnected computers are called a **Network**. It is a wide and fascinating topic and many systems and additional protocols are needed to make this network feasible and usable. The material here is a summarized version of the book *Computer Networks: A System's Approach*.

**Contents:**

[[#Chapter 1 Foundation]]
* [[#Classes of Applications]]
* [[#Requirements]]
* [[#Network Architecture]]
* [[#Socket Programming]]
[[#Chapter 2 Getting Connected]]
[[#Chapter 3 Internetworking]]

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

### Scalable
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
* **Reliable Transmission**: We must make it seem like these links are reliable despite them being otherwise in their nature (given the errors and all)
* **Media Access Control**
## Classes of Links

Most practical links rely on some sort of electromagnetic radiation.

Two methods of characterizing links, **the medium** like copper wire as in Digital Subscriber Line (DSL), coaxial cable, optical fibres, or wireless links and the **frequency** mesaured in hz. 

The distance between a pair of adjacent maxima and minima of a wave is called the *wavelength*.
![[2025-03-31_12-49.png]]

**Modern long-distance links are almost exclusively replaced by fiber today.** They commonly use a technology called SONET. 

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

* baseline wonder: the reciever keeps an average of the signal seen and uses it to distinguish between high and low, however consecutive 1s or 0s cause the average to change, making distinguishing them difficult.
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

5 bits can represent more things because there are 32 possible symbols and only 16 of them is used for the actual data.

11111 is used when the line is idle and 00000 is used when the line is dead, and 00100 is interpreted as halt. 7 of the remaining 13 codes are invalid for actual data because they invalidate the one leading and two trailing 0s rule. And the 6 others represent control symbols.
## Framing

When node A wishes to send a frame to node B, the adaptor on node B has to determine where the frame begins and ends in the signal and that is the central problem called the **Framing Problem**.

### Byte-Oriented Protocols

One of the oldest approaches. It is to view each frame as a collection of bytes and not bits. Here are a few protocols developed using that approach:

#### Sentinel-Based Approaches

* Binary Synchronous Communication (BISYNC): Developed by IBM in late 1960s
* Point-to-Point (PPP): The more recent and widely used one.

BISYNC uses special characters to dictate the start and end of the body of a message. using STX (meaning start of text) and ETX (End of text)
 ![[BISYNC frame.png]]

There's also a special field labeled CRC (cyclic redundancy check) used to detect transmission errors.

SYN is a special character that stands for synchronization which dictates the beginning of the frame. 

If there's a special character in the body of the message, it is escaped similar to how it's done in C for special characters in strings. This is called character stuffing in networking jargon. Stuffing extra characters into the data portion basically.

There are additional header fields which are used for link-level reliable delivery algorithms and such.

PPP is used for point-to-point links. Similar to BISYNC.

![[2025-04-06_11-19.png]]

Flag field is the start of text character which is 01111110

Address and Control fields contain default values usually.

Protocol field is used for demultiplexing. Identifies the high-level protocol such as IP or IPX.

Checksum field is either 2 or 4 bytes long.

Payload size is the body of the message and its size can be negotiated.

The negotiation of the sizes is done by a protocol called Link Control Protocol (LCP). LCP sends control messages encapsulated in PPP frames. These messages are denoted by an LCP identifier in the Protocol field. And then turns around and changes PPP's format based on the information in the control messages. 

It also is involved in establishing connection between two peers when both sides detect that communication is possible.
#### Byte-Counting Approaches

* Digital Data Communication Message (DDCMP): used in DECNET
The practice of ditating the size of the frame and then counting the bytes until the frame ends. Similar to how manipulating arrays is like in C.

![[2025-04-06_11-26.png]]

One danger with this approach is that count field will get corrupted due to transmission errors. 
Once a framing error has occured and it is detected by accumulating as many bytes as the count field indicates and using the error detection field, it will skip and wait for the next SYN character. Because of that if the count field fails there can be back to back framing errors.
### Bit-Oriented Protocols

It is not concerned with bytes and instead views the frame as a collection of bits that can be ASCII characters, a pixel, an instruction, or whatever.

 The Synchronous Data Link Control (SDLC) developed by IBM is an example. It was later standardized by the ISO as the High-Level Data Link Control (HDLC).

HDLC denotes both start and end of frame with a bit sequence of 01111110 (It is also transmitted when the link is idle for too long to have both devices synchronized)

This protocol uses a technique known as *bit stuffing* to escape bits that are special symbols. If the sender wants to send a '01111110' bit sequence, it has to put a 0 right before the sixth 1: '011111010'

This way the reciever can reason as to what this bit means. if it sees 5 consecutive 1s and then sees a 0, it knows to remove the 0 and treat it as the data otherwise it'll know that it is an end sequence. If however the last 0 is not seen it can tell that the frame has errors and the whole frame is discarded.

![[2025-04-06_11-46.png]]

A side effect of bit stuffing and character stuffing is that the size of a frame is dependent on the data so we can't have fixed frame sizes for all types of data.

### Clock-Based Framing

A third approached used by the Synchronous Optical Network (SONET) standard. First proposed by Bell Communications Research (Bellcore) then developed under the American National Standards Institute (ANSI), it has been for many years the dominant standard for long-distance transmission of data over optical networks.

It addresses both the encoding and framing problems and also multiplexing several low-speed links onto one high-speed link which phone companies deal with a ton.

## Error Detection

bit errors are sometimes introduced into frames. Detecting errors is one part of the problem the other is dealing with it. One way is to simply retransmit the frame, the other is using algorithms that correct the error.

The goal of designing error detection algorithms is to maximize probability of detecting errors using only a small number of redundant bits.
### Detecting Errors

The basic idea is to send redundant information to help detect if any errors occured however if we just send two copies to make sure everything is in order we would have n bits of redundancy for n bits of data which is really inefficient.

We can however shorten that. We use a few algorithms that derive a piece of redundant bits from the data itself and then send it. If the reciever does the same algorithm and gets the same result then everything is in order otherwise a bit must have flipped.

### Two-Dimensional Parity

TODO: Watch 3Blue1Brown's video on this.



### Checksum
A checksum is named as such because it involves a summation. 
This algorithm isn't used on the link level.

The idea is **you add up all the words that are transmitted and then transmit the result of the sum, being the checksum**.

It's good that it doesn't occupy much space however it's strength as an error detection algorithm is bad. Sometimes if we have more than one error, it'll detect nothing. The reason it's sometimes used is it's easy to implement.

```C
ushort cksum(ushort* buf, int count)
{
	register u_long sum = 0;
	while(count--)
	{
		sum += *buff++;
		if(sum & 0xFFFF0000)
		{
			sum &= 0xFFFF;
			sum++;
		}
	}
	return ~(sum & 0xFFFF);
}
```

### Cyclic Redundancy Check

The most common technique is known as **cyclic redundancy check (CRC)**, it is used in most link-level protocols.
It uses string math to achieve the goal of detecting errors with as little redundant data as possible, using a branch of math called *finite fields*.

We think of a message that has n+1 bits as an n-powered polynomial:
for 10110
$$
M(x) = 1x^4+0x^3+1x^2+1x^1+0x^0 = x^4 + x^2 + x^1
$$
It works by both sender and reciever choosing a C(x) that is a polynomial degree of degree k. This is usually decided as a part of a protocol or based on some assumption.

When a sender transmits a message M(x), what is sent is the (n+k+1) bit messages which is called P(x). What has to happen is that P(x) is divisible by C(x), if not then an error has occured.

* B(x) can be divided by a factor of C(x) if the degree of B(x) is higher than C(x)
* The same if they are the same degree
* The remainder is obtained by doing an XOR on the matching coefficients of two polynomials (Here it means those that are 1)

The polynomial $$x^3+1$$ divided by $$x^3 + x^2 + 1$$ will be:
$$
XOR operation: 0x^3 + 1x^2 + 0 = x^2
$$

This is used for CRC in this way:
* Muultiply M(x) by x<sup>k</sup>: add k zeros at the end making it a **zero-extended message** T(x).
* Divide T(x) by C(x) and find the remainder
* Then subtract the remainder from T(x)

What is left is a message that is exactly divisible by C(x). The resulting message will be M(x) with the remainder from T(x) appended at the end.

The reciever then divides it by C(x), if it's not 0 it concludes that an error occured. it either requests for a resend or 

## Reliable Transmission

In the case that errors occur, we can now detect them using CRC, while theoratically we can fix them sometimes, most of the time the overhead is not worth it and most of them should be discarded. A link-level protocol that wants to deliver frames has to be able to recover discarded frames.

This is accomplished using a combination of two mechanisms:

* acknowledgements
* timeouts

An acknowledgement or ACK for short is a message with no body (control frame) that is sent to a peer that states it's gotten a previous frame. 

If a sender does not get an ACK after some time, it retransmits that frame. This action of waiting for a reasonable amount of time is called a *timeout*.

This whole strategy is called automatic repeat request (ARQ). Here are a few ARQ algorithms:

### Stop-and-Wait

It is the simplest scheme. The sender waits for an acknowledgement after it has sent a frame before sending the next frame, if it does not recieve one it retransmits the previous frame.
![[2025-04-13_11-46.png]]

In the case when the frame has been recieved but the ACK does not come in time for the sender's timeout window, the frame is retransmitted even though the reciever has gotten the previous frame, meaning duplicate frames occur.

The solution is that the header for the stop-and-wait protocol includes a 1-bit sequence number. Either a 0 or a 1. This sequence alternates every frame, so if it's repeating a frame, it sends the frame with the same sequence so the reciever knows that it's not a new frame. It still acknowledges it but discards it.

The shortcomming is that only 1/8th of the capacity of the link is used. If we have to wait for an ACK after sending only 1 frame, then we won't be able to send many frames in quick succession making things slower.

For example there's a 1.5-Mbps link with a 45-ms round trip time. delay x bandwidth is 67.5 kb or 8 KB. since the sender can only send one frame per RTT (assuming a frame size is 1 KB), the maximum sending rate will be:
Bits per frame / Time per Frame: 1024x8 / 0.045 = 182 kbps, one-eigth of the link's capacity of 1.5 Mbps.
### Sliding Window

The sliding window can allow us for instance to send 8 frames of 1 KB size in a link that has a delay x bandwidth of 8 KB, the ninth frame will be sent just as the ACK for the first frame arrives.
![[2025-04-14_19-16.png]]

It's basically an algorithm that uses a sliding window mechanism to manage frame sending and acknowledgements.

Each frame is numbered sequentially.
For the sender, it saves these variables:
* SWS: Send window size, which is the size of the window of packets that it can without ACKs
* LFS: The number of the last frame it sent
* LAF: Last acknowledged frame, is the most recent frame that got an ACK back.
It then checks for this: LFS-LAF <= SWS
If it's not true, then it stops sending until LAF or LFS gets to an acceptable amount. This means that if a large number of packets are sent without acknowledgements back, SWS prevents it from sending more until they arrive. It retransmits each frame based on its timeout scale.

The reciever has a similar scheme:
* RWS: recieve window size, which means the number of out of order frames it can hold onto in its buffer.
* LFR: the number of the last frame recieved
* LAF: largest acceptable frame, it's calculated by doing LFR + RWS
It then checks for LFS - LAR <= SWS. If this does not check out for a recieving frame, it gets discarded.

Sequence numbers can't grow infinitely large however so we need to reuse them. So they can also wrap around. For example, because of a 3-bit header, after the seventh frame the next one will be 0.

Another condition that is added will be this:

**When RWS == SWS:** 
$$SWS < (MaxSeqNum + 1) / 2$$

This alterantes between the two halves of the number space just like how stop-and-wait alternated between 0 and 1.

This rule also doesn't account for when frames are recieved out of order.

#### Implementation

SKIPPED (FOR NOW)

The sliding window protocol fulfils 3 roles:
* Sending frames on an unreliable network efficiently
* Perserving order of frames
* flow control: a mechanism where it can throttle the sender to keep the sender from over-running the reciever. you can augment the protocol by not only acknowledging the senders frames but also telling it how many frames it has room to recieve.
### Concurrent Logical Channels

It's used in the ARPANET and it is an alternative to the sliding window, the frames sent are not kept in any particular order however it manages to keep the pipe full while using the stop-and-wiat algorithm. it also does not imply anything about flow control.

The idea is to multiplex several logical channels onto a single link and run stop-and-wait algorithm on each of these channels. It can keep the link full because a different frame can be outstanding on each of the several logical channels.

It keeps a 3 bit state:
* a boolean stating if the link is busy
* the 1-bit sequence number to use for the next frame sent
* next sequence number to expect a frame that arrives on this channel

In practice ARPANET supported 8 logical channels over each ground link. 16 over satellite.
## Multiple Access Networks

The Ethernet is a multiple access network a set of nodes sends and recieves frames on a shared link. The more general name for it is Carrier Sense, Multiple Access with Collision Detect (CSMA/CD)

Carrier Sense means all nodes can distinguish between an idle and a busy link. 

Collision Detect means a node listens as it transmits and can detect when a frame being transmitted has interfered with a frame being transmitted by another node.

The core idea is to control when each node can transmit frames through the medium (The medium here being a coaxial cable)

Today's Ethernets are usually point-to-point so the multiple access part of the technology is not used that often anymore, they are mostly used by wireless networks.

### Physical Properties

originaly made using coaxial cable of length up to 500 m. A **transciever** a small device directly attached to the tap detected when a line was idle and drove the signal when the host was transmitting, it also recieved incomming signals. It was connected to an adaptor which was connected to the host.
![[2025-04-14_21-02.png]]

Multiple Ethernet segments can be joined together by a *repeater* that forwards digital signals like an amplifier.

![[2025-04-14_21-04.png]]

a multiway repeater is also called a *hub*. It just repeats what it hears in more ports other than one.

hubs and repeaters forward all signals and **terminators** at the end of each segment absorb the signal and keep it from bouncing back.

**The Orginal Ethernet used Manchester encoding but nowadays they all use 4B/5B or 8B/10B.**

Data transmitted by one host reached all hosts in the network.
## Access Protocol

Because all hosts have access to one shared medium, it is important to deal with the competition that is resulted from that (hosts being in the same *collision domain*)

The algorithm that allows access control is also called *Multi Access Control (MAC)*.

Each Ethernet frame has the following format:
* Preamble(64 bits): that is attached to the frame by the host adaptor and removed by the reciever adaptor
* CRC (32 bits): also attached by the adaptor and then removed
* Destination Address (48 bits)
* Source Address(48 bits)
* Type(16 bits): 
* Body of the frame
![[2025-04-14_21-21.png]]

The address is not an IP but an Ethernet or MAC address. Each adaptor is assigned to a globally unique MAC address with the following format, 8 bit numbers separated by colons: 

**81:04:e4:2b:2d:ae**

This is assigned at the hardware level and can not be changed and each adaptor has completely unique MAC address.

Each frame is transmitted to every Ethernet adaptor but an adaptor passes on only those frames that are addressed to it.

An Ethernet address consisting of all binary 1s is a **broadcast** message. All adaptors pass those.

Ethernet generally passes frames that:
* Are addressed to its own address
* addressed to the broadcast address
* addressed to a multi-cast address if it has been instructed to listen to that
* All frames, if it has been placed in promiscuous mode

Here is the transmitter algorithm:

When a line is free, the packet is sent with no negotiation. However if every adaptor does that, there could be conflict, therefore some protocols use a probabilistic system of transmission. A network with a probability of transmitting a packet is called a p-persistent network where 0 <= p <= 1. The Ethernet is a *1-persistent* protocol.

because there is no control it is possible for more than one adaptor to send data at the same time. This is called the frames colliding on the network. Each sender can tell if a collision is in progress. in order for an adaptor to know if collision is at foot, it sends 96 bits of a runt frame called a jamming sequence, 64 bit preamble and 32 bit the jamming sequence.

The benefits of Ethernet is:
* Cable is cheap
* Easy to administer and add a new host

Experience with Ethernet networks showed that it worked best in light workloads (30 percent).

*The Ethernet cables can not have more than 2 repeaters between any pair of hosts and can be no more than 2500 m in length.*
## Wireless

Wireless differs from wired technologies:
* Bit errors are of even greater concern due to potential environment noise
* power is big issue
* inherently multi-access
* it's hard to control who recieves your signal so there are security threats.

Wi-Fi is based on IEEE 802.11 standards, and is also called 802.11 sometimes.

There are 4 variants of the 802.11 standard which supports different daata rates:
* 802.11a using a variant of FDM called OFDM, data rates of 54 Mbps
* 802.11b is a physical layer which provides up to 11 Mbps
* The original 802.11 standard defined two radio-based physical layers, both providing up to 2 Mbps
* 802.11g also used OFDM and delivers 54 Mbps and is backward compatible with version b

It is common for commercial products to support all versions of the 802.11 standard.

There of course needs to be redundant error correcting information (an XOR of the data), the higher resilience requires more information however at the cost of lowering effective data rate. The systems try to pick an optimal bit rate based on the environment. 
# Chapter 3: Internetworking

We know how to connect a few hosts together and how to send data to them. Now how do we scale it globally and into multiple networks? That is what *internetworking* tries to solve.
## Switching and Bridging

The core job of a switch is to take packets that arrive and *forward* them so that they reach their destination. There are two methods of finding the right output of the switch to achieve that, one of them is **connectionless** and the other is **connection-oriented or virtual circuit**, a less common third approach is **source routing**. Most of the modern internet runs on connectionless methods.

A switch adds a **star topology**:

* Large networks can be built by interconnecting a number of switches that are in a star topology. 
* We can connect switches to each other using point-to-point links
* adding a new host does not reduce performance of the network

Every host on the swtiched network has its own link to the switch. 

In general switched networks are considered as more scalable than shared-media networks like Ethernet. 

In terms of its OSI model, it belongs to the network layer (layer 3)
### Datagrams

You just include all the data that a switch needs to make a decision of where to send the data.
![[2025-04-19_12-12.png]]

Whenever a packet wants to move from a host to another, the switch consults the forwarding table or routing table, which tells it which port number that host can be reached with.

* When a host sends a packet it has no way of knowing if a destination host can even get its message
* A host can send packets anywhere it wants at any time because any packet that turns up at a switch can be immediately forwarded. That's why datagram method is called *connection-less*
* Each packet forwarded is unique from each other, thus two successive packets send to and from the same hosts can follow different paths.
* a switch or link failure may not hava any serious effects if it is possible to find an alternate route around it.
### Virtual Circuit Switching

This is a connection-oriented model. it requires setting up a **virtual connection** from source to destination before data is sent.

There's a two stage process:
* connection setup
* data transfer
In the connection setup, we establish a *connection state*, it consists of an entry inside a "VC table".
* a unique id (VCI)
* incoming interface
* outgoing interface
* a different VCI that can be used for outgoing packets

**A VCI is not globally unique. It is only in use in that particular local link. However a new connection has to create a new VCI that is unique to that connection on that link**

If a packet arrives at an incoming interface (port), if that packet has the designated VCI, then the packet should be sent to the outgoing interface (port) and the outgoing VCI value is placed in its header

There are two ways of establishing this:
* By an administrator (Permanent Virtual Circuit, PVC)
* Or by signalling. A host can send messages into the network. The resulting virtual circuits are called *switched* (SVC: Switched Virtual Circuit)

Once a switch sees an incoming VCI and interface, it knows it belongs to a particular host. It knows how to send it to other hosts because of the table it has. It sends it however with its outgoing interface as the new incoming one. Once the destination host gets a packet with a particular packet VCI, it knows which host it's from.

This will get impossible to do by hand so even PVCs of large networks are usually done using signalling.

To start a connection, host A sends a setup message to a switch. Let's assume the switches know how to get to host B, so the setup reaches host B. Each switch aside from sending the message, updates its VCI Table for that particular incomming VCI. Each switch now knows that if it gets an incomming VCI, they have to send it on port x with a value of its outgoing VCI. *They all pick their own VCIs*. 

Once the setup message reaches Host B, it sends an acknowledgement to its switch, the acknowledgement gets passed onwards until it reaches host A which tells it to use a particular VCI that a switch has chosen to communicate to host B.

Once host A no longer wants to send packets, it sends a teardown message. The switch removes the relevant entry and forwards it to other switches. 

Several things to note:

* There is at least one RTT worth of delay before data is sent, because it has to wait for the connection request.
* The per packet overhead is reduced because each local link has identifiers which are only unique on that link. 
* If a connection fails, a new one has to be reestablished because the old VCI tables get cleared out to free up storage.
* the process of deciding which link to forward a request requires some sort of a *routing algorithm

*Congestion* in switches is when a switch will start dropping packets because it has run out of buffer memory to store them.

A benefit of VCI is that it avoids the problem of congestion because the creation of the connection means that it has enough resources to make it, and after the first connection setup, it is obvious where the packet has to go. However this also means that it is conservative and the switches might be underutilized.

The simple datagram seemingly can run into congestion problems. 

There have been many virtual circuit technologies: X.25, Frame Relay, and Asynchronous Transfer Mode (ATM).

ATM is the most well-known virtual circuit-based networking technology. It came at a time when Ethernet and Token rings were starting to feel slow and the telephone industry embraced it as well.

An ATM Packet is known also as an ATM Cell is shown below.
![[2025-04-20_23-03.png]]

VPI is Virtual Path Identifier and VCI is the Virtual Circuit Identifier. The whole thing is the same as the VCI we talked about. The reason to break it into two parts was to implement hierarchy. 

It uses fixed size cells (53 bytes) for two reasons:
* If they're the same size, parralelism can take place easier with each part finishing in a similar time.
* It is easier to build hardware to do simple jobs and knowing the packet size ahead of time makes its job easier

## Source Routing

The idea is that the source tells it the topology of the network. Take the below example:
![[2025-04-20_23-17.png]]

A packet from Host A has to first go out through port 1 on switch 1 then 0 and then 3. The packets then have an ordered list of port numbers and the switches read the rightmost port to know which port to send the packet through. After they have read that number they will rotate the ordered list, that way the next switch can read the next port to go through. This way a (3, 0, 1) list will turn into a (1, 0, 3) list when it wants to return from host B to host A.

It assumes that host A knows the topology enough to give precise directions like this. We also can't predict how big the header has to get. 

There are three ways of dealing with the headers:
* Rotate them
* strip them (this way host B won't know how to send it back as opposed to the last method)
* Contain a pointer that points to the current port
![[2025-04-20_23-23.png]]

Source routing can be used in both datagram and virtual circuit networks as well. 

There are also two categories of source routing, strict where a strict route to a different host has to be included and loose where it only specifies a set of nodes. The good thing about that is it helps limit the information that a source needs to create a source route.

## Bridges and LAN Switches

Widely used in campus and enterprise networks. There is a class of switches used to forward packets between LANs such as Ethernets which are also referred to as *bridges*.

In order to connect two Ethernet networks together, we can put a node with an Ethernet adaptor on it operating in promiscuous mode. That is a bridge!

A collection of LANs connected like this is called an extended LAN. Simple bridges are ones that get incoming frames and copy them on the outgoing interface.

An optimization we can make is to not copy frames if they are intended for hosts within one network. Bridges learn that information by inspecting the source address and record which port they arrived in, that way it knows where they live. So a table of Hosts and ports are created. Each entry has a timeout associated to it and if they do not hear from each, they are discarded.

The above strategy works until there is a loop in the extended LAN. Why would that be?

* managed by more than one administrator
* Another more likely reason is that it is done in purpose to provide redundancy in case of a link failure

This is solved by having the bridges running a spaning tree algorithm. A spanning tree is a sub graph that has no loops. If we think of the extended LAN as a graph, its bridges can run a spanning tree algorithm to determine a subgraph with no loops. The idea is to restrict the bridges so that they should not send frames over specific ports so that a loop is not caused.

Each bridge is given an ID. The bridge with the smallest ID is elected as the root bridge. The root bridge always forwards frames out over all its ports. Then each bridge computes the shortest path to the root and notes which one of its ports are in the path, finally all bridges designate one bridge that will be responsible for forwarding frames to the root bridge. 

We however need all the bridges to agree on one configuration, so the below algorithm is used:
Each bridge claims to be root and advertises that using information like this: (Root Node, distance to root, Sending Node), for example the bridge which is called B3 will send this: (B3, 0, B3). If they get a better message than what they recorded they will adjust it. A better message is:

* A root with a smaller ID
* The same root but with a shorter distance
* The same root and distance but smaller ID of the sender

After it discards the old information it will add 1 to the distance it had recorded.

Once the root has been chosen the others stop transmiting configuration messages and only the root does so. If it for whatever reason stops sending them, the algorithm restarts again to elect a new root node.

*Although the algorithm can reconfigure in case of failure, it can't due anything about rerouting in the case of congestion or anything like that*

There are limitations to bridges, 

