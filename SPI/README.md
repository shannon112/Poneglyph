# Intro:
The **Serial Peripheral Interface** (SPI，發音類似spy) is a synchronous serial communication interface specification used for short-distance communication, primarily in embedded systems. The interface was developed by Motorola in the mid-1980s and has become a de facto standard (a custom or convention that has achieved a dominant position by public acceptance or market forces). Typical applications include Secure Digital cards (SD card) and liquid crystal displays (LCD). [1]

# Character:
- **Master-Slave-Based**, can have only one master and can have one or multiple slaves.
- **Full duplex communication**, send and receive simultaneously, both master and slave can transmit data at the same time.
- **Synchronous communication**, the data from the master or the slave is synchronized on the rising or falling clock edge.
- **High speed**, SPI > I2C > UART, SPI devices support much higher clock frequencies compared to I2C interfaces, so it is suitable for SDcard module.  
- **Short distance**, around 20cm, UART > I2C > SPI.

# Interface
- **MISO**：Master In, Slave Out 主入從出. Transmits data from the master to the slave.
- **MOSI**：Master Out, Slave In 主出從入. Transmits data from the slave to the master.
- **SCK/ SCLK/ Clock**：Serial Clock 串列時脈. The device that generates the clock signal is called the master.
- **SS/ CS**：Slave Select/Chip Select 從選. This is normally an active low signal and is pulled high to disconnect the slave from the SPI bus. When multiple slaves are used, an individual chip select signal for each slave is required from the master. LOW表示裝置可以與Master通訊；HIGH表示不與Master通訊。只需普通的可輸出digital的pin, not restricted to arduino pin10，any digital pin is ok. 
- **SDO**: Serial Data Out 串列資料輸出. An output signal on a device where data is sent out to another.
- **SDI**: Serial Data In 串列資料輸入.

<img src="https://raw.githubusercontent.com/shannon112/Notes/main/SPI/SPI_interface.png" width=400>

# Data Transmission

# Data Mode (Clock Polarity and Clock Phase)

# Pros and cons 
- Advantages
  -
  -
- Disadvantages
  -
  -

# Demo: SPI LED Shift Register [3]
<img src="https://raw.githubusercontent.com/shannon112/Notes/main/SPI/SPI_LED_Shift_Register.png" width=400>
```
#include <SPI.h>

int slaveSelect = 2;

int delayTime = 50;

void setup() {
  pinMode(slaveSelect, OUTPUT);
  SPI.begin();
  SPI.setBitOrder(LSBFIRST);   
}

void loop() {
  for (int i; i < 256; i++)        //For loop to set data = 0 then increase it by one for every iteration of the loop, when the counter reaches the condition (256) it will be reset
  {
    digitalWrite(slaveSelect, LOW);            //Write our Slave select low to enable the SHift register to begin listening for data
    SPI.transfer(i);                     //Transfer the 8-bit value of data to shift register, remembering that the least significant bit goes first
    digitalWrite(slaveSelect, HIGH);           //Once the transfer is complete, set the latch back to high to stop the shift register listening for data
    delay(delayTime);                             //Delay
  }
}
```

# Reference
[1] https://en.wikipedia.org/wiki/Serial_Peripheral_Interface  
[2] https://www.analog.com/media/en/analog-dialogue/volume-52/number-3/introduction-to-spi-interface.pdf  
[3] https://www.youtube.com/watch?v=ZGaCXHvgcE4
