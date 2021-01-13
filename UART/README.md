# Intro:
The **Universal Asynchronous Receiver-Transmitter** (UART, pronounced U-ART) is an asynchronous serial communication in which the data format and transmission speeds are configurable. It sends data bits one by one, from the least significant to the most significant, framed by start and stop bits so that precise timing is handled by the communication channel. It was one of the earliest computer communication devices, used to attach teletypewriters for an operator console. It was also an early hardware system for the Internet. The electric signaling levels are handled by a driver circuit external to the UART. Two common signal levels are RS-232, a 12-volt system, and RS-485, a 5-volt system. Specialised UARTs are used for automobiles, smart cards and SIMs. Early teletypewriters used current loops. A UART is usually an individual (or part of an) integrated circuit (IC) used for serial communications over a computer or peripheral device serial port. One or more UART peripherals are commonly integrated in microcontroller chips.[1] Applications: serial (COM) port, RS-232, modems, etc.

# Character:
- **Serial communication bus**, with contrary to parallel.
- **Device-to-device communication**, with contrary to multi-master, multi-slave.
- **Can be Simplex, Half-Duplex, Full-Duplex communication**, one line TX->RX data send in one direction, 2 lines data send in each direction but only one at a time, 2 lines data send in both directions simultaneously.
- **Asynchronous communication**, no clock mechanism, the transmitter and receiver must at the same speed (baud rate) and frame structure.
- **Transmitted as frames**, a frame contains start bit, data bits, stop bit, parity bits(optional).

# Interface:
The main purpose of a transmitter and receiver line for each device is to transmit and receive serial data intended for serial communication.  
- **TX**：Transmitter 發送端. 
- **RX**：Receiver 接收端.
<img src="https://raw.githubusercontent.com/shannon112/Notes/main/UART/uart_interface.png" width=400>

# Speed (baud rate)
9600
19200
57600
115200

# Frame
- Start bit: from high to low
- Stop bit: stay or return to high. can have 1 or 2 stop bit
- Data bits: 5~9bits (usually 7 or 8 bits) and LSB first
- Parity bit: used for error detection. Even parity or odd
high voltage (marked), low voltage (space)
idle is held high

# Data Transmission
Setting: 
speed baud rate, 4800, 9600, 
parity, None, even, odd, mark, space
data bits, 5 6 7 8
stopbits, 1, 2
e.g. 9600 8N1 for speed=9600, databits=8, parity=none, stopbits=1

# Pros and Cons 
- Advantages  
  - low cost, and easy construct for low speed, low throughput application
- Disadvantages [1]  

# Demo: UART Communication with Computer
Original experiment from: https://github.com/binaryupdates/Arduino-Serial-Communication-using-UART [3]

<img src="https://raw.githubusercontent.com/shannon112/Notes/main/UART/demo_txrx.png" height=250> <img src="https://raw.githubusercontent.com/shannon112/Notes/main/UART/demo_wiring.png" height=250>

```c
int value = 0;

void setup() {
  Serial.begin(9600);
  pinMode(13, OUTPUT);
}

void loop() {
  if (Serial.available() > 0) {
    value = Serial.read();
    delay(5);

    if (value == '1') {
      digitalWrite(led, HIGH);
      Serial.println("LED is ON");
    }

    if (value == '0') {
      digitalWrite(led, LOW);
      Serial.println("LED is OFF");
    }
  }
}
```
- Arduino usb port to PC is a uart serial (COM) port, shown as /dev/cu.usbserial-1410
- Open the arduino serial monitor on PC and choose the correct frame format, then can send data through uart to arduino

# LSB first v.s. MSB first
- Most significant bit (MSB) first means that the most significant bit will arrive first.
- Least significant bit (LSB) first means that the least significant bit will arrive first.
<img src="https://raw.githubusercontent.com/shannon112/Notes/main/UART/MSB_LSB.png" width=400>

Data: B 1 0 1 0 0 1 1  (ASCII'S',0x53)  
LSB (first) order: 1 1 0 0 1 0 1  
MSB (first) order: 1 0 1 0 0 1 1  
<img src="https://raw.githubusercontent.com/shannon112/Notes/main/UART/LSB_first.png" width=400>

# Reference
[1] https://en.wikipedia.org/wiki/Universal_asynchronous_receiver-transmitter  
[2] https://www.youtube.com/watch?v=sTHckUyxwp8  
[3] https://www.youtube.com/watch?v=LubYc87S9tQ
[4] https://www.analog.com/en/analog-dialogue/articles/uart-a-hardware-communication-protocol.html
