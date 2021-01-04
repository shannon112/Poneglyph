# Intro:
The Serial Peripheral Interface (SPI，發音類似spy) is a synchronous serial communication interface specification used for short-distance communication, primarily in embedded systems. The interface was developed by Motorola in the mid-1980s and has become a de facto standard (a custom or convention that has achieved a dominant position by public acceptance or market forces). Typical applications include Secure Digital cards (SD card) and liquid crystal displays (LCD). [1]

# Character:
- Master-Slave-Based, can have only one master and can have one or multiple slaves.
- Full duplex communication, send and receive simultaneously, both master and slave can transmit data at the same time.
- Synchronous communication, the data from the master or the slave is synchronized on the rising or falling clock edge.
- High speed, SPI > I2C > UART, so it is suitable for SDcard module.

# Interface
- MISO：Master In, Slave Out 主入從出
- MOSI：Master Out, Slave In 主出從入
- SCK/ SCLK/ Clock：Serial Clock 串列時脈。
- SS/ CS：Slave Select/Chip Select 從選。LOW表示裝置可以與Master通訊；HIGH表示不與Master通訊。只是普通的digital pin，default是pin10，但可以設成別的，not restricted to pin10，any digital pin is ok。
- SDO: Serial Data Out. An output signal on a device where data is sent out to another 
- SDI: Serial Data In.
<img src="https://raw.githubusercontent.com/shannon112/Notes/main/SPI/SPI_interface.png" width=100>

# Reference
[1] https://en.wikipedia.org/wiki/Serial_Peripheral_Interface  
[2] https://www.analog.com/media/en/analog-dialogue/volume-52/number-3/introduction-to-spi-interface.pdf  
[3] 
