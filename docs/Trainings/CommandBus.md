##CSTM

###Entry/Exit

* CSTM is entered and exited using MPC command.

    |  Function  |      MPC Mode      |OP7|OP6|OP5|OP4|OP3|OP2|OP1|OP0| 
    | :------: |:--------:| :-:| :-:|  :-:| :-:|  :-:| :-:|  :-:| :-:| 
    | Exit CS Training Mode | CSTMX |  0|0|0|0|0|0|0|0| 
    | Enter CS Training Mode | CSTMN |  0|0|0|0|0|0|0|1| 

* CSTM can only be entered when all Bank are Idle.

###Operation

![](../images/commandbustraining/cstmsignals.drawio)

![](../images/commandbustraining/cstmlogic.drawio)

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

##CATM

###Entry/Exit

* CATM is entered using MPC command.

    |  Function  |      MPC Mode      |OP7|OP6|OP5|OP4|OP3|OP2|OP1|OP0| 
    | :------: |:--------:| :-:| :-:|  :-:| :-:|  :-:| :-:|  :-:| :-:| 
    | Enter CA Training Mode | CATM |  0|0|0|0|0|0|1|1| 

CATM can only be entered when all Bank are Idle.

###Operation

![](../images/commandbustraining/catmsignals.drawio)