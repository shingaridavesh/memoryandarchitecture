
|  Features  |      DDR1      |   DDR2    |      DDR3      |      DDR4      |   DDR5    |  
| :--------: |:-------------| :---------| :---------| :-------- | :-------------|
| ECC | Not Present| Not Present | Not Present | Not Present | * On-Die ECC <br> * ECC Error Check and Scrub (ECS)|


![](../images/ecc/inlinesidebankecc.drawio)

##DDR5

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


###ECS Statistics

We understand that for ECS operation, DRAM internally operates on 128 data bits, which means in a single ECS operation it works on 2^7 bits. No if we know the density of the device, we can determine how many operations it will take to perform ECS for entire device.

|  Size(Gb)  |     Size(b)      | Number of Operations = Size/128b| 
| :--------: |:-------------:| :---------:| 
| 8Gb | 8 x 1024 x 1024 x 1024 = 2^3 x 2^10 x 2^10 x 2^10  = 2^33  |  2^33 / 2^7 = 2^26|
| 16Gb | 16 x 1024 x 1024 x 1024 = 2^4 x 2^10 x 2^10 x 2^10  = 2^34  |  2^34 / 2^7 = 2^27|
| 24Gb | 24 x 1024 x 1024 x 1024 = 2^3 x 3 x 2^10 x 2^10 x 2^10  = 2^33 x 3  |  2^33 x 3 / 2^7 = 2^27 x 1.5|
| 32Gb | 32 x 1024 x 1024 x 1024 = 2^5 x 2^10 x 2^10 x 2^10  = 2^35  |  2^35 / 2^7 = 2^28|
| 64Gb | 64 x 1024 x 1024 x 1024 = 2^6 x 2^10 x 2^10 x 2^10  = 2^36  |  2^36 / 2^7 = 2^29|

