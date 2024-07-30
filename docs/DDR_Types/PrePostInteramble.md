#Preamble, Postamble and Interamble

Before we have a valida data transaction whether we have a Read or Write operation, we need a  DQS Data Strobe, given DQ Data gets latched on DQS Data Strobe. A typical Read/Write transaction consists of DQS Preamble + DQS Toggling + DQS Postamble.
1. Preamble provides a timing window for the receiving device to enable its receivers while a known/valid level is present on the DQS Data Strobe signal, this avoiding a false trigger of the receiver.  
2. Following Preamble, DQS Data Strobe toggles for the duration of data burst (which will be Burst Length). DQS toggles at same frequency as the clock (atleast as of release of DDR5). __NOTE:__ on LPDDR5/5x we have WCK which act as a strobe and toggles at different frequency as clock.  
3. Postamble is the valid DQS Data Strobe transitions following the DQS toggling.

|  Types  |      DDR1      |   DDR2    |      DDR3      |      DDR4      |   DDR5    |  
| :--------: |:-------------:| :---------:| :---------:| :--------: | :-------------:|
| Read Premable | 2002| 2006 | 2010 | 1nCK,2nCK | 2019 |
| Read Postamble | 128Mb-1Gb| 128Mb-4Gb | 512Mb-8Gb | 0.5nCK | 8Gb-64Gb |
| Write Preamble  | 200-400 | 400-800 | 800-2133 | 1nCK,2nCK | 3200-6400 |
| Write Postamble  | 2.5 | 1.8 | 1.5 | 0.5nCK | 1.1 |

##DDR4
1. On DDR4 we have option to choose from 2 settings for Read/Write Preamble, whereas Read/Write Postamble is fixed.  
2. Read & Write Preamble can be chosen from MR4 and has option of 1 tCK and 2 tCK. The 1 tCK preamble applies to all speed, whereas 2 tCK preamble is valid for DDR4-2400/2666/3200.

###Preamble


##DDR5
1. On DDR5, we have longer DQS Preambles and Postambles for Reads and Writes.  
2. Reason for long Preamble and Postamble is to help with ISI on DQS at beginning of each burst, given we have more ISI at higher speeds at which DDR5 operate.  
