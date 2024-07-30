#Preamble, Postamble and Interamble

Before we have a valida data transaction whether we have a Read or Write operation, we need a  DQS Data Strobe, given DQ Data gets latched on DQS Data Strobe. A typical Read/Write transaction consists of __DQS Preamble__ + __DQS Toggling__ + __DQS Postamble__.  
1. __DQS Preamble__: Preamble provides a timing window for the receiving device to enable its receivers while a known/valid level is present on the DQS Data Strobe signal, this avoiding a false trigger of the receiver.  
2. __DQS Toggling__: Following Preamble, DQS Data Strobe toggles for the duration of data burst (which will be Burst Length). DQS toggles at same frequency as the clock (atleast as of release of DDR5). __NOTE:__ on LPDDR5/5x we have WCK which act as a strobe and toggles at different frequency as clock.  
3. __DQS Postamble__: Postamble is the valid DQS Data Strobe transitions following the DQS toggling.

##Differences

|  Types  |      DDR1      |   DDR2    |      DDR3      |      DDR4      |   DDR5    | Comment | 
| :--------: |:-------------:| :---------:| :---------:| :--------: | :-------------:| :-------------:|
| Read Premable | 1 tCK | 1 tCK | 1 tCK | 1 tCK, 2 tCK | 1 tCK, 2 tCK, 3 tCK, 4 tCK | Not 100% sure about DDR1, DDR2 <br> DDR5 supports 2 different kind of 2 tCK Preamble |
| Read Postamble | 0.5 tCK| 0.5 tCK | 0.5 tCK | 0.5 tCK | 0.5 tCK, 1.5 tCK | Not 100% sure about DDR1, DDR2 |
| Write Preamble  | 0.25 tCK | 0.25 tCK | 1 tCK | 1 tCK, 2 tCK | 2 tCK, 3 tCK, 4 tCK | Not 100% sure about DDR1, DDR2 |
| Write Postamble  | 0.5 tCK | 0.5 tCK | 0.5 tCK | 0.5 tCK | 0.5 tCK, 1.5 tCK | Not 100% sure about DDR1, DDR2 |

##DDR4
1. On DDR4 we have option to choose from 2 settings for Read/Write Preamble, whereas Read/Write Postamble is fixed.  
2. Read & Write Preamble can be chosen from MR4 and has option of 1 tCK and 2 tCK. The 1 tCK preamble applies to all speed, whereas 2 tCK preamble is valid for DDR4-2400/2666/3200.

###Preamble


##DDR5
1. On DDR5, we have longer DQS Preambles and Postambles for Reads and Writes.  
2. Reason for long Preamble and Postamble is to help with ISI on DQS at beginning of each burst, given we have more ISI at higher speeds at which DDR5 operate.  

###Read Preamble
1. DDR5 supports 5 different type of Read Preamble Settings.
|  Function  |      Register Type      |   Operand    |      Data      |      Comment      |  
| :--------: |:-------------:| :---------:| :---------:| :--------: |
| Read Preamble | R/W| OP[2:0] | 000B: 1 tCK - 10 Pattern <br> 001B: 2 tCK - 0010 Pattern <br> 010B: 3 tCK - 1110 Pattern (DDR4 Style) <br> 011B: 3 tCK - 000010 Pattern <br> 100B: 4 tCK - 00001010 Pattern <br> 101B: Reserved <br>110B: Reserved <br>111B: Reserved| | 
2. These settings can be set through MR8 OP[2:0].

###Write Preamble

###Read Postamble

###Write Postamble

