##CSTM

###Entry/Exit

* CSTM is entered and exited using MPC command.

    |  Function  |      MPC Mode      |OP7|OP6|OP5|OP4|OP3|OP2|OP1|OP0| 
    | :------: |:--------:| :-:| :-:|  :-:| :-:|  :-:| :-:|  :-:| :-:| 
    | Exit CS Training Mode | CSTMX |  0|0|0|0|0|0|0|0| 
    | Enter CS Training Mode | CSTMN |  0|0|0|0|0|0|0|1| 

* CSTM can only be entered when all Bank are Idle.

###Operation

To understand the operation, we need to understand some basic concept.

![](../images/commandbustraining/cstmsignals.drawio)

![](../images/commandbustraining/cstmlogic.drawio)

![](../images/commandbustraining/cstmtimingdiagram.drawio)

###Output

|Output|x16|x8|x4|
|:-:|:-:|:-:|:-:|
| DQ0 | CSTM Output | CSTM Output | CSTM Output |
| DQ1 | CSTM Output | CSTM Output | CSTM Output |
| DQ2 | CSTM Output | CSTM Output | CSTM Output |
| DQ3 | CSTM Output | CSTM Output | CSTM Output |
| DQ4 | CSTM Output | CSTM Output | NA |
| DQ5 | CSTM Output | CSTM Output | NA |
| DQ6 | CSTM Output | CSTM Output | NA |
| DQ7 | CSTM Output | CSTM Output | NA |
| DML | NA | NA | NA |
| TDQS_c | NA | NA | NA |
| DQSL_t | NA | NA | NA |
| DQSL_c | NA | NA | NA |
| DQ8 | CSTM Output | NA | NA |
| DQ9 | CSTM Output | NA | NA |
| DQ10 | CSTM Output | NA | NA |
| DQ11 | CSTM Output | NA | NA |
| DQ12 | CSTM Output | NA | NA |
| DQ13 | CSTM Output | NA | NA |
| DQ14 | CSTM Output | NA | NA |
| DQ15 | CSTM Output | NA | NA |
| DMU | NA | NA | NA |
| DQSU_t | NA | NA | NA |
| DQSU_c | NA | NA | NA |

##DCS Training Mode (DCSTM)

Just like we have CSTM, we have DCSTM mode on RDIMMs/LRDIMMs. On RDIMMs, Clock/Command-Address/Control signals go to RCD and not to DRAM device directly, so RCD has DCSTM mode to align DCS_n signal to center of the RCD clock DCK.

* Enable DCS Training: RW02[3:0]

    |Channel(A or B)|Operation|OP7|OP6|OP5|OP4|OP3|OP2|OP1|OP0| 
    | :------: |:--------:| :-:| :-:|  :-:| :-:|  :-:| :-:|  :-:| :-:| 
    | Channel A | DCS0_A_n Training Mode |  x|x|x|x|x|x|1|0| 
    | Channel A | DCS1_A_n Training Mode |  x|x|x|x|x|x|1|1| 
    | Channel B | DCS0_B_n Training Mode |  x|x|x|x|1|0|x|x| 
    | Channel B | DCS1_B_n Training Mode |  x|x|x|x|1|0|x|x| 

* Select DCS Output Pin: RW01[5]

    |Operation|OP7|OP6|OP5|OP4|OP3|OP2|OP1|OP0| 
    |:--------:| :-:| :-:|  :-:| :-:|  :-:| :-:|  :-:| :-:| 
    | Both Sub channels feedback on ALERT_n |  x|x|0|x|x|x|x|x|
    | Sub CH_A feedback on QLBD, Sub CH_B feedback on QLBS |  x|x|1|x|x|x|x|x| 

![](../images/commandbustraining/dcstraining.drawio)

##CATM

###Entry/Exit

* CATM is entered using MPC command.

    |  Function  |      MPC Mode      |OP7|OP6|OP5|OP4|OP3|OP2|OP1|OP0| 
    | :------: |:--------:| :-:| :-:|  :-:| :-:|  :-:| :-:|  :-:| :-:| 
    | Enter CA Training Mode | CATM |  0|0|0|0|0|0|1|1| 

CATM can only be entered when all Bank are Idle.

###Operation

![](../images/commandbustraining/catmsignals.drawio)