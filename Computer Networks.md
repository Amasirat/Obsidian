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
* Modularity and Separation of concerns: Makin it so that you can easily change technologies related to on layer of the design without impacting the lower layers.

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
And the subnet layer is layer 2.

Internet Architecture has three features:
* Does not imply strict layering, meaning an application is free to bypass the defined transport layer (TCP and UDP), and to directly use IP.
* IP serves as the focal point of the architecture. This creates an hourglass shape in the protocol graph (Shown above)
* For a new protocol to be officially recognized, there must be both a **specification** and the existence of at least one or two **implementations**.

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

# Chapter 2: Getting Connected

