要開始看這些傳輸協定之前，必須先瞭解我們到底在講一個communication中的哪一段，不然很容易失焦，像是你不會想問TCP/IP用的是哪一種型號的接頭，或是拿著DP9的RS-232接頭問說這條線IP是多少，而透過OSI 7 Layer Model，一個「概念模型」，可以幫助了解和分類各個通訊協定所在的分工位置多上層多底層或是他到底是一個多大scale的東西橫跨幾層。

# Open System Interconnection Model (OSI Model)
開放式系統互聯模型（Open System Interconnection Model, OSI）是一種概念模型，由國際標準化組織提出，一個試圖使各種電腦在世界範圍內互連為網路的標準框架。  
The Open Systems Interconnection model (OSI model) is a conceptual model that characterises and standardises the communication functions of a telecommunication or computing system without regard to its underlying internal structure and technology. Its goal is the interoperability of diverse communication systems with standard communication protocols.  
The model partitions the flow of data in a communication system into seven abstraction layers, from the physical implementation of transmitting bits across a communications medium to the highest-level representation of data of a distributed application. Each intermediate layer serves a class of functionality to the layer above it and is served by the layer below it. Classes of functionality are realized in software by standardized communication protocols. [1][2]

|      |   Layer  |  Protocol data unit (PDU)  |   Function   | Examples |
| ---- | -------- | -------------------------- | ------------ | -------- |
| Host layers |	7.Application | Data |	High-level APIs, including resource sharing, remote file access | DNS, FTP, HTTP, SMTP, Telnet, DHCP |
| Host layers | 6.Presentation| Data |  Translation of data between a networking service and an application; including character encoding, data compression and encryption/decryption | MIME, XDR, ASN.1, ASCII, PGP |
| Host layers | 5.Session 	  | Data |  Managing communication sessions, i.e., continuous exchange of information in the form of multiple back-and-forth transmissions between two nodes | Real-time Transport Protocol (RTP) |
| Host layers | 4.Transport 	|Segment, Datagram| Reliable transmission of data segments between points on a network, including segmentation, acknowledgement and multiplexing | TCP, UDP | 
| Media layers |3.Network 	| Packet | 	Structuring and managing a multi-node network, including addressing, routing and traffic control | IPv4, IPv6 |
| Media layers |2.Data link |	Frame  |	Reliable transmission of data frames between two nodes connected by a physical layer | IEEE 802.3(Ethernet), IEEE 802.11(WIFI), MAC, ISO 11898-1(CAN) |
| Media layers |1.Physical 	| Bit, Symbol |	Transmission and reception of raw bit streams over a physical medium | IEEE 802.3(Ethernet), IEEE 802.11(WIFI), USB, Bluetooth, RS-232, ISO 11898-2(CAN)|

<img src="https://raw.githubusercontent.com/shannon112/Notes/main/OSI_model/OSI_data_pyramid.png" width=400>

# Common Protocol in Robotics Application: Overview
|           |  I2C | SPI |  RS-232(UART based) | RS-485 (UART based) |  CAN          | Ethernet  |  USB  |  CANOpen | EtherCAT | 
| --------  | ---- | --- | ------------------- | ------------------- | ------------- | --------  | ------| -------- | -------- | 
| 1.Physical|  I2C |  SPI  | RS-232(EIA/TIA-232-F) | RS-485(EIA/TIA-485-A)|  ISO 11898-2  | IEEE 802.3 | USB 1.1/2.0/3.0/3.1 | CAN | Ethernet |
| 2.Data-Link| I2C |  SPI  |    UART               | UART                 |  ISO 11898-1  | IEEE 802.3  | USB 1.1/2.0/3.0/3.1 | CAN | Ethernet MAC, Mailbox/Buffer Handling, Process Data Mapping, Extreme Fast Auto-Forwarder |
| 3.Network|   | | | | | | USB 1.1/2.0/3.0/3.1 |
| 4.Transport| | | | | | | USB 1.1/2.0/3.0/3.1 |
| 5.Session|
| 6.Presentation|
| 7.Application|
| * Speed | 100/400 Kbps |	a few Mbps | 9.6/19.2/38.4/57.6/115.2 Kbps |  a few mbps | 10/10/100/1000/400000 Mbps | ? | 1.5(12)/480/5000/10000 Mbps | - | - |
| * Topology | Bus | Star | 1-to-1 | Daisy Chain with terminating resistors |  Daisy Chain with terminating resistors |  Star / Daisy Chain with terminating resistors | Star | - | - |
| * Max # devices | 112(7bits)/1008(10bits) | multiple | 2 | 64(4-wires) / 32(2-wires) | 128 | 2(point-to-point)/30(chain) | 2(point-to-point)/127(per hub) | - | - |
| * Max length    | 1m | 0.2m | 50m | 1200m | ? | 500/100/15/100/1000up m | 2~5 m | - | - |
| * Transfer mode | Half-Duplex | Full-Duplex | Half-Duplex | Full-Duplex(4-wires) / Half-Duplex(2-wires) | Half-Duplex | Full-Duplex | Full-Duplex(3.x) / Half-Duplex(1.x/2.x) | - | - |
| *  Multi-Master Support | Y | N | N | N | Y | Y | N | - | - |


# RS-232
Overview: https://www.youtube.com/watch?v=eo9dbnrpspM
- Recommended Standard 232 (RS-232) is a standard originally introduced in 1960 for **serial communication** transmission of data. 
- It formally defines signals connecting between a **DTE (data terminal equipment) such as a computer terminal, and a DCE (data circuit-terminating equipment or data communication equipment), such as a modem**. 
- The standard defines the electrical characteristics and timing of signals, the meaning of signals, and the physical size and pinout of connectors. **OSI Layer 1 Physical Layer**. EIA standard RS-232-C[3] as of 1969 defines:
  - Electrical signal characteristics such as voltage levels, signaling rate, timing, and slew-rate of signals, voltage withstand level, short-circuit behavior, and maximum load capacitance.
    > The voltage levels that correspond to logical one and logical zero levels for the data transmission and the control signal lines. Valid signals are either in the range of +3 to +15 volts(0,space) or the range −3 to −15 volts(1,mark) with respect to the "Common Ground" (GND) pin; consequently, the range between −3 to +3 volts is not a valid RS-232 level. For data transmission lines (TxD, RxD, and their secondary channel equivalents), logic one and zero is represented. Control signals have the opposite polarity: the asserted or active state is positive voltage and the de-asserted or inactive state is negative voltage. Examples of control lines include request to send (RTS), clear to send (CTS), data terminal ready (DTR), and data set ready (DSR). 
  - Interface mechanical characteristics, pluggable connectors and pin identification.
    > 1.According to the standard, **male connectors have DTE pin functions, and female connectors have DCE pin functions**. Other devices may have any combination of connector gender and pin definitions. Many terminals were manufactured with female connectors but were sold with a cable with male connectors at each end; the terminal with its cable satisfied the recommendations in the standard.  
    > 2.The standard recommends the D-subminiature 25-pin(D-sub 25, DB25). Most devices only implement a few of the twenty signals specified in the standard, so connectors and cables with fewer pins are sufficient for most connections, more compact, and less expensive. Personal computer manufacturers replaced the DB-25M connector with the smaller DE-9M(DB-9) connector (see Serial port).  
    > 3.Presence of a 25-pin D-sub connector does not necessarily indicate an RS-232-C compliant interface.  
    > 4.The standard does not define a maximum cable length, but instead defines the maximum capacitance that a compliant drive circuit must tolerate. A widely used rule of thumb indicates that cables more than 15 m (50 ft) long will have too much capacitance, unless special cables are used. By using low-capacitance cables, communication can be maintained over larger distances up to about 300 m (1,000 ft).For longer distances, other signal standards, such as RS-422, are better suited for higher speeds.   
  - Functions of each circuit in the interface connector.
  - Standard subsets of interface circuits for selected telecom applications.
    > A minimal "3-wire" RS-232 connection consisting only of TX, RX, and GND, is commonly used when the full facilities of RS-232 are not required. Even a "2-wire" connection (data and ground) can be used if the data flow is one way (for example, a digital postal scale that periodically sends a weight reading, or a GPS receiver that periodically sends position, if no configuration via RS-232 is necessary). When only hardware flow control is required in addition to two-way data, the RTS and CTS lines are added in a "5-wire" version. 
  - The standard does not define such elements as the character encoding (i.e. ASCII, EBCDIC, or others), the framing of characters (start or stop bits, etc.), transmission order of bits, or error detection protocols. It is **OSI Layer 2**. The character format and transmission bit rate are set by the serial port hardware, typically a **UART**, which may also contain circuits to convert the internal logic levels to RS-232 compatible signal levels. The standard does not define bit rates for transmission, except that it says it is intended for bit rates lower than 20,000 bits per second.
- The current version of the standard is **TIA-232-F** Interface Between Data Terminal Equipment and Data Circuit-Terminating Equipment Employing Serial Binary Data Interchange, issued in 1997. Electronic Industries Association (EIA). The RS-232 standard had been commonly used in computer serial ports and is still widely used in **industrial communication devices such as Programmable Logic Controller(PLC), Human-Machine Interface (HMI), motor driver**.
- A serial port complying with the RS-232 standard was once a standard feature of many types of computers. Personal computers used them for connections not only to **modems**, but also to **printers, computer mice, data storage, uninterruptible power supplies, and other peripheral devices**.
- RS-232, when compared to later interfaces such as RS-422, RS-485 and Ethernet, has **lower transmission speed, short maximum cable length, large voltage swing, large standard connectors, no multipoint capability and limited multidrop capability**.
- In modern personal computers, USB has displaced RS-232 from most of its peripheral interface roles. Few computers come equipped with RS-232 ports today, so one must use either an external **USB-to-RS-232 converter** or an internal expansion card with one or more serial ports to connect to RS-232 peripherals. 
- Nevertheless, thanks to their simplicity and past ubiquity, RS-232 interfaces are still used—particularly in industrial machines, networking equipment, and scientific instruments where a **short-range, point-to-point, low-speed, inexpensive** wired data connection is fully adequate. 
- Hardware Interrupt:
  - Ring Indicator (RI) is a signal sent from the DCE to the DTE device. It indicates to the terminal device that the phone line is ringing. In many computer serial ports, a **hardware interrupt** is generated when the RI signal changes state. Having support for this hardware interrupt means that a program or operating system can be informed of a change in state of the RI pin, without requiring the software to constantly "poll" the state of the pin.
- Flow control (data) § Hardware flow control:
  - The **Request to Send (RTS) and Clear to Send (CTS)** signals were originally defined for use with half-duplex (one direction at a time) modems such as the Bell 202. These modems disable their transmitters when not required and must transmit a synchronization preamble to the receiver when they are re-enabled. **The DTE asserts RTS to indicate a desire to transmit to the DCE, and in response the DCE asserts CTS to grant permission**, once synchronization with the DCE at the far end is achieved. Such modems are no longer in common use. There is no corresponding signal that the DTE could use to temporarily halt incoming data from the DCE. Thus RS-232's use of the RTS and CTS signals, per the older versions of the standard, is asymmetric. 
  - There is a need for symmetric, bidirectional flow control. They redefined the RTS signal and defined a new signal **"RTR (Ready to Receive)"**, shares the same pin as RTS. In this scheme, commonly called **"RTS/CTS flow control" or "RTS/CTS handshaking"** (though the technically correct name would be "RTR/CTS"), the DTE asserts RTR whenever it is ready to receive data from the DCE, and the DCE asserts CTS whenever it is ready to receive data from the DTE. Unlike the original use of RTS and CTS with half-duplex modems, these two signals operate independently from one another. This is an example of hardware flow control.
- Limitations:
  - The large **voltage swings** and requirement for positive and negative supplies increases **power consumption** of the interface and complicates power supply design. The voltage swing requirement also **limits the upper speed** of a compatible interface.
  - Single-ended signaling referred to a **common signal ground** limits the **noise immunity and transmission distance**.
  - **Multi-drop** connection among more than two devices is not defined. While multi-drop "work-arounds" have been devised, they have limitations in speed and compatibility.
  - The standard does not address the possibility of connecting **a DTE directly to a DTE, or a DCE to a DCE**. **Null modem cables** can be used to achieve these connections, but these are not defined by the standard, and some such cables use different connections than others.
  - The definitions of the **two ends of the link are asymmetric**. This makes the assignment of the role of a newly developed device problematic; the designer must decide on either a DTE-like or DCE-like interface and which connector pin assignments to use.
  - **The handshaking and control lines** of the interface are intended for the setup and takedown of a dial-up communication circuit; in particular, the use of handshake lines for **flow control** is not reliably implemented in many devices.
  - No method is specified for **sending power to a device**. While a small amount of current can be extracted from the DTR and RTS lines, this is only suitable for low-power devices such as mice.
  - The DB25 connector recommended in the standard is large compared to current practice.

# RS-485
Overview: https://www.youtube.com/watch?v=3wgKcUDlHuM
- RS-485, also known as TIA-485(-A) or EIA-485, is a standard defining the electrical characteristics of drivers and receivers for use in **serial communications systems**. 
- **Electrical signaling is balanced, and multipoint systems are supported**(multidrop communications, up to 32 devices).**Inexpensive, faster, longer distance, less noise**. Such as a younger brother of RS-232.
- Digital communications networks implementing the standard can be used effectively over long distances and in electrically noisy environments. Multiple receivers may be connected to such a network in a linear, multidrop bus. These characteristics make RS-485 useful in industrial control systems and similar applications such as Variable Frequency Drives(VFD or motor drives), PLC, HMI. 
- Using the same differential signaling over twisted pair as RS-422. RS-485, like RS-422, can be made **full-duplex by using four wires**. 
- It is generally accepted that RS-485 can be used with data rates up to **10 Mbit/s** or, at lower speeds, distances up to **1,200 m (4,000 ft)**. As a rule of thumb, the speed in bit/s multiplied by the length in metres should not exceed **10^8**. Thus a **50-meter cable should not signal faster than 2 Mbit/s**.
- RS-485 only specifies the electrical characteristics of the generator and the receiver. **OSI layer 1 the physical layer**. It does not specify or recommend any communications protocol; Other standards define the protocols for communication over an RS-485 link. The foreword to the standard references The Telecommunications Systems Bulletin TSB-89 which contains application guidelines, including data signaling rate vs. cable length, stub length, and configurations.
  - Section 4 defines the electrical characteristics of the generator (transmitter or driver), receiver, transceiver, and system. These characteristics include: definition of a unit load, voltage ranges, open-circuit voltages, thresholds, and transient tolerance. 
  - It also defines three generator interface points **(signal lines); A, B and C.** The data is transmitted on A and B. C is a ground reference. This section also defines the **logic states 1 (off, mark) and 0 (on, space)**, by the polarity between A and B terminals. If A is negative with respect to B, **the state is binary 1. The reversed polarity (A +, B −) is binary 0**. The standard does not assign any logic function to the two states. 
  - In addition to the A and B connections, an optional, third connection may be present (the TIA standard requires the presence of a common return path between all circuit grounds along the balanced line for proper operation) called SC, G or reference, the common signal reference ground used by the receiver to measure the A and B voltages. This connection may be used to limit the common-mode signal that can be impressed on the receiver inputs. The allowable common-mode voltage is in the range −7V to +12V, i.e. ±7V on top of the 0-5V signal range. Failure to stay within this range will result in, at best, signal corruption, and, at worst, damage to connected devices. 
  - Connector types are not specified. Refer to RS-232, using DB9 connector. Cable is using balanced interconnecting cable (shielded cable) to prevent noise.

# Ethernet
Overview: https://www.youtube.com/watch?v=HLziLmaYsO0 
- Ethernet is a family of wired computer networking technologies commonly used in **local area networks (LAN)**, metropolitan area networks (MAN) and wide area networks (WAN). 
- It was commercially introduced in 1980 and first standardized in 1983 as **IEEE 802.3, covering OSI physical layer and data-link layer**.
- Ethernet has since been refined to support **higher bit rates, a greater number of nodes, and longer link distances, but retains much backward compatibility**. Over time, Ethernet has largely replaced competing wired LAN technologies such as Token Ring, FDDI and ARCNET.
- Physical layer: About the cabling, signaling and devices. 
  - The original Ethernet uses **coaxial cable (10BASE5)** as a shared medium, using a thick and stiff coaxial cable up to **500 meters** (1,600 ft) in length. Up to 100 stations can be connected to the cable using vampire taps and share a single collision domain with **10 Mbit/s** of bandwidth shared among them. The system is difficult to install and maintain. https://en.wikipedia.org/wiki/10BASE5
  - The newer Ethernet variants use **twisted pair (10BASE-T, 100BASE-TX, and 1000BASE-T)** with **8P8C modular connectors**. Correspondingly, Cat3 10Mbit/s and 100meters, Cat5e 100Mbit/s and 15meters, Cat5 1000Mbit/s and 100meters. https://en.wikipedia.org/wiki/Ethernet_over_twisted_pair
  - 8P8C commonly referred to as RJ45 in the context of Ethernet and category 5 cables, RJ-45 originally refers to a specific wiring configuration of an 8P8C connector. A telephone-system-standard RJ45 plug has a key which excludes insertion in an un-keyed 8P8C socket. https://en.wikipedia.org/wiki/Modular_connector
  - The newest **Fiber optic** (glass or plastic) with **SFP and SC connector**, are also very popular in larger networks, offering high performance, better electrical isolation and longer distance, run at 400Gbit/s and longer than 1000 meters. https://en.wikipedia.org/wiki/Optical_fiber
  - SC(Subscriber/Square/Standard Connector) and SFP(Small Factor/Form Pluggable): https://en.wikipedia.org/wiki/Optical_fiber_connector and https://en.wikipedia.org/wiki/Small_form-factor_pluggable_transceiver
  - Ethernet devices are consisting of any device which have a Network Interface Card (NIC), USB based or PCI based. https://en.wikipedia.org/wiki/Network_interface_controller
  - Repeaters and hubs. For signal degradation and timing reasons, coaxial Ethernet segments have a restricted size. Somewhat larger networks can be built by using an Ethernet repeater and hub. https://en.wikipedia.org/wiki/Ethernet_hub 
  - Switch and Router act as a director of the network. Connecting multiple devices and enabling communication between all the devices. https://en.wikipedia.org/wiki/Network_switch and https://en.wikipedia.org/wiki/Router_(computing)
  - Gateway and Bridge are used to connect multiple Ethernet network together, allow communication across them. Gateway connects two dissimilar network together. Bridge connects two similar network together. https://en.wikipedia.org/wiki/Gateway_(telecommunications) and https://en.wikipedia.org/wiki/Bridging_(networking)
- Data-link layer: Including framing, media access control(MAC) and logical link control(LLC).
  - Logical Link Control(LLC): The LLC establishes paths for data on the Ethernet to transmit between devices. The LLC sublayer provides multiplexing mechanisms that make it possible for several network protocols to coexist within a multipoint network and to be transported over the same network medium. Since bit errors are very rare in wired networks, Ethernet does not provide flow control or automatic repeat request (ARQ), meaning that incorrect packets are detected but only cancelled, not retransmitted (except in case of collisions detected by the CSMA/CD MAC layer protocol). Instead, retransmissions rely on higher layer protocols. https://en.wikipedia.org/wiki/Logical_link_control
  - Media Access Control(MAC): The Media Access Control uses hardware addresses that are assigned to Network Interface Cards (NIC) to identify a specific computer or device to show the source and destination of data transmissions. The 48-bit MAC address was adopted by other IEEE 802 networking standards, including IEEE 802.11 (Wi-Fi), as well as by FDDI. https://en.wikipedia.org/wiki/Medium_access_control
  - The frame begins after the start frame delimiter with a frame header featuring source and destination MAC addresses and the EtherType field giving either the protocol type for the payload protocol or the length of the payload. The middle section of the frame consists of payload data including any headers for other protocols (for example, Internet Protocol) carried in the frame. The frame ends with a 32-bit cyclic redundancy check(CRC), which is used to detect corruption of data in transit. Notably, Ethernet packets have no time-to-live field, leading to possible problems in the presence of a switching loop. https://en.wikipedia.org/wiki/Ethernet_frame and https://en.wikipedia.org/wiki/Cyclic_redundancy_check.
  - CSMA/CD is used as a standard for Ethernet to reduce data collisions and increase successful data transmission. The algorithm first checks to see if there is traffic on the network. If it does not find any, it will send out the first bit of information to see if a collision will occur. If this first bit is successful, then it will send out the other bits while still testing for collisions. If a collision occurs, the algorithm calculates a waiting time and then starts the process all over again until the full transmission is complete. https://en.wikipedia.org/wiki/Carrier-sense_multiple_access
- Ethernet is widely used in homes and industry, and interworks well with wireless Wi-Fi technologies. The Internet Protocol is commonly carried over Ethernet and so it is considered one of the key technologies that make up the Internet. 

# EtherCAT
https://www.youtube.com/watch?v=tYAl2jkaB8Q
ethercat is a ethernet based communication protocol, ethernet for control automation technology(CAT), with several mechanism benefit to automation application

# CAN
https://www.youtube.com/watch?v=FqLDpHsxvf8
The Controller Area Network system, electronic control systems (ECS) are connected by CAN bus. In automative system ECUs can be engine control unit, airbags,  audio system, ...etc(more than 70 ECUs in a morden car). 1.Low cost 2.Centralized 3.Robust 4.Efficient 5.Flexible, Bosch invent it. CAN msg format 8 key parts. Related protocol: SAE J1939, OBD-II, CANopen. It is a data link layer(ISO 11898-1) and physical layer(ISO 11898-2).

# CANOpen
https://www.youtube.com/watch?v=DlbkWryzJqg
CANOpen is a CAN based communication protocol, but 6 new important concept. https://www.csselectronics.com/screen/page/canopen-tutorial-simple-intro
https://www.canopensolutions.com/english/about_canopen/CANopen-and-the-OSI-reference-model.shtml#

# I2C, SPI, UART
Read the details in:
- Comparison: https://shannon112.blogspot.com/2021/01/embedded-system-serial-communication.html
- SPI: https://shannon112.blogspot.com/2021/01/embedded-system-serial-peripheral.html
- I2C: https://shannon112.blogspot.com/2021/01/embedded-system-inter-integrated.html
- UART: https://shannon112.blogspot.com/2021/01/embedded-system-universal-asynchronous.html

# Reference
[1] https://en.wikipedia.org/wiki/OSI_model  
[2] http://linux.vbird.org/linux_server/0110network_basic.php#whatisnetwork_what  
[3] https://en.wikipedia.org/wiki/RS-232  
[4] https://en.wikipedia.org/wiki/D-subminiature  
[5] https://en.wikipedia.org/wiki/Multidrop_bus  
[6] https://en.wikipedia.org/wiki/RS-485  
[7] https://en.wikipedia.org/wiki/Shielded_cable  
[8] https://en.wikipedia.org/wiki/Balanced_line  
[9] https://en.wikipedia.org/wiki/Ethernet  
[10] https://www.geeksforgeeks.org/network-devices-hub-repeater-bridge-switch-router-gateways/  
[11] https://en.wikipedia.org/wiki/USB  
[12] USB3.1: https://www.synopsys.com/designware-ip/technical-bulletin/protocol-layer-changes.html  

[] Comparison1: http://ucpros.com/work%20samples/Microcontroller%20Communication%20Interfaces%203.htm  
[] Comparison2: https://blog.servo2go.com/2013/09/23/comparing-canopen-and-ethercat-fieldbus-networks/  
[] Comparison3: https://blog.csdn.net/djl806943371/article/details/89331048  
