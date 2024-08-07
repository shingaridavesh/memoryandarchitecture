#Preamble, Postamble and Interamble

Before we have a valida data transaction whether we have a Read or Write operation, we need a  DQS Data Strobe, given DQ Data gets latched on DQS Data Strobe. A typical Read/Write transaction consists of __DQS Preamble__ + __DQS Toggling__ + __DQS Postamble__. 

*  __DQS Preamble__: Preamble provides a timing window for the receiving device to enable its receivers while a known/valid level is present on the DQS Data Strobe signal, this avoiding a false trigger of the receiver.  
*  __DQS Toggling__: Following Preamble, DQS Data Strobe toggles for the duration of data burst (which will be Burst Length). DQS toggles at same frequency as the clock (atleast as of release of DDR5). __NOTE:__ on LPDDR5/5x we have WCK which act as a strobe and toggles at different frequency as clock.  
*  __DQS Postamble__: Postamble is the valid DQS Data Strobe transitions following the DQS toggling.

##Differences

|  Types  |      DDR1      |   DDR2    |      DDR3      |      DDR4      |   DDR5    | Comment | 
| :--------: |:-------------:| :---------:| :---------:| :--------: | :-------------:| :-------------:|
| Read Premable | 1 tCK | 1 tCK | 1 tCK | 1 tCK, 2 tCK | 1 tCK, 2 tCK, 3 tCK, 4 tCK | Not 100% sure about DDR1, DDR2 <br> DDR5 supports 2 different kind of 2 tCK Preamble |
| Read Postamble | 0.5 tCK| 0.5 tCK | 0.5 tCK | 0.5 tCK | 0.5 tCK, 1.5 tCK | Not 100% sure about DDR1, DDR2 |
| Write Preamble  | 0.25 tCK | 0.25 tCK | 1 tCK | 1 tCK, 2 tCK | 2 tCK, 3 tCK, 4 tCK | Not 100% sure about DDR1, DDR2 |
| Write Postamble  | 0.5 tCK | 0.5 tCK | 0.5 tCK | 0.5 tCK | 0.5 tCK, 1.5 tCK | Not 100% sure about DDR1, DDR2 |

##DDR4
* On DDR4 we have option to choose from 2 settings for Read/Write Preamble, whereas Read/Write Postamble is fixed.  
* Read & Write Preamble can be chosen from MR4 and has option of 1 tCK and 2 tCK. The 1 tCK preamble applies to all speed, whereas 2 tCK preamble is valid for DDR4-2400/2666/3200.

###Preamble


##DDR5
* On DDR5, we have longer DQS Preambles and Postambles for Reads and Writes.  
* Reason for long Preamble and Postamble is to help with ISI on DQS at beginning of each burst, given we have more ISI at higher speeds at which DDR5 operate.  

###Read Preamble and Postamble
* DDR5 supports 5 different type of Read Preamble and 2 different types of Read Postamble.

|  Function  |      Mode Register      |   Operand    |      Data      |      Comment      |  
| :--------: |:-------------:| :---------:| :---------:| :--------: |
| Read Preamble | MR8 | OP[2:0] | 000B: 1 tCK - 10 Pattern <br> 001B: 2 tCK - 00 10 Pattern <br> 010B: 2 tCK - 11 10 Pattern (DDR4 Style) <br> 011B: 3 tCK - 00 00 10 Pattern <br> 100B: 4 tCK - 00 00 10 10 Pattern <br> 101B: Reserved <br>110B: Reserved <br>111B: Reserved| | 
| Read Postamble | MR8 | OP[6] | 0B: 0.5 tCK - 0 Pattern <br> 1B: 1.5 tCK - 010 Pattern | | 

* Read Preamble can be set through MR8 OP[2:0], and Read Postamble can be set through MR8 OP[6].

> **Read Preamble Types with Postamble=0.5 tCK (Source: Micron DDR5 Datasheet)**
> ![zoomify](../images/DDR_Types/Read_Pre_Post_1.png)
> **Read Preamble Types with Postamble=1.5 tCK (Source: Micron DDR5 Datasheet)**
> ![zoomify](../images/DDR_Types/Read_Pre_Post_2.png)

###Write Preamble and Postamble
* DDR5 supports 3 different type of Write Preamble and 2 different types of Write Postamble.

|  Function  |      Mode Register      |   Operand    |      Data      |      Comment      |  
| :--------: |:-------------:| :---------:| :---------:| :--------: |
| Write Preamble | MR8 | OP[4:3] | 00B: Reserved <br> 01B: 2 tCK - 00 10 Pattern (Default) <br> 10B: 3 tCK - 00 00 10 Pattern <br> 11B: 4 tCK - 00 00 10 10 Pattern | | 
| Write Postamble | MR8 | OP[7] | 0B: 0.5 tCK - 0 Pattern <br> 1B: 1.5 tCK - 000 Pattern | | 

* Write Preamble can be set through MR8 OP[4:3]], and Write Postamble can be set through MR8 OP[7].

> **Write Preamble Types with Postamble=0.5 tCK (Source: Micron DDR5 Datasheet)**
> ![zoomify](../images/DDR_Types/Write_Pre_Post_1.png)
> **Write Preamble Types with Postamble=1.5 tCK (Source: Micron DDR5 Datasheet)**
> ![zoomify](../images/DDR_Types/Write_Pre_Post_2.png)

###Read and Write Interamble

* As you would have observed that DDR5 has long Preambles and at higher speeds, we do use longer Preamble. But with longer Preamble, when we have back to back transaction, then we will have performance loss because of bubbles created by first transaction Postamble + second transaction Preamble.
* To combat this DDR5 has concept of Interamble. __Intermable__ happens when DQS Postamble of first burst is allowed to overlap with DQS Preamble of second burst.

__NOTE__: Interamble happens only when 2 transactions are of same type and same rank, which makes sense because DQS will be driven by Controller/PHY incase of Write and will be driven by DDR incase of Read. So valid cases where Interamble happens are:  
* Read-gap-Read, Read-gap-gap-Read etc.  
* Write-gap-Write, Write-gap-gap-Write etc.  
We will look at the examples for these scenarios.

####Read Interamble
Consider back to back Read operations and the timing between 2 Read operation is tCCD (technically tCCD_S or tCCD_L). The minimum time is BL/2, given it takes BL/2 clock cycles for one burst to finish, before second burst can start. So if we consider this scenario, then we will have something as follows:

> **Read Interamble with No Gap (Source: Micron DDR5 Datasheet)**
> ![zoomify](../images/DDR_Types/RD_Interamble_0_Gap.png)

> **Read Interamble with 1 CLK Gap (Source: Micron DDR5 Datasheet)**
> ![zoomify](../images/DDR_Types/RD_Interamble_1_Gap.png)

> **Read Interamble with 2 CLK Gap (Source: Micron DDR5 Datasheet)**
> ![zoomify](../images/DDR_Types/RD_Interamble_2_Gap.png)

> **Read Interamble with 5 CLK Gap (Source: Micron DDR5 Datasheet)**
> ![zoomify](../images/DDR_Types/RD_Interamble_5_Gap.png)

####Write Interamble
Consider back to back Write operations and the timing between 2 Write operation is tCCD (technically tCCD_S or tCCD_L). The minimum time is BL/2, given it takes BL/2 clock cycles for one burst to finish, before second burst can start. So if we consider this scenario, then we will have something as follows:

> **Write Interamble with No Gap (Source: Micron DDR5 Datasheet)**
> ![zoomify](../images/DDR_Types/WR_Interamble_0_Gap.png)

> **Write Interamble with 1 CLK Gap (Source: Micron DDR5 Datasheet)**
> ![zoomify](../images/DDR_Types/WR_Interamble_1_Gap.png)

> **Write Interamble with 2 CLK Gap (Source: Micron DDR5 Datasheet)**
> ![zoomify](../images/DDR_Types/WR_Interamble_2_Gap.png)

> **Write Interamble with 5 CLK Gap (Source: Micron DDR5 Datasheet)**
> ![zoomify](../images/DDR_Types/WR_Interamble_5_Gap.png)