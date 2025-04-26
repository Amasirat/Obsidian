A series of interconnected computers are called a **Network**. It is a wide and fascinating topic and many systems and additional protocols are needed to make this network feasible and usable. The material here is a summarized version of the book *Computer Networks: A System's Approach*.

**Contents:**

[[#Chapter 1 Foundation]]
* [[#Classes of Applications]]
* [[#Requirements]]
* [[#Network Architecture]]
* [[#Socket Programming]]
* [[#Performance]]
[[#Chapter 2 Getting Connected]]
* [[#Classes of Links]]
* [[#Shannon-Hartley Theorem]]
* [[#Encoding]]
	* [[#NRZ non-return to zero]]
	* [[#NRZI non-return zero inverted]]
	* [[#Manchester]]
	* [[#4B/5B]]
 * [[#Framing]]
 * [[#Error Detection]]
 * [[#Reliable Transmission]]
 * [[#Multiple Access Networks]]
 * [[#Wireless]]
[[#Chapter 3 Internetworking]]
* [[#Switching and Bridging]]
* [[#Basic Internetworking (IP)]]
* [[#Routing]]

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
	// listening, it tells the kernel that sock is willing to accept 5 connections on waiting list (however this program itself can only deal with one connection at a time)
	listen(sock, MAX_PENDING);
	
	int new_sock;
	socklen_t len = 0;
	while(1)
	{
		// Here the accept function returns a new socket specific to that 
		// connection
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

## Shannon-Hartley Theorem
The Shannon-Hartley theorem is given by this formula:

$$
C = B\log_2 (1 + S/N)
$$

C is the achievable capacity measured in bits per second.
B is the frequency bandwidth in Hz.
S is the average signal power
N is the average noise power

S/N is also related to the signal to noise ratio or SNR, expressed usually with decibels:

$$
SNR = 10\log_10(S/N)
$$
A 30 dB would imply S/N = 1000. thus we will have:
$$
C = 3000\log_2(1001) = 30 kbps
$$

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

Each SONET frame is 9 x 90 byte rows. The first three columns of each row is overhead and the rest is the payload. 

The first 2 bytes of the frame is used to determine where it starts. bit stuffing is not used in SONET, so to guard against the payload having a similar bit sequence, the reciever looks for the special bit pattern consistently appearing once every 810 bytes. 

The overhead bytes are encoded using NRZ but to avoid baseline wonder and other problems with that approach, the bytes get XORed with a special bit pattern that results in a bit pattern that changes more often.

The frames said above are for STS-1 frames that have a max bandwidth of around 50 Mbps. STS-2 or STS-3 and STS-N have larger frames, integer multiples in fact. STS-2 is 2 times 810, STS-3 is 3 times, etc. This way frames of different lengths can be multiplexed together because 2 frames of STS-1 can fit in 1 frame of STS-2 for example.

A link that contains a concatenated frame of different STS-N frames is called STS-Nc, c for concatenated.
## Error Detection

bit errors are sometimes introduced into frames. Detecting errors is one part of the problem the other is dealing with it. One way is to simply retransmit the frame, the other is using algorithms that correct the error.

The goal of designing error detection algorithms is to maximize probability of detecting errors using only a small number of redundant bits.
### Detecting Errors

The basic idea is to send redundant information to help detect if any errors occured however if we just send two copies to make sure everything is in order we would have n bits of redundancy for n bits of data which is really inefficient.

We can however shorten that. We use a few algorithms that derive a piece of redundant bits from the data itself and then send it. If the reciever does the same algorithm and gets the same result then everything is in order otherwise a bit must have flipped.

General approach is to add extra redundant bits that are derived from the original message. If the message is corrupted, then by looking at the redundant information we can tell something went wrong.
### Two-Dimensional Parity

Used by BISYNC when it is transmitting ASCII characters. The idea is to have an even number of 1s in the data no matter what. The extra information is another bit, it checks to see if the number of 1s in a single row is even or odd, if it's odd, the redundant bit will be 1, otherwise it will be 0.

Now if the recieving side ever sees that there are odd number of 1s, taking in both the error-detecing codes and the data itself. It knows that an error has occurred.
![[2025-04-26_10-41.png]]
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

originaly made using coaxial cable of length up to 500 m. A **transciever** was a small device directly attached to the tap that detected when a line was idle and drove the signal when the host was transmitting, it also recieved incomming signals. It was connected to an adaptor which was connected to the host.
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

An Ethernet frame must contain at least 46 bytes of data, the reason for this minimum is that the frame must be long enough to detect collision. Each Ethernet frame has the following format:
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

Experience with Ethernet networks showed that it worked best in light workloads (30 percent). The capacity is wasted by collisions otherwise.

*The Ethernet cables can not have more than 2 repeaters between any pair of hosts and can be no more than 2500 m in length.*
## Wireless

Wireless differs from wired technologies:
* Bit errors are of even greater concern due to potential environment noise
* power is big issue
* inherently multi-access
* it's hard to control who recieves your signal so there are security threats.

Because wireless links share the same medium (air) the challenge is to share it so that they don't interfere with each other.

One way is to assign exclusive frequency band and restrictions in space. This assignment is usually done by government organizations. 

Another idea is *spread spectrum*, the idea is to change the frequency often using a psuedorandom number generator. Both sender and reciever have the same seed so they can change frequencies instead of staying on one. Basically **spread out** their frequency. Makes it unlikely that two signals would be using the same frequency for a long time.

A second spread technique is *direct sequence*, it adds redundant information to spread out the signal far more than it would have takenn based on the frame it was sending. it has n random bits called n-bit chipping code. They are generated using a pseudo random number generator known to both sender and reciever. For each bit, its exclusive OR is sent as well.
![[2025-04-26_11-35.png]]

There are two endpoint classifications:
* *base station* where there is one single base station that is connected to the internet via wire but it transmits a signal which lets client nodes connect to the internet through it as well.
* *ad hoc* topology or *mesh* networks. This way many non-base nodes connect to each other as peers and messages may be forwarded via a chain of peer nodes as long as they are in range
### 802.11 Standard
Wi-Fi is based on IEEE 802.11 standards, and is also called 802.11 sometimes.

There are 4 variants of the 802.11 standard which supports different daata rates:
* 802.11a using a variant of FDM called OFDM, data rates of 54 Mbps
* 802.11b is a physical layer which provides up to 11 Mbps
* The original 802.11 standard defined two radio-based physical layers, both providing up to 2 Mbps
* 802.11g also used OFDM and delivers 54 Mbps and is backward compatible with version b

It is common for commercial products to support all versions of the 802.11 standard. Although they define maximum bit rates they also support lower bit rates as well. At lower bit rates it is easier to decode transmitted signals in the presence of noise.

There of course needs to be redundant error correcting information (an XOR of the data), the higher resilience requires more information however at the cost of lowering effective data rate. The systems try to pick an optimal bit rate based on the environment. 

Collision detection in wireless networks is a lot more difficult than Ethernets.

The 802.11 standard uses CSMA/CA for Collision Avoidance in contrast to CD of Ethernets which meant collision detection.

Take for instance three nodes A, B, and C. A and B know they exist but A has no information about C because they do not overlap, but A and B do. Say that A and C want to send frames to B, because they doon't have any idea they exist, there's going to be interference in that case.


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

There are limitations to bridges, mainly one being that they are not scalable and the spanning tree algorithm scales linearly.

A method for increasing scalability is the virtual LAN (VLAN).

![[2025-04-21_21-15.png]]

An attractive feature of VLAN is that you can change the topology without moving any wires. If you want to add Z to VLAN 100 for instance, you can just change a configuration on bridge 2.

Bridges can only connect networks that support the same framing format (48 bit size payload), they can't for instance connect ATM networks.

Their main advantage however is that LANs can be connected without any host knowing about it. The main danger with that is a bridged network is more unpredictable than a single LAN, bridges can be congested or fail, resulting in variable latency.

## Basic Internetworking (IP)

An internet is a *logical* network built out of a collection of physical networks.

The internet protocol is the key tool used to build scalable heterogenous internetworks.
IP has defined what services it provides, it was important for IP to not wish for much and make the standard fairly lax in order to have more connections. For instance it does not need to have a specific latency, that way networks with higher latency can also be connected as well.
It has:
* An addressing scheme
* A Datagram delivery model

The network makes its best effort to get a packet to its destination, but it does not guarantee it.
This is called a *best-effort connectionless* service or an *unreliable service*. It is however the simplest thing to ask for an internetwork to do, do its best to deliver packets. If it was instead a *reliable* model that was running on an unreliable network, there would have be a lot of extra functionality that a router needed to have. The simplicity of the demands of IP is deemed as its key to success.

![[2025-04-22_11-20.png]]
The format of an IP packet is like this:
**First word**
* Version: Dictates what IP version it is, IPv4 or IPv6
* HLen: The length of the header in 32 bit words
* TOS: For the type of service, this header has changed over the years
* Length: Length of the datagram including the header, this one counts bytes rather than words. the maximum size of an IP datagram is 65,535 bytes.

**Second Word**
Contains information about **fragmentation** and **reassembly**. Although a packet can be 65,535 bytes long, the network may not support that, so a large packet may be fragmented.

**Third Word**
* TTL: Time to live, intent is to catch packets that have been going around in routing loops and discard them. It used to be seconds but now its a hop count
* Protocol: a demultiplexing key signifying which higher level IP to pass this to
* Checksum: calculated considering the entire IP header, uses the checksum algortihm discussed in [[#Checksum]]

The last two words are the source address and destination addresses.

The challenge with connecting different network types together using IP is that each one has a different idea of how big a packet should be. We can either choose a small enough packet to send that will fit all, or we can fragment and reassemble if necessary to pass through networks with different sized packets.

Each network type has a *maximum transmission unit* (MTU), which is the largest IP datagram that it can carry in a frame. This value is smaller than the largest packet size on that network.

If the MTU of a local network is smaller than the packet size of an IP datagram, it has to be fragmented and then reassembled inside the network. Otherwise there won't be a need for that. It's like a filter telling the outside network how big a packet should be, and then it will chop up anything larger to fit inside. The reverse is true as well, a larger sized packet can't leave the network unless it is fragmented out by the host.

If every fragment of a datagram reaches the reciever then they are reassembled, otherwise it will be discarded. IP does not try to recover from missing fragments.
![[2025-04-22_12-01.png]]

The M bit is set to one if there are more packets that need to arrive.
For example we have two routers with different MTU sizes, the first one has a larger MTU but smaller than the datagram size which will fragment the packet into two. Then the next router has an MTU of 532 bytes which will again fragment it into another two. All in all three fragmented packets will be recieved at the host.

The offset means what byte position this belongs to, however it is counted in 8-byte chunks not 1 byte chunks. So 512 / 8 = 64.

It is usually a good thing to avoid fragmentation as much as we can because it may lead to more complexity.

### Global Addresses

IP Addresses are hierarchical meaning they are made of several parts that correspond to one hierarchy of the network. IP addresses consist of two parts:
* Network: Identifies the network the host is attached. All hosts attached to the same network have the same Network part
* host: identifies each host uniquely

IP is written as four integer numbers from 0 to 255 separated by a dot. (Each representing one byte of the address)

e.g: 178.21.3.12

IPs used to be divided into different classes
* Class A: Signified by the MSD being a 0
* Class B: Signified by the MSD and the bit next to it being 1 and 0
* Class C: Signified by 110 in its MSDs
* Class D (For multicast groups)
* Class E (unused currently)
![[2025-04-23_10-57.png]]

This seemed a flexible design. There could only be 126 class A networks, each accomodating for about 16 million hosts. Class B allocates 14 bits for network and 16 bits for hosts meaning each class B can have 65,534 hosts. Class C dedicates 21 bits to network and 8 bits to host which means it can have only 254 attached hosts. The idea was that the internet would like this:
* Some Wide Area Networks (Class A)
* A moderate amout of medium sized networks (Class B)
* And a lot of LANs (Class C)
However it turned to not be as flexible as it was thought so nowadays IP addresses are not class based.

### Datagram Forwarding in IP

Forwarding is the process of taking a packet from input and sending it through the right output but *routing* is the process of building up the tables that allow correct output to be determined.

* Every datagram has the IP of the destination host
* The network part uniquely identifies a physical network inside the internet
* All host and routers that share the same network part can communicate each other on the same network
* Every physical network has at least one router which is at least connected to one other physical network

When a packet is sent from a source host, it passes through different routers until it reaches the destination host. Once a packet is sent to a router, it first checks to see if the network part of the packet is the same as one of the physical networks it is connected to. If it is not connected to the same physical network as the destination then it sends the datagram to another router. 

They have a choice of routers, so they need to pick the best one. The chosen router is called the *next-hop router*. It holds an ordered list of \<NetworkNum, NextHop\> which tells it what the next hop router will be for a particular network part. If it is not listed, it also has a default router that it sends to.

### Subnetting and Classless Addressing

The initial idea was that the Network part would represent only one physical network. There are however only a finite number of network numbers and as the internet grew larger, it became unsustainable. Another issue was that most people would rather get a Class B network instead of a Class C one because who knew if thir network would need more than 254 hosts?

Another problem with the class-based system was that, if we had a class C network with only 2 nodes, we would essentially be throwing away the remaining 253 addresses we could have had. So the class based IP scheme would use up most IP addresses much faster.

Another drawback is concerned with routing. Routers keep a network address in their memory, if network addresses become large, routers will run out of space, not to mention they become much slower to operate.

subnetting is giving the same network address to many physical networks and dividing their address space among them. A perfect scenario is a large campus that has many networks which are represented by the same general network address part. From the outside the only thing you need to know to reach any subnet of the campus is where it connects to the internet.

Now each host comes with a *subnet mask* as well as its IP address. If we want to know what network a host belongs to, we do a bitwise AND of its IP with its subnet mask.

IP Address: 128.96.34.15
Subnet Mask: 255.255.255.128

Result: 128.96.34.0, this called the subnetted address or subnet number

![[2025-04-24_16-18.png]]

A Host does a bitwise AND of the subnet mask and the IP address, if it belongs to the same subnet number it will do the connection directly otherwise the packet has to do a hop.

The forwarding table of a router will change with this new scheme, instead of \<NetworkNum, NextHop\> we're going to have \<SubnetNumber, SubnetMask, NextHop\>

The router ANDs the packet destination address with the SubnetMask for each entry. If it matches with one, that hop is done otherwise it goes through the default router.

A consequence of subnetting is that different parts of the internet see it very differently. routers outside the campus see the collection of networks as just 128.96 and they keep one entry but inside the campus, routers need to forward packets to the right subnets. This is an example of *aggregation* of routing information which is very important to scalability of these routing systems.

subnetting has a counterpart, *supernetting*, more often called *classless Interdomain Routing* (CIDR).

If we have a network that needs 256 hosts, we can give it a class B network address or many class C addresses. However what if instead of that we give *contiguous* class C networks for example from 192.4.16.x to 192.4.31.x. The top 20 bits are the same so we can say this is a 20 bit network number, something between a class B IP and a class C IP. We define this by a CIDR prefix like this:

128.4.16/X, the X means how many bits are the network part. For our example 128.4.16/20 will be the IP. Nowadays instead of class C IPs we say they are /24 IPs meaning 24 bits belong to the network (3 bytes). 192.168.1/24 is a common IP address for home networks in Iran for instance with 24 bits of network and a subnet mask of 255.255.255.0 meaning the entire space is used as one network. from 0 to 255 meaning 254 hosts can be connected (the first address 192.168.1.1 is reserved for the default gateway or the home router in other words and the last one 192.168.1.255 is used as a broadcast IP if any host wants to broadcast a packet to all hosts within the network)

With the advent of CIDR, during forwarding packets we can come across a situation where IP addresses overlap each other. For example we can have both 168.25.2.10 and 168.25 as entries. The rule here will be the longest match will win. If a packet destination for example is 168.25.2.10, then the first entry will be used otherwise if we have 168.25.3.1, because we don't have any direct entries with this IP number then the second entry will be used.

### Address Translation

IP datagrams only have information about their IP addresses, however to a host or router the only thing they understand is addressing scheme of that particular network. (MAC addresses for example), so we need a way of translating IP addresses into data-link level addresses.

There are manual ways of doing so but all of them are not scalable, so it's better to have a dynamic method of finding information about that.

Here comes the Address Resolution Protocol (ARP). The goal of ARP is to enable each host to build a table of mappings between IP addresses and link-level addresses.

If host A wants to send a packet to host B for example within one network it checks to see if it has the MAC address (or hardware address) of the destination IP. It checks a special table called the ARP cache.

This is an example, this is the arp cache of my computer, The IP address shown here is my default gateway A.K.A the ADSL router. The Device tells me which interface I used to access this gateway. wlp8s0 is the name of my WiFi interface. If I had connected a direct Ethernet cable to my router then the device would be en2p.
(I hid my router's MAC address with xx just in case)
```bash
IP address       HW type     Flags       HW address            Mask     Device
192.168.1.1      0x1         0x2         30:ef:xx:xx:xx:xx     *        wlp8s0
```

It is fair to assume a router also has an arp cache which dictates the MAC address of the routers they are connected to.

You could have manually added them but what if a particular IP interface decides to change its wifi card or adaptor? we need to find the hardware address dynamically. If host A does not have the desired hardware address, it sends a broadcast message through the network, this message is called an ARP request. 

An ARP request includes the IP and Hardware address of host A along with the destination IP, whoever in the network has the same IP address will respond with an ARP response to host A (This will be simple because host A's ARP request included its hardware address as well). Host B will also cache the hardware address of this IP address if it had not done so.

This way once host A recieves the ARP response it can cache that address and start communicating with host B. If instead of the host, it was connected to a multi-access network of switches, bridges, and routers, etc. The process will be the same but host A only has to worry about the router or switch that it is connected to. The rest of the job will be done by them.

Each ARP entry will be deleted if there has been no connection to it for 15 or so minutes. This way the entire process will repeat if the network changes, or hardware addresses change as well for example a change in router in a home network setting.

![[2025-04-24_19-23.png]]

### Host Configuration

Ethernet and hardware addresses are gobally unique and that's all we ask of them to be.

However IP addresses also need to represent their network structure, therefore we can't just set a fixed IP address to one host, so they need to be reconfigurable. 

The primary method of assigning IPs to hosts is using a protocol known as Dynamic Host Configuration Protocol. 

A DHCP server exists to store all the IP addresses that are assigned or are available to the network and a sophistacated use of a DHCP server would be to have it assign IP addresses to a newly booted host.

a newly attached host sends a DHCPDISCOVER broadcast message to all nodes on the network. In the simplest terms, one of these nodes will be the DHCP server but since it would still be unrealistic to have a DHCP server per one network, a realy agent whose job is to connect to a DHCP server does the job instead. Once a relay agent gets the broadcast message, it unicasts the message to the DHCP server, awaits the response and then sends it back to the host.

*Fun fact: The message is sent using the User Datagram Protocol (UDP)*

![[2025-04-24_19-48.png]]

The host puts its hardware address in the *chaddr* field, then the DHCP server replies by filling *yiaddr* (your ip address), other information like a default gateway can be included in *options*.

A DHCP server can not give an IP address permenantly to someone else since it will run out however it can't count on the client to send it back because a problem could have risen with it. That's why an IP address is *leased* and will be re-aquired once it runs out. A client therefore has to renew it periodically if it is functioning correctly.

Since IP now is dynamic it might make the job of the network admin harder in the case where a particular host is malfunctioning.

### Error Reporting

Internet Control Message Protocol (ICMP) defines a collection of error messages that can be sent in the case of an error in processing an IP Datagram or other things. Like when a detination is unreachable, the router sends an ICMP message back, or if a reassembly procces failed, etc.

The most interesting ICMP message is the *ICMP-Redirect*. If a router for example knows that for a specific connection another router is better it will send this to let the host know and send it through that instead.

ICMP echo messages are also behind the ping command, determining if a node is alive and reachable. *traceroute* is also provided by this protocol. 

### Virtual Networks and Tunnels



## Routing

The fundamental problem of routing is how switches and routers aquire the information in their forwarding tables.

There are several routing algorithms that exists, they generally don't scale very well however they help build the bedrock of the routing infrastructure of the internet collectively known as intradomain routing protocols or interior gateway protocols.

We shall first explore these algorithms on mid-sized networks like a university campus or company servers, etc.

In essence a routing problem is a shortest-path graph problem. Each node being a client, router, switch, or network, etc and each edge being a link, with its weight symbolizing our inclination for sending traffic on that network. We can simply calculate all shortest paths and save them however the problem with that is:

* Does not deal with node or link failure
* does not consider addition of new nodes or links
* it implies that edge costs do not change which is not true at all for networks

It is difficult to make centralized solutions scalable, so most used protocols use *distributed algorithms* to solve these problems. However that causes additional complexity to the problem. For instance two routers can have different ideas about the shortest path, both thinking their closer to the destination and decide to send packets to the other one, causing an infinite loop.

### Distance-Vector (RIP)

Each node constructs a one-dimensional array containing costs to all other nodes and distributes that to its immediate neighbors. A link that is down is assigned an infinite cost.

![[2025-04-24_20-59.png]]

For example, node A initially sets a cost of one to all its adjacent nodes. So A to B is 1, A to E is 1, A to F is also 1 as well as C.

then every node sends a message to all its direct neighbors with their current beliefs. For example, F knows it can reach G in one hop, so A figures out that it can go to G in 2 hops by hopping on F. 

When A gets the information that B can reach C in 1, it'll ignore it since it already had a shorter path by going to it directly and 1 < 2.

![[2025-04-24_21-03.png]]

![[2025-04-24_21-04.png]]

Two different circumstances when a given node decides to send a routing update to its neighbors:
* *periodic* update: Each node automatically sends an update message every so often
* *triggered* update: When a node notices a link failure or recieves an update from its neighbors

The system normally settles down pretty quickly after a change in network topology. An update is sent from the node that realizes the change first and this update propogates through out the graph.

There is however a specific circumstance that causes issues. For example, let's say node E from the picture goes down. Node E was only connected to A so if the link fails it will become completely unreachable. A advertises a distance of infinity but B and C still think they can reach in two hops, not registering the new information of infinity because infinity > 2. In this manner, B advertises that it can reach E in 3 hops since C can reach it in 2, node A then concludes it can reach E in 5, etc. This goes on forever but none of the nodes can ever figure out that E has become unreachable in actuality. This is called the *count to infinity*.

Routing Information Protocol (RIP) is a straight-forward implementation of the distance-vector algorithm. In an internetwork, routers advertise the cost of reaching *networks* instead of individual nodes. 

Routers running RIP send advertisements every 30 seconds.
![[2025-04-25_11-44.png]]

An RIP packet also supports multiple address families and not just IP.

RIP assumes a weight of 1 for each link, valid distances are 1 to 15 meaning the maximum size of a network is limited to fairly small networks with a maximum of 15 hops. 16 is deemed as infinity.

## Link State

The link state is another method of finding the shortest path. The idea is also similar to distance-vector in that we assume each node knows how to reach its neighbors and if we make sure that the totality is disseminated to every node, we can find the shortest path.

*reliable flooding* is the process of making sure thqat all nodes get a copy of the link-state information. 

Each node creates an update packet which is called the *link-state packet (LSP)* which has:
* ID of the node creating the LSP
* A list of its neighbors with the cost of each link
* a sequence number
* time to live (TTL)

The first two are necessary for route calculation and the last two are for reliability. Like making sure each node has the most recent copy of the information.

To make LSPs reliable they use a similar system of acknowledgements and timeouts. If Node X gets an LSP from Node Y, it first checks if it has gotten an LSP from this node before, if not it accepts it however if it has gotten one, it checks to see if its sequence number is greater than its previous LSP from this node. If it is it'll update its information and if not it will discard the LSP. if it has accepted an LSP, it will send a copy of that LSP to all its neighbors except Y. In turn, X's neighbors will do the same until the most recent copy of the link-state information reaches all nodes.

*A node does not send a copy to the one it has recieved an LSP because that way the broadcasting can end at some point*

One of link-state's goals is to quickly update every node on the current state of the network and do not let old information stay for too long.

Each time a node generates a new LSP, it increments the sequence number by 1, these sequence numbers are not expected to wrap.

If a node goes down and comes back up, the sequence number starts from 0.

Aside from that a TTL is stored, whenever an LSP moves through each node, it gets decremented by 1. That is there to make sure old information dies eventually and gets deleted. If TTL = 0, a node knows that it has to delete that LSP. This is to avoid the count to infinity problem.

After all LSPs from every node reaches it, the node starts to do the route calculations. It uses a variant of the Dikjstra's Algorithm to find the shortest path called the *forwarding search*

Each node saves two lists of confirmed and tentative. It first adds itself to the confirmed list (D, 0, -), because it knows it can reach itself in 0 hops (duh!)

Whenever a new confirmed entry is added, check the LSP of the newly confirmed node, therefore we check what the currently confirmed node knows about its neighbors.
The information about the node that we get, if there has been no entry in tentative, we just add, but if we did have, we check if the sum of reaching that node from the current is smaller than the cost it previously has in tentative,

then we add or update the tentative list accordingly. After that we choose the entry from tentative with the least cost to join the confirmed list. rinse and repeat.

Here's a live example of how it works, here's an example graph:
![[2025-04-25_12-20.png]] And then here are the steps it takes:

![[2025-04-25_12-25.png]]

The difference between distance-vector and link-state is that, distance-vector tells its neighbors the latest information it thinks it has of the whole network however link-state only advertises the information it knows *for sure* that it has, I.E the cost to reach its own neighbors.

### OSPF

The Open Shortest Path First Protocol (OSPF) was created under the Internet Engineering Task Force (IETF). The SPF comes from the alternate name of link-state. This protocol includes a few extra features:
* Authentication of routing messaages: if a node lies about the state of the network, routing will become problematic, therefore it's good to authenticate these messages. At first a simple 8-byte password was used, it would not stop malicious actors however avoided misconfiguration or casual attacks. later stronger cryptographic techniques were used.
* Additional hierarchy: OSPF partitions a domain into different areas, a router does not need to know how to reach every network within that domain, it might be enough for it to know how to get to that *area* instead. 
* Load Balancing: allows multiple routes to the same place be assigned the same cost. This way we can make better use of the network capacity.
![[2025-04-25_12-35.png]]
The above is an OSPF packet format, some important properties include:
* **Type**: means which type of message this is, 1 for a simple hello to know if a link is in working order, the remaining types are used to request, send, and acknowledge reciepts of link-state messages.
* **SourceAddr**: What address has sent this packet
* **Checksum**: the entire packet except the authentication is protected by a 16 bit checksum
* **Authentication Type**: means which system of authentication is used. 1 for simple password 
* **Authentication**: This one then contains the actual authentication being used.

![[2025-04-25_12-41.png]]

An LSP is OSPF is called an LSA and its format is above