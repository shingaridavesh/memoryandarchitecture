
With ultra-high-speed memory like DDR4 DRAM, even a small amount of noise between the DRAM controller and the DRAM can change the logical value of the signal. Therefore, DDR4 adds a function to detect errors that occur when transmitting data signals, command signals, address signals, etc.

　For example, when writing a data signal, a "Write Data CRC" function is provided that sends a Cyclic Redundancy Check (CRC) code along with the write data. The DRAM controller generates a CRC code from the write data, and sends the write data and CRC code together to the DRAM. The DRAM generates a CRC code from the write data and compares it with the value of the CRC code sent from the DRAM controller. If there is a difference between the two, the DRAM sends a flag indicating an error has occurred to the DRAM controller.

　The length of the CRC code is 8 bits, and an 8-bit CRC code is generated from the 72 bits of write data including DBI. 272 ​​2-input XOR logic gates are used to generate the CRC code. The number of logic stages is 6, which is quite deep. Naturally, using "Write Data CRC" reduces the write throughput.

　The simplest, fastest, and most common error detection code is the 1-bit parity code, but it has the weakness of being unable to detect errors of 2 or more bits. CRC codes can detect 2-bit errors and odd-numbered bit errors of 2 or more bits, so although they are more complicated than parity codes, they have a wider range of error detection.


|  CRC Feature  |      Write CRC      |   Read CRC |
| :--------: |:-------------:| :-------------:| 
| DDR4 | Present | Not Present |
| DDR5 | Present | Not Present |
| DDR4 RDIMM/LRIMM | 256 bit | |
| DDR5 RDIMM/LRIMM | 256 bit | |