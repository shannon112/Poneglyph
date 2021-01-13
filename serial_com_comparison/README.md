# Serial v.s. Parallel [1][2]
<img src="https://raw.githubusercontent.com/shannon112/Notes/main/serial_com_comparison/Serial.png" height=300> <img src="https://raw.githubusercontent.com/shannon112/Notes/main/serial_com_comparison/Parallel.png" height=300>

# Asynchronous v.s. Synchronous [1][2]
<img src="https://raw.githubusercontent.com/shannon112/Notes/main/serial_com_comparison/Asynchronous.png" height=300> <img src="https://raw.githubusercontent.com/shannon112/Notes/main/serial_com_comparison/Synchronous.png" height=300>

# Serial Communication on Arduino Uno
<img src="https://raw.githubusercontent.com/shannon112/Notes/main/serial_com_comparison/arduino_uno.png" width=600>

# Serial Communication on Arduino Nano
<img src="https://raw.githubusercontent.com/shannon112/Notes/main/serial_com_comparison/arduino_nano.png" width=600>

# Serial Communication on Raspberry Pi (RPi) [3]
<img src="https://raw.githubusercontent.com/shannon112/Notes/main/serial_com_comparison/raspberry_pi.png" width=600>

# Serial Communication: UART v.s. I2C v.s. SPI

|          |   UART    |   I2C   |    SPI   |
| -------- | -------- | -------- | -------- |
| Full Name  | Universal Asynchronous Receiver Transmitter  | Inter-Integrated Circuit  | Serial Peripheral Interface |
| Connection | img     | img     | img     |
| SYN        | Asynchronous     | Synchronous     | Synchronous     |
| Wires      | 2 (RX, TX)     | 2 (SCL, SDA)   | 3 (SCLK, MISO, MOSI) + N (SS)   |
| Devices    | 1 to 1     | N Masters, N Slaves     | 1 Master, N Slaves    |
| Receiver and Transmitte | Simplex or Half-Duplex or Full-Duplex    | Half-Duplex     | Full-Duplex    |
| Speed      | img     | img     | img     |
| Distance   | img     | img     | img     |
| Power      | img     | img     | img     |
| Acknowledge Pin | img     | img     | img     |
| Error Detection | Parity Bit   | img     | img     |
| Flow Control    | img     | img     | img     |
| Data Start From | LSB First   | img     | img     |

# Reference
[1] https://www.youtube.com/watch?v=IyGwvGzrqp8
[2] https://www.youtube.com/watch?v=MebhACqcdno
[3] https://www.engineersgarage.com/raspberrypi/articles-raspberry-pi-i2c-bus-pins-smbus-smbus2-python/
