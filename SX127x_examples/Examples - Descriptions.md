SX12XX Library Example Programs

#Basics

#### 1\_LED\_Blink
This program blinks an LED connected the pin number defined by LED1.The pin 13 LED, fitted to some Arduinos is blinked as well. The blinks should be close to one per second. Messages are sent to the Serial Monitor also.

#### 2\_Register\_Test
This program checks that a SX127X LoRa device can be accessed. There should be two short LED flashes at start-up. If the SX127X is detected there will be two more LED flashes and the contents of the registers from 0x00 to 0x7F are printed, this is a copy of a typical printout below. Note that the read back changed frequency may be different to the programmed frequency (434100000hz), there is a rounding error, the frequency can only be programmed in units of approx 61hz. 

If the SX127X is not detected LED1 will flash rapidly.

    Frequency at reset 434000000
    Registers at reset
    Reg0  1  2  3  4  5  6  7  8  9  A  B  C  D  E  F 
    0x00  00 09 1A 0B 00 52 6C 80 00 4F 09 2B 20 08 02 0A
	0x10  FF 70 15 0B 28 0C 12 47 32 3E 00 00 00 00 00 40
	0x20  00 00 00 00 05 00 03 93 55 55 55 55 55 55 55 55
	0x30  90 40 40 00 00 0F 00 00 00 F5 20 82 03 02 80 40
	0x40  00 00 12 24 2D 00 03 00 04 23 00 09 05 84 32 2B
	0x50  14 00 00 12 00 00 00 0F E0 00 0C 03 08 00 5C 78
	0x60  00 19 0C 4B CC 0F 41 20 04 47 AF 3F D4 00 53 0B
	0x70  D0 01 10 00 00 00 00 00 00 00 00 00 00 00 00 00

	Changed Frequency 434099968
	Reg0  1  2  3  4  5  6  7  8  9  A  B  C  D  E  F
	0x00  00 09 1A 0B 00 52 6C 86 66 4F 09 2B 20 08 02 0A
	0x10  FF 70 15 0B 28 0C 12 47 32 3E 00 00 00 00 00 40
	0x20  00 00 00 00 05 00 03 93 55 55 55 55 55 55 55 55
	0x30  90 40 40 00 00 0F 00 00 00 F5 20 82 03 02 80 40
	0x40  00 00 12 24 2D 00 03 00 04 23 00 09 05 84 32 2B
	0x50  14 00 00 12 00 00 00 0F E0 00 0C 03 08 00 5C 78
	0x60  00 19 0C 4B CC 0F 41 20 04 47 AF 3F D4 00 53 0B
	0x70  D0 01 10 00 00 00 00 00 00 00 00 00 00 00 00 00



#### 3\_LoRa\_Transmitter
This is a simple LoRa test transmitter. A packet containing ASCII text is sent according to the frequency and LoRa settings specified in the 'Settings.h' file. The pins to access  the SX127X need to be defined in the 'Settings.h' file also.

The details of the packet sent and any errors are shown on the Serial Monitor, together with the transmit power used, the packet length and the CRC of the packet. The matching receive program, '4_LoRa_Receive' can be used to check the packets are being sent correctly, the frequency and LoRa settings (in Settings.h) must be the same for the Transmit and Receive program. 

Sample Serial Monitor output;

2dBm Packet> {packet contents*}  BytesSent,19  CRC,3882  TransmitTime,54mS  PacketsSent,1

#### 4\_LoRa\_Receiver

The program listens for incoming packets using the LoRa settings in the 'Settings.h' file. The pins to access the SX127X need to be defined in the 'Settings.h' file also. There is a printout of the valid packets received, the packet is assumed to be in ASCII printable text, if its not ASCII text characters from 0x20 to 0x7F, expect weird things to happen on the Serial Monitor. The LED will flash for each packet received and the buzzer will sound, if fitted.

Sample serial monitor output;

	1109s  {packet contents}  CRC,3882,RSSI,-69dBm,SNR,10dB,Length,19,Packets,1026,Errors,0,IRQreg,50

If there is a packet error it might look like this, which is showing a CRC error,

	1189s PacketError,RSSI,-111dBm,SNR,-12dB,Length,0,Packets,1126,Errors,1,IRQreg,70,IRQ_HEADER_VALID,IRQ_CRC_ERROR,IRQ_RX_DONE


----------

----------


##Diagnostics and Test

#### 10\_LoRa\_Link\_Test\_TX

This is a program that can save you a great deal of time if your testing the effectiveness of a LoRa links or attached antennas. Simulations of antenna performance are no substitute for real world tests and this simple program allows both long distance link performance to be evaluated and antenna performance to be compared.

The program sends short test packets that reduce in power by 1dBm at a time. The start power is defined by start_power and the end power is defined by end_power (see Settings.h file). Once the end_power point is reached, the program pauses a short while and starts the transmit sequence again at start_power. The packet sent contains the power used to send the packet. By listening for the packets with the basic LoRa receive program 4_LoRa_Receiver) you can see the reception results, which should look something like this;

	11s  1*T+05,CRC,80B8,RSSI,-03dBm,SNR,9dB,Length,6, Packets,9,Errors,0,IRQreg,50
	12s  1*T+04,CRC,9099,RSSI,-74dBm,SNR,9dB,Length,6, Packets,10,Errors,0,IRQreg,50
	14s  1*T+03,CRC,E07E,RSSI,-75dBm,SNR,9dB,Length,6, Packets,11,Errors,0,IRQreg,50

Above shows 3 packets received, the first at +05dBm (+05 in printout), the second at 4dBm (+04 in printout) and the third at 3dBm (+03) in printout.

If it is arranged so that reception of packets fails halfway through the sequence by attenuating either the transmitter (with an SMA attenuator for instance) or the receiver (by placing it in a tin perhaps) then if you swap antennas you can see the dBm difference in reception, which will be the dBm difference (gain) of the antenna.

To start the sequence a packet is sent with the number 999, when received it looks like this;

T*1999

This received packet could be used for the RX program to be able to print totals etc.

LoRa settings to use for the link test are specified in the 'Settings.h' file.

#### 11\_LoRa\_HEX\_Print\_RX

This is a useful packet logger program. It listens for incoming packets using the LoRa settings in the 'Settings.h' file. The pins to access the SX127X need to be defined in the 'Settings.h' file also.

There is a printout of the valid packets received in HEX format. Thus the program can be used to receive and record non-ASCII packets. The LED will flash for each packet received and the buzzer will sound, if fitted. The measured frequency difference between the frequency used by the transmitter and the frequency used by the receiver is shown. If this frequency difference gets to 25% of the set LoRa bandwidth, packet reception will fail. The displayed error can be reduced by using the 'offset' setting in the 'Settings.h' file. 


#### 13\_Frequency\_and\_Power\_Check\_TX

This is a program that transmits a long LoRa packets lasting about 5 seconds that can be used to measure the frequency and power of the transmission using external equipment. The bandwidth of the transmission is only 10khz, so a frequency counter should give reasonable average result.

The LoRa settings to use, including transmit powwer, are specified in the 'Settings.h' file.

#### 16\_LoRa\_RX\_Frequency\_Error\_Check

This program can be used to check the frequency error between a pair of LoRa devices, a transmitter and receiver. This receiver measures the frequecy error between the receivers  centre frequency and the centre frequency of the transmitted packet. The frequency difference is shown
for each packet and an average over 10 received packets reported. Any transmitter program can be used to give this program something to listen to, including example program '3_LoRa_Transmit'.  

#### 20\_LoRa\_Link\_Test\_Receiver

The program listens for incoming packets using the LoRa settings in the 'Settings.h' file. The pins to access the SX127X need to be defined in the 'Settings.h' file also.

The program is a matching receiver program for the '10_LoRa_Link_Test_TX'. The packets received are displayed on the serial monitor and analysed to extract the packet data which indicates the power used to send the   packet. A count is kept of the numbers of each power setting received. When the transmitter sends the test mode packet at the beginning of the sequence (displayed as 999) the running totals of the powers received is printed. Thus you can quickly see at what transmit power levels the reception fails. 

## Low Memory

#### 8\_LoRa\_LowMemory\_TX

The program transmits a packet without using a processor buffer, the LoRa device internal buffer is filled direct with variables. The program is a simulation of the type of packet that might be sent from a GPS tracker. Note that in this example a buffer of text is part of the transmitted packet, this does need a processor buffer which is used to fill the LoRa device internal buffer, if you don't need to transmit text then the uint8_t trackerID[] = "LoRaTracker1"; definition can be omitted.

The matching receiving program '9_LoRa_LowMemory_RX' can be used to receive and display the packet, though the program  '15_LoRa_RX_Structure' should receive it as well, since the packet contents are the same.

The contents of the packet received, and printed to serial monitor, should be;
  
"LoRaTracker1" (buffer)      - trackerID  
1+             (uint32\_t)   - packet count     
51.23456       (float)       - latitude   
-3.12345       (float)       - longitude  
199            (uint16\_t)   - altitude  
8              (uint8\_t)    - number of satellites   
3999           (uint16\_t)   - battery voltage 
-9             (int8_t)      - temperature

#### 9\_LoRa\_LowMemory\_RX

The program receives a packet without using a processor buffer, the LoRa device internal buffer is read direct and copied to variables. The program is a simulation of the type of packet that might be received from a GPS tracker. Note that in this example a buffer of text is part of the   received packet, this does need a processor buffer which is filled with data from the LoRa device internal buffer, if you don't need to send and receive text then the uint8_t receivebuffer[32]; definition can be   omitted. 

The contents of the packet received, and printed to serial monitor, should be;
  
"LoRaTracker1" (buffer)      - trackerID  
1+             (uint32\_t)   - packet count     
51.23456       (float)       - latitude   
-3.12345       (float)       - longitude  
199            (uint16\_t)   - altitude  
8              (uint8\_t)    - number of satellites   
3999           (uint16\_t)   - battery voltage 
-9             (int8_t)      - temperature

#### 14\_LoRa\_Structure\_TX

This program demonstrates the transmitting of a structure as a LoRa packet. The contents of the structure are the same as in the **8\_LoRa\_LowMemory\_TX** program. The packet sent is typical of what might be sent from a GPS tracker. The structure type is defined as trackerPacket and an instance called location1 is created. The structure which includes a character array (text) is filled with values and transmitted.

The matching receiving program '15_LoRa_RX_Structure' can be used to  receive and display the packet, though the program '9_LoRa_LowMemory_RX' should receive it as well, since the contents are the same.

Note that the structure definition and variable order (including the buffer size) used in the transmitter need to match those used in the receiver. 

The contents of the packet transmitted should be;
  
"LoRaTracker1" (buffer)      - trackerID  
1+             (uint32\_t)   - packet count     
51.23456       (float)       - latitude   
-3.12345       (float)       - longitude  
199            (uint16\_t)   - altitude  
8              (uint8\_t)    - number of satellites   
3999           (uint16\_t)   - battery voltage 
-9             (int8_t)      - temperature

#### 15\_LoRa\_Structure\_RX


This program demonstrates the receiving of a structure as a LoRa packet. The packet sent is typical of what might be sent from a GPS tracker.

The structure type is defined as trackerPacket and an instance called location1 is created. The structure includes a character array (text).

The matching receiving program is **15\_LoRa\_RX\_Structure** can be used to receive and display the packet, though the program **9\_LoRa\_LowMemory\_RX** should receive it as well, since the packet contents are the same.

Not that the structure definition and variable order (including the buffer size) used in the transmitter need to match those used in the receiver. Good luck.

The contents of the packet received, and printed to serial monitor, should be;
  
"LoRaTracker1" (buffer)      - trackerID  
1+             (uint32\_t)   - packet count     
51.23456       (float)       - latitude   
-3.12345       (float)       - longitude  
199            (uint16\_t)   - altitude  
8              (uint8\_t)    - number of satellites   
3999           (uint16\_t)   - battery voltage 
-9             (int8_t)      - temperature

## Remote Control

#### 21\_On\_Off\_Transmitter

This program is a remote control transmitter. When one of three switches are made (shorted to ground) a packet is transmitted with single byte indicating the state of Switch0 as bit 0, Switch1 as bit 1 and Switch2 as bit 2. To prevent false triggering at the receiver the packet contains a
32 bit number called the TXIdentity which in this example is set to 1234554321. The receiver will only act on, change the state of the outputs, if the identity set in the receiver matches that of the  transmitter. The chance of a false trigger is fairly remote.

Between switch presses the LoRa device and Atmel microcontroller are put to sleep. A switch press wakes up the processor from sleep, the switches are read and a packet sent. The pin definitions, LoRa frequency and LoRa modem settings are in the Settings.h file.

#### 22\_On\_Off\_Receiver

This program is a remote control receiver. When a packet is received an 8 bit byte (SwitchByte) is read and the three outputs (defined in Settings.h) are toggled according to the bits set in this byte. If the Switch1 byte has bit 0 cleared, then OUTPUT0 is toggled. If the Switch1 byte has bit 1 cleared, then OUTPUT1 is toggled. If the Switch1 byte has bit 2 cleared, then OUTPUT2 is toggled. 
  
To prevent false triggering at the receiver the packet contains also contains a 32 bit number called the TXIdentity which in this example is set to 1234554321. The receiver will only act on, change the state of the outputs, if the identity set in the receiver matches that of the transmitter. The chance of a false trigger is fairly remote.

The pin definitions, LoRa frequency and LoRa modem settings are in the Settings.h file.

## Sensor


#### 17\_Sensor\_Transmitter

The program transmits a LoRa packet without using a processor buffer, the LoRa devices internal buffer is filled directly with variables. 

The sensor used is a BME280. The pressure, humidity, and temperature are read and transmitted. There is also a 16bit value of battery mV (simulated) and and a 8 bit status value at the packet end. Although the LoRa packet transmitted and received has its own internal CRC error checking, you could still receive packets of the same length from another source. If this valid packet were to be used to recover the sensor values, you could be reading rubbish. To reduce the risk of this, when the packet is transmitted the CRC value of the actual sensor data is calculated and sent out with the packet. This CRC value is read by the receiver and used to check that the received CRC matches the supposed sensor data in the packet. 

As an additional check there is some addressing information at the beginning of the packet which is also checked for validity. Thus we can be relatively confident when reading the received packet that its genuine and from this transmitter. The packet is built and sent in the sendSensorPacket() function, there is a 'highlighted section' where the actual sensor data is added to the packet.

Between readings the LoRa device, BME280 sensor, and Atmel micro controller are put to sleep in units of 8 seconds using the Atmel processor internal watchdog.

The pin definitions, LoRa frequency and LoRa modem settings are in the Settings.h file.

The Atmel watchdog time is a viable option for a very low current sensor node. A bare bones ATmega328P with regulator and LoRa device has a sleep current of 6.5uA, add the LoRa devices and BME280 sensor module and the sleep current only rises to 6.7uA.

However there is the time the processor takes to wake up from sleep and start running the program code. When monitoring the supply rail with a resistor, I see power pulses of circa 2.4mS when the watchdog timer wakes the processor up. So assuming a powered time of 2.4mS every 8 seconds (1/3333th of the time) and a processor run current of maybe 6mA, thats an average of 1.8uA.

So adding the predicted average wake up current to the measured sleep current give an average node current in sleep of maybe 8.5uA, not bad for a freebie sleep timer. If such a node were put into a loop that kept it permanently in sleep, then a pack of AA alkaline batteries could last 37 years, if it were possible for the battery shelf life to be that long.  


#### 18\_Sensor\_Receiver

The program receives a LoRa packet without using a processor buffer, the LoRa devices internal buffer is read direct for the received sensor data. 
  
The sensor used in the matching '17_Sensor_Transmiter' program is a BME280 and the pressure, humidity, and temperature are being and received. There is also a 16bit value of battery mV and and a 8 bit status value at the end of the packet.

When the program starts, the LoRa device is setup to set the DIO0 pin high when a packet is received, the Atmel processor is then put to sleep and will wake up when a packet is received. When a packet is received, its printed and assuming the packet is validated, the sensor results are printed to the serial monitor and screen. Between readings the sensor transmitter is put to sleep in units of 8 seconds using the Atmel
processor internal watchdog.

For the sensor data to be accepted as valid the folowing need to match;

The 16bit CRC on the received sensor data must match the CRC value transmitted with the packet. 
The packet must start with a byte that matches the packet type sent, 'Sensor1'
The RXdestination byte in the packet must match this node ID of this receiver node, defined by 'This_Node'

In total thats 16 + 8 + 8  = 32bits of checking, so a 1:4294967296 chance (approx) that an invalid packet is acted on and erroneous values displayed.

The pin definitions, LoRa frequency and LoRa modem settings are in the Settings.h file.

Easy Mikrobus Pro Mini Sleep current, plain TX only = 89uA
Bares bones Arduino sleep current, plain TX only = 6.5uA


## Sleep

#### 5\_LoRa\_TX\_Sleep\_Timed\_Wakeup\_Atmel

This program tests the sleep mode and register retention of the SX127X in sleep mode, it assumes an Atmel ATMega328P processor is in use. The LoRa settings to use are specified in the 'Settings.h' file.

A packet is sent, containing the text 'Before Device Sleep' and the LoRa device and Atmel processor are put to sleep. The processor watchdog timer should wakeup the processor in 15 seconds (approx) and register values should be retained.  The device then attempts to transmit another packet 'After Device Sleep' without re-loading all the LoRa settings. The receiver should see 'After Device Sleep' for the first packet and 'After Device Sleep' for the second.

Tested on an ATmega328P bare bones board, the current in sleep mode was 6.5uA.


#### 6\_LoRa\_RX\_and\_Sleep\_Atmel

The program listens for incoming packets using the LoRa settings in the 'Settings.h' file. The pins to access the SX127X need to be defined in the 'Settings.h' file also.

When the program starts the LoRa device is setup to recieve packets with pin DIO0 set to go high when a packet arrives. The receiver remains powered (it cannot receive otherwise) and the processor (Atmel ATMega328P or 1284P) is put to sleep. When pin DIO0 does go high, indicating a packet is received, the processor wakes up and prints the packet. It then goes back to sleep.

There is a printout of the valid packets received, these are assumed to be in ASCII printable text. The LED will flash for each packet received and the buzzer will sound,if fitted.

#### 7\_LoRa\_TX\_Sleep\_Switch\_Wakeup\_Atmel

This program tests the sleep mode and register retention of the SX127X in sleep mode, it assumes an Atmel ATMega328P processor is in use. The LoRa settings to use are specified in the 'Settings.h' file.

A packet is sent, containing the text 'Before Device Sleep' and the SX127x and Atmel processor are put to sleep. The processor should remain asleep until the pin defined by SWITCH1 in the Settings.h file is connected to ground, the LoRa device register values should be retained.  The LoRa device then attempts to transmit another packet 'After Device Sleep' without re-loading all the LoRa settings. The receiver should see 'After Device Sleep' for the first packet and 'After Device Sleep' for the second.

Tested on an bare bones ATmega328P board, the curent in sleep mode was 6.1uA.





## Tracker

#### 23\_Simple\_GPS\_Tracker\_Transmitter


This program is an example of a basic GPS tracker. The program reads the GPS, waits for an updated fix and transmits location and altitude, number of satellites in view, the HDOP value, the fix time of the GPS and the battery voltage. This transmitter can be also be used to investigate GPS performance. At startup there should be a couple of seconds of  recognisable text from the GPS printed to the serial monitor. If you see garbage or funny characters its likley the GPS baud rate is wrong. If the transmitter is turned on from cold, the receiver will pick up the cold fix time, which is an indication of GPS performance. The GPS will be powered on for around 4 seconds before the timing of the fix starts. Outside with a good view of the sky most GPSs should produce a fix in around 45 seconds. The number of satellites and HDOP are good indications to how well a GPS is working. 

The program writes direct to the LoRa devices internal buffer, no memory buffer is used.
  
The LoRa settings are configured in the Settings.h file. file. `

#### 24\_Simple\_GPS\_Tracker\_Receiver

This program is an basic receiver for the '23_Simple_GPS_Tracker_TX' program. 

The program reads the received packet from the tracker transmitter and displays the results on the serial monitor. The LoRa and frequency settings settings provided in the Settings.h file must match those used by the transmitter. 

The program receives direct from the LoRa devices internal buffer.

#### 26\_GPS\_Echo

This is a simple program to test a GPS. It reads characters from the GPS using software serial and sends them (echoes) to the IDE serial monitor. If your ever having problems with a GPS (or just think you are) use this program first.

If you get no data displayed on the serial monitor, the most likely cause is that you have the receive data pin into the Arduino (RX) pin connected incorrectly.

If the data displayed on the serial terminal appears to be random text with odd symbols its very likely you have the GPS serial baud rate set incorrectly.

Note that not all pins on an ATmega will work with software serial, see here;

https://www.arduino.cc/en/Reference/softwareSerial

For a more detailed tutorial on GPS problems see here;

https://github.com/LoRaTracker/GPSTutorial

Serial monitor baud rate is set at 115200.


#### 28\_GPS\_Checker

This program is a GPS checker. At startup the program starts checking the data coming from the GPS for a valid fix. It checks for 5 seconds and if there is no fix, prints a message on the serial monitor. During this time the data coming from the GPS is copied to the serial monitor also. 
  
When the program detects that the GPS has a fix, it prints the Latitude, Longitude, Altitude, Number of satellites in use and the HDOP value to the serial monitor.

#### 29\_GPS\_Checker\_With\_Display

This program is a GPS checker with a display output. It uses an SSD1306 or SH1106 128x64 I2C OLED display. At startup the program starts checking the data coming from the GPS for a valid fix. It reads the GPS for 5 seconds and if there is no fix, prints a message on the serial monitor and updates the seconds without a fix on the display. During this time the data coming from the GPS is copied to the serial monitor also.

When the program detects that the GPS has a fix, it prints the Latitude, Longitude, Altitude, Number of satellites in use, the HDOP value, time and date to the serial monitor. If the I2C OLED display is attached that is updated as well. Display is assumed to be on I2C address 0x3C.
