#DDR5

##New Features

###Higher Speed

###Lower Voltage

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
