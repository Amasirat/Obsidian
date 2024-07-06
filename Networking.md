
**Hosts** are computers that can send and receive data or traffic.
Hosts are divided into two groups:

* **Clients** who make requests for data
* **Servers** who send data to clients

**This relationship however is not static. A traditionally server host can become a client in relative to another server. For example when a web server is downloading new web-pages from a data server**

**IP Addresses** are 32 bit numbers identifying a host computer. These 32 bits are divided into 4 1 byte numbers and turned into a decimal number. each division is an *octet*. *example: 128.90.12.32*

Each octet is used hierarchically. For example each machine inside a company starts with 30, then each subdivision uses a different number like 30.20 or 30.30 and so on.

This process is called [[Subnetting]]

A **Network** automates the process of sharing data between different computers.

* Netwroks are logical groupings of different hosts.
* A network can contain multiple networks, these are called sub-networks or subnets.
* Networks can connect to each other.

Instead of manually connecting two networks to each other, we can create a central resource called the internet that all networks connect to.

In the past networks were made between hosts through wires however **Data decays as it travels through wires**

**Routing** is the process of sending data **between** networks.
**Switching** is the process of sending data **within** a network.
## Repeaters

**A repeater** regenerates signals. It basically acts as a buffer which buffes data through wires.

## Hubs

We can connect two hosts with one wire however if our hosts become greater we'll need to connect them with the other two hosts. With more hosts, we'll have connect even more hosts. That simply does not scale. To solve that issue we can use a central device which connects to all devices, with that we can simply connect a new host to that device and it will be connected to the network.

One such type of device is a **Hub**.

Hubs are basically a **Multi-port repeater**. They get data and send it to all hosts connected to it.
## Bridges

In a hub, everyone recieves each others data. To avoid doing that we can use a bridge to sepearte two networks from each other.

A bridge learns which hosts are on which side and they will send data to the other side only when it is required.

![[bridges.png]]

## Switches

They facilitate communication **within** a network.
Their purpose is to perform the act of **switching**.

They are a mixture of both **Hubs** and **Bridges**. They can stop data from trafficing to unnecessary hosts however they can learn which hosts are which on a port's basis. Meaning There's much more specificity and we can communicate only with two hosts.

![[switches.png]]

## Routers

They facilitate communication **between** networks.
Their purpose is to perform the act of **routing**.

![[router.png]]

The internet could be thought of as a bunch of routers throughout the world.


## OSI Model

