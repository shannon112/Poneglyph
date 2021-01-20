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
|           |  I2C | SPI |  RS-232 RS-485 (UART based)|  CAN          | Ethernet |  CANOpen | EtherCAT | Modbus |
| --------  | ---- | --- | -------------------------- | ------------- | -------- | -------- | -------- | ------ |
| 1.Physical|  I2C |  SPI  |   RS-232 RS-485          |  ISO 11898-2  | IEEE 802.3  |  
| 2.Data-Link| I2C |  SPI  |    UART                  |  ISO 11898-1  | IEEE 802.3  |
| 3.Network|
| 4.Transport|
| 5.Session|
| 6.Presentation|
| 7.Application|


# Reference
[1] https://en.wikipedia.org/wiki/OSI_model  
[2] http://linux.vbird.org/linux_server/0110network_basic.php#whatisnetwork_what  
