
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

Number of Operations needed for
8Gb = 8 x 1024 x 1024 x 1024 x 8 = 2^3 x 2^10 x 2^10 x 2^10  = 2^33 bits
It works on 128 bits i.e. 2^7. So total operations needed = 2^33 / 2^7 = 2^26