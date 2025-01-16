
With ultra-high-speed memory like DDR4 DRAM, even a small amount of noise between the DRAM controller and the DRAM can change the logical value of the signal. Therefore, DDR4 adds a function to detect errors that occur when transmitting data signals, command signals, address signals, etc.

　For example, when writing a data signal, a "Write Data CRC" function is provided that sends a Cyclic Redundancy Check (CRC) code along with the write data. The DRAM controller generates a CRC code from the write data, and sends the write data and CRC code together to the DRAM. The DRAM generates a CRC code from the write data and compares it with the value of the CRC code sent from the DRAM controller. If there is a difference between the two, the DRAM sends a flag indicating an error has occurred to the DRAM controller.

　The length of the CRC code is 8 bits, and an 8-bit CRC code is generated from the 72 bits of write data including DBI. 272 ​​2-input XOR logic gates are used to generate the CRC code. The number of logic stages is 6, which is quite deep. Naturally, using "Write Data CRC" reduces the write throughput.

　The simplest, fastest, and most common error detection code is the 1-bit parity code, but it has the weakness of being unable to detect errors of 2 or more bits. CRC codes can detect 2-bit errors and odd-numbered bit errors of 2 or more bits, so although they are more complicated than parity codes, they have a wider range of error detection.

![](../images/crc/ddr4_ddr5_crc.drawio)


|  CRC Feature  |      Write CRC      |   Read CRC |
| :--------: |:-------------:| :-------------:| 
| DDR4 | Present | Not Present |
| DDR5 | Present | Not Present |
| DDR4 RDIMM/LRIMM | 256 bit | |
| DDR5 RDIMM/LRIMM | 256 bit | |

## CRC vs ECC

When learning about CRC, I always had a question that why we need both i.e. CRC and ECC. So let us try to understand how they both are different and target different things. 

Let us consider DDR4 as an example. Consider a DIMM which has 8 x8 devices which are for Data and 1 x8 device which is for ECC. If DIMM was non-ECC, there would be only be 8 x8 devices. As a refresher, DDR4 has a BL of 8, so when we perform a Write operation, we write data for 8 DQS cycles. So a x8 device will actually transfer 64 bits of data (8 DQ lines and each DQ performing BL8). So if we have 8 x8 devices on DIMM, total data transfer will be 512b (64 DQ lines and each DQ performing BL8). For this data transfer, Controller will also transmit ECC across 1 x8 device i.e. it will transmit 64b (8 ECC lines and each ECC performing BL8). So we have 64b-72b i.e. for 64b of data 8b of additional ECC data be send, so actuall transaction is 72b instead of 64b.