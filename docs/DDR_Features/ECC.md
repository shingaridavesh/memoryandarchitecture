
|  Features  |      DDR1      |   DDR2    |      DDR3      |      DDR4      |   DDR5    |  
| :--------: |:-------------| :---------| :---------| :-------- | :-------------|
| ECC | Not Present| Not Present | Not Present | Not Present | * On-Die ECC <br> * ECC Error Check and Scrub (ECS)|

##ECC Types

Before we dive into ECC we need to understand different types of ECC present on DDR devices. I would broadly classify the ECC into 2 categories:

* Channel Level ECC
* Device Level ECC

###Channel Level ECC

![](../images/ecc/inlinesidebankecc.drawio)

##DDR5

On DDR5 there are 2 ECC features present at **Device** level:

* On-Die ECC
* ECC Transparency and Error Scrub

I will give a high level overview here but will look into more detail in their respective section.

* **On-Die ECC** improves the data integrity within the device (not channel/DIMM). DDR5 device internally computes and store 8 bit ECC check bits for every 128 data bits. This is computed and stored during Writes, and is used during Reads for correcting error (if any). This feature just corrects the error when Read operation is performed and it doesnt correct data in the data array.
* **ECC Transparency and Error Scrub** is a feature which enables device to internally read data bits, correct single bit errors (using check bits) and write back corrected data bits to the data array, and also provides transparency about the error it encountered through Mode Registers.

###On-Die ECC

DDR5 devices generates ECC internally and has logic to compute ECC code and store these codes inside the DDR5 device (this space is not included in specified memory density of the device). This code is generated and stored during Write operation and is computed again during Read to check data integrity. If there is single bit error, it is corrected by DDR5 device itself at device level.

DDR5 uses 8 bits ECC check bit which are generated for each 128 data bits. To understand this, lets revisit the Prefetch width for different DDR5 devices.

|  Devices  |      Prefetch (16)      |   
| :--------: |:-------------:| 
| x4 | 64 bit | 
| x8 | 128 bit | 
| x16 | 256 bit | 

![](../images/ecc/inlineecc.drawio)

* So from this you can understand that on x8 device, whenever we perform a operation, internally 128 bits are fetched and 8 bit ECC check bits will be computed and stored/checked depending on WR/RD operation. 
* But when we have x4 device, internally only 64 bits are fetched, so we need additional 64 bits. DDR5 devices fetches additional section of device array to provide additional 64 bits and computes the 8 bit ECC check bits. 
* In case of x16 devices, DDR5 internally fetches 256 bits, so 2 8-bit ECC check bits is generated and stored/checked. the 2 8-bit ECC check bits are checked separately and in parallel.

####On-Die ECC Operation

To understand ECC operation, you need to understand what Parity and Syndrome is. If you need a quick recap, I would hghly recommend this video "https://www.youtube.com/watch?v=z89uW4eCRx0".

#####Write Operation

* During Write operation, ECC check bits will be computed and stored in a separate region of the memory.
* For a x8 device, it is pretty straightforward. Given BL16, we will receive 16 x 8 = 128 bits of data. Given On-Die ECC on DDR5 operates on 128 bits, this is perfect. ECC Check Bit Generator will generate the 8-bit Check Bits and store it along with the data.
* For a x4 device it becomes little more interesting. On x4 device, we will be receving 64 bits of data, so we will need additional 64 bits of data to compute the 8-bit Check Bits. This is done by Read-Modify-Write operation. Device will first perform internal read (to get additional 64 bits of data) and correct if there are any errors. Then this corrected data will be merged with incoming 64 bits and 8-bit Check Bits will be recomputed and stored along with data.
* For x16 device we will have 256 bits of data. Given device operates on 128 bits, there will be 2 Check Bit operations happening and they will happen separately and in parallel to generate 2 8-bit Check Bits.

![](../images/ecc/ondieeccwrite.drawio)

#####Read Operation

* During Read operation, On-Die ECC will correct any 1-bit error before sending data back to controller. It should be noted that corrected data wont be updated in memeory data array.
* During Read, data being read and check bits are fed into Syndrome generator. Depending on the ECC logic implementation by DRAM vendor, Syndrome generated will be used to correct any single bit error, if any. Sun
* Just like how we saw for x4 devices above, we will fetch additional 64 bits to complete the 128 bits and then operation will be performed.
* For x16 devices, there will be 2 operations happening simultaneously in parallel. One set of 128 bits will be mapped to DQ[0:7] and other 128 bits will be mapped to DQ[8:15].

![](../images/ecc/ondieeccread.drawio)

###ECC Transparency and Error Scrub

####ECS Mode
There are 2 ways to perform/initiate ECS (Error Check and Scrub) operation.

1. Manual ECS
2. Automatic ECS

|  Function  |      Mode Register      |   Operand    |      Data      | 
| :--------: |:-------------:| :---------:| :---------| 
| ECS Mode | MR14 | OP[7] | **0B:** Automatic ECS Mode (Default) <br> **1B:** Manual ECS Mode |


####ECS Procedure

Manual ECS Operation

* 
*

    |  Function  |      MPC Mode      |OP7|OP6|OP5|OP4|OP3|OP2|OP1|OP0| 
    | :--------: |:-------------:| :---------:| :---------:|  :---------:| :---------:|  :---------:| :---------:|  :---------:| :---------:| 
    | Manual ECS Operation | ECS |  0|0|0|0|1|1|0|0| 


####ECS Statistics

We understand that for ECS operation, DRAM internally operates on 128 data bits, which means in a single ECS operation it works on 2^7 bits. No if we know the density of the device, we can determine how many operations it will take to perform ECS for entire device.

|  Size(Gb)  |     Size(b)      | Number of Operations = Size/128b| 
| :--------: |:-------------:| :---------:| 
| 8Gb | 8 x 1024 x 1024 x 1024 = 2^3 x 2^10 x 2^10 x 2^10  = 2^33  |  2^33 / 2^7 = **2^26**|
| 16Gb | 16 x 1024 x 1024 x 1024 = 2^4 x 2^10 x 2^10 x 2^10  = 2^34  |  2^34 / 2^7 = **2^27**|
| 24Gb | 24 x 1024 x 1024 x 1024 = 2^3 x 3 x 2^10 x 2^10 x 2^10  = 2^33 x 3  |  2^33 x 3 / 2^7 = **2^27 x 1.5**|
| 32Gb | 32 x 1024 x 1024 x 1024 = 2^5 x 2^10 x 2^10 x 2^10  = 2^35  |  2^35 / 2^7 = **2^28**|
| 64Gb | 64 x 1024 x 1024 x 1024 = 2^6 x 2^10 x 2^10 x 2^10  = 2^36  |  2^36 / 2^7 = **2^29**|

Now to compute how long will ECS take, we need to consider JEDEC recommendation. JEDEC recommends to perform full error check and scrub within 24 hours. 
24 hours = 24 * 60 * 60 seconds = 86400 seconds

|  Size(Gb)  |     Number of Operations      | Average Periodic ECS Interval (tECSint)| 
| :--------: |:-------------:| :---------:| 
| 8Gb | 2^26  |  86400 / 2^26 = **1.287ms**|
| 16Gb | 2^27  |  86400 / 2^27 = **0.644ms**|
| 24Gb | 2^27 x 1.5 |  86400 / (2^27 x 1.5) = **0.429ms**|
| 32Gb | 2^28  |  86400 / 2^28 = **0.322ms**|
| 64Gb | 2^29  |  86400 / 2^29 = **0.161ms**|

####ECS Tracking
Along with ECS feature, DDR5 also provides **Tracking** feature, thats why JEDEC calls it "ECC **Transparency** and Error Scrub", because it is transparent to the user about how many error DRAM device is encountering and correcting.

There are 2 options which can be selected by MR14 OP[5], to be tracked by error counter.

* Track either number of rows with errors **OR**
* Track code words with errors


|  Function  |      Mode Register      |   Operand    |      Data      | 
| :--------: |:-------------:| :---------:| :---------| 
| Code Word/Row Count | MR14 | OP[5] | **0B:** ECS counts Rows with errors <br> **1B:** ECS counts Code words with errors |

Once you have chosen what you want to track, tracked errors are present at MR20.

|  Function  |      Mode Register      |   Operand    |      Data      | 
| :--------: |:-------------:| :---------:| :---------| 
| Error Count EC[7:0] | MR20 | OP[7:0] | Contains the error count range data |

**NOTE**: Incase it is 3DS-DDR5 stack device, then the errors correspond to CID chosen through MR14 OP[3:0] field.

#####ECC Row Count Error

#####ECC Code Word Error

* In this mode, Error Count increments each time a code word with check bit errors is detected. 
* After all code words - on all rows, banks, bank groups have ECS performed, result in loaded into MR20, subject to error threshold reporting.
* Once the Error Count has been transferred to MR20, it gets reset.


|  Function  |     Mode Register     |OP7|OP6|OP5|OP4|OP3|OP2|OP1|OP0| 
| :--------: |:-------------:| :---------:| :---------:|  :---------:| :---------:|  :---------:| :---------:|  :---------:| :---------:| 
| Max Row Error Address R[7:0] | MR16 |  R7|R6|R5|R4|R3|R2|R1|R0| 
| Max Row Error Address R[15:8] | MR17 |  R15|R14|R13|R12|R11|R10|R9|R8|
| Max Row Error Address R[17:16],BA[1:0],BG[2:0] | MR18 |  RFU|BG2|BG1|BG0|BA1|BA0|R17|R16| 