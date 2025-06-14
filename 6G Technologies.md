6G is the next generation of 5G networks and is expected to improve on 5G in these criteria:
* Frequency
* data rate
* peak data rate
* latency
* core network
* mobility
![[2025-05-23_11-21.png]]
The main feature of 6G, are high data rates and high energy efficiency and intelligent decision making. The expected peak device data rate of 6G is 1Tbps, Latency is reduced to 10 to 100  microseconds, Maximum spectral efficiency will be 100 bps/Hz, Mobility will be up to 100 km/h, and network efficiency will rise to 100 times of current 5G networks.\[2]

With the rise of IoT devices, 6G is expected to be designed with these devices in mind. Greater speeds and more power efficiency can make IoT devices more useful in the long run as well.

Along with the improvements in performance and data rate, it is expected that 6G networks will use AI algorithms and Machine Learning techniques in multiple layers of their architecture. That is because the more complex systems become, the harder they will be to manage and make decisions in using humans alone. With AI's great data analysis tools, certain levels of automation can be achieved.

Some select technologies that have been considered for improving communications are as follows:
* Terahertz
* Index Modulation
* Cell-free massive MIMO
* space-air-ground-sea integrated network (SAGSIN)
* blockchain-based network
* Visible Light communication
* Device-to-Device (D2D)

Here we will discuss some of these technologies.
## Terahertz

![[2025-05-30_21-09.png]]
Currently the RF band which is used by current generation networks are not enough to fulfil the requirements of 6G. Therefore the bandwidth was increased to allow more data to be carried through wirelessly. 

Some challenges in its way is its propagation loss, molecular absorption at a higher frequency, the high penetration loss for solid objects. etc \[2]
## Index Modulation

It is a new technique for encoding and modulating bits based on the idea of selecting different resources based on the data we want to encode. (resources being subcarriers, antennas, etc) It can send spare bits of information along with each transmission without any additional power consumption.\[1] 

![[2025-05-27_15-41.png]]
A general overview of the process can be stated like this: 
Bit signals are divided into two parts, the index part which signifies which resource the next bit sequence needs to be sent through, and the rest are symbol bits that signify the data.

The core idea is to give extra information by the choice of a resource index, that way the reciever can infer the extra bits through the implication. For this to work both the transmitor and the reciever are given information about what data is implied by the choice of each resource.

Categories of IM include:
* Spatial-domain (SD-IM)
* Frequency-domain (FD-IM)
* Time-Domain (TD-IM)
* Channel Domain (CD-IM)
### Challenges

Desite the massive gain in data rate achieved by this method, there are some considerable drawbacks. An important one is the fact that unused resource entities are kept empty, therefore utilization will not be maximized. This is troubling because at scale, these empty resources might compound on themselves. Another challenge is the complicated nature of the detection algorithm used with this method. 
## AI in Networking

AI has been used in different parts of networks and will become a hallmark of the 6G network future. Many algorithms are used to optimize and perhaps solve previously unsolved problems in the realm of networking.
### Supervised Learning 

Can be used in both physical and network layer. We can use supervised learning for channel states estimation, channel decoding, etc. For the networking layer, this technique can be used in caching, traffic classification, delay mitigation, and use cases like that.\[1]
### Unsupervised Learning

Some examples of techniques using unsupervised learning are optimal modulation, channel-aware feature-extraction in the physical layer and also in routing, traffic control and parameter prediction in the network layer.\[1]
## Full-Duplex

Full-duplex refers to technology that enables two systems to transmit and recieve data simultaneously, while in half-duplex systems, this simply can not be done. For obvious reasons, this is inefficient because systems spend more time by first transmitting and then recieving data. In 4G and even 5G, communications can not be done on the same frequency. However 6G will utilize FD in a complete fashion, which will as a result greatly increase efficiency of sending and recieving data.\[1]
## Usecases

v
### Autonomous Vehichles
6G can provide capabilities to use distributed intelligence and commuication between other vehicles to improve road saftey, traffic prediction and by extension the entire driving experience.\[2]

There are different types of unmanned vehicles:
* Unmanned Ground Vehicles(UGV)
* Unmanned Aerial Vehicles(UAV)
* Unmanned Underwater Vehicles(UUV)
* Driver-less trains
Fully automonous systems like these require massive amounts of data per second, therefore the latency is of cruicial importance. That is an important challenge that could be solved by 6G technologies.\[2]
## Industry 5.0
Industry 5.0 is the newest revolution of industry which uses smart technologies. Machines are capable of commuicating with other machines themselves (M2M). They can self-diagnose and predict any future failures using AI algorithms. This was introduced by 5G networks however 6G will further promote this concept due to its increased capabilities. 

![[2025-05-27_18-02.png]]
Picture from \[2]
### Smart Cities

A smart city is an urban area equipped with various sensors that collect data to manage and improve the infrastructure and services of the area.\[2]

5G started the smartification of cities, however at this point in time, a complete smart city has not been developed. There are pockets of locations that are smart however that has not extended to the entire city. 6G can provide more powerful capabilities for that vision to become reality.

## Challenges of 6G



### Security and Privacy

The complexity of 6G networks creates a challenge for security immensely. This includes many aspects of security such as access control, detection of malicious activities, authentication and encryption. As of now there have been some measures explored to solve these challenges such as access control mechanisms, authentication mechanisms, malicious activity detection algorithms, and etc. However there are still many avenues that require further research.



