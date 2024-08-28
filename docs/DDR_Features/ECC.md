
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