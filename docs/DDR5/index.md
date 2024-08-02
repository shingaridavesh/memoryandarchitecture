#DDR5

##Higher Speed
Data Rates have been increased from DDR4 to DDR5. This leads to increased performance and bandwidth.
- DDR4: 1600-3200 MT/s
- DDR5: 3200-6400 MT/s. Latest JEDEC specs have increased it to 8800 MT/s.

##Lower Voltage
Voltages have been decreased from DDR4 to DDR5. This leads to lower power on DDR5 as compared to DDR4.
- DDR4: VDD=1.2V, VDDQ=1.2V, VPP=2.5V
- DDR5: VDD=1.1V, VDDQ=1.1V, VPP=1.8V

##Densities
DDR5 devices have more densinites as compared to DDR4, meaning more memory.
- DDR4: 2Gb-16Gb
- DDR5: 8Gb-64Gb

##Prefetch
Prefetch width has increased on DDR5. DDR3 & DDR4 didnt change Prefetch width but DDR4 introduced concept of Bank Group. But DDR5 has increased Prefetch width. This enables higher data rates while keeping internal core clock similar to DDR4.
- DDR4: 8n
- DDR5: 16n

##Bank Groups
DDR4 introduced the concepts of Bank Groups. Bank Groups allow to have faster burst access, with latency between Bank Groups smaller than latency between Banks. DDR added more Bank Groups. 

|  Feature  |      DDR4      |   DDR5    |    
| :--------: |:-------------:| :---------:|
| Bank Group and Banks | 4GB x 4 Banks = 16 Banks| 8BG x 2 Banks = 16 Banks (8Gb x4/x8) <br> 4BG x 2 Banks = 8 Banks  (8Gb x16) <br> 8BG x 4 Banks = 32 Banks (16-64Gb x4/x8) <br> 4BG x 4 Banks = 16 Banks (16-64Gb x16) | 


###Pin



###DIMM Architecture
###Burst Length
DDR5 supports BL16 as native option. It also supports BC8 OTF, Fixed BL32, BL32 OTF (OTF = On the Fly). In comparison, DDR4 used BL8 as native option but also supported Fixed BC4, BC4 OTF and BL8 OTF.

##Pin Description

###Power and VREF

####VDD, VSS
DDR Core Power and Ground.
Present in both DDR4 and DDR5.

####VDDQ, VSSQ
DDR IO Power and Ground for Data Group Signals.
VDDQ is present in both DDR4 and DDR5. However, VSSQ is not mentioned in DDR5 JEDEC spec.
VSSQ is separate from VSS for improved noise immunity.

####VPP
1. VPP is the DRAM Activation Power Supply.  
2. Up until DDR3, we had charge pump inside the DRAM devices but they were inefficient. So from DDR4 onwards, we have charge pump inside the DDR devices which are used to supplu power when activation command is issued.

####VREFDQ and VREFCA
1. DDR3 uses SSTL for both DQ and CA lines, and they use an external VREFDQ and VREFCA reference voltage which is VDD/2.  
2. DR4 use POD for DQ and SSTL for CA, and VREFCA is external pin and VREFDQ is internally generated.  
3. DDR5 uses POD for both DQ and CA, and VREFDQ and VREFCA is internally generated.  

###Clock, Command and Control

###Data, Data Strobes, Data Mask

###Miscellaneous

####ZQ
1. ZQ pin is present in all DDR3, DDR4 and DDR5.
2. It is tied to 

###DIMM Pins



##Trainings
###Command Bus Training
Command Bus Training has been added to DDR5. It is derived from LPDDR4 but is slighlty different from LPDDR4. It consists of 2 trainings:
1. CS Training
2. CA Training
###Read Preamble Training
###Write Leveling (External + Internal)
###Read Training
###Write Training
