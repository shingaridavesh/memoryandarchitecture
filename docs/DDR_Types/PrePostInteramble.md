#Preamble, Postamble and Interamble

Before we have a valida data transaction whether we have a Read or Write operation, we need a  DQS Data Strobe, given DQ Data gets latched on DQS Data Strobe. A typical Read/Write transaction consists of DQS Preamble + DQS Toggling + DQS Postamble.  
1. Preamble provides a timing window for the receiving device to enable its receivers while a known/valid level is present on the DQS Data Strobe signal, this avoiding a false trigger of the receiver.  
2. Following Preamble, DQS Data Strobe toggles for the duration of data burst (which will be Burst Length). DQS toggles at same frequency as the clock (atleast as of release of DDR5). __NOTE:__ on LPDDR5/5x we have WCK which act as a strobe and toggles at different frequency as clock.  
3. Postamble is the valid DQS Data Strobe transitions following the DQS toggling.

##Differences

|  Types  |      DDR1      |   DDR2    |      DDR3      |      DDR4      |   DDR5    |  
| :--------: |:-------------:| :---------:| :---------:| :--------: | :-------------:|
| Read Premable | 2002| 2006 | 2010 | 1 tCK, 2 tCK | 1 tCK, 2 tCK, 3 tCK, 4 tCK |
| Read Postamble | 128Mb-1Gb| 128Mb-4Gb | 512Mb-8Gb | 0.5 tCK | 0.5 tCK, 1.5 tCK |
| Write Preamble  | 200-400 | 400-800 | 800-2133 | 1 tCK, 2 tCK | 2 tCK, 3 tCK, 4 tCK |
| Write Postamble  | 2.5 | 1.8 | 1.5 | 0.5 tCK | 0.5 tCK, 1.5 tCK |

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
| Read Preamble | R/W| OP[2:0] | 000B: 1 tCK - 10 Pattern <br> 
001B: 2 tCK - 0010 Pattern <br> 
010B: 3 tCK - 1110 Pattern (DDR4 Style) <br> 
011B: 3 tCK - 000010 Pattern <br> 
100B: 4 tCK - 00001010 Pattern <br> 
101B: Reserved <br>
110B: Reserved <br>
111B: Reserved| | 

2. These settings can be set through MR8 OP[2:0]

###Write Preamble

###Read Postamble

###Write Postamble
