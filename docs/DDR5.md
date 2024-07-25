#DDR5

##New Features

###Higher Speed

###Lower Voltage

###Pin

####Power and VREF
|  Pins  |      DDR1      |   DDR2    |      DDR3      |      DDR4      |   DDR5    |   Comments    |
| :--------: |:-------------:| :---------:| :---------:| :--------: | :-------------:| :---------:|
| VDDQ  | 2.5V | 1.8V | 1.5V | 1.2V | 1.1V | DQ Power Supply |
| VDD | 2.5V| 1.8V| 1.5V | 1.2V | 1.1V | Core Power Supply |
| VSS | GND| GND | GND | GND | GND | Ground |
| VSSQ | GND| GND | GND | GND | Not Present | DQ Ground |
| VDDL | Not Present | 1.8V | Not Present | Not Present | Not Present | DLL Power Supply |
| VSSDL | Not Present | GND | Not Present | Not Present | Not Present | DLL Ground |
| VPP | Not Present | Not Present | Not Present | 2.5V | 1.8V | DRAM Activating Power Supply |
| VREF | Present | Present | Not Present | Not Present | Not Present | Reference Voltage used for both CA and DQ lines |    
| VREFCA | Not Present | Not Present | Present | Present | Present | Reference Volatge for Control, Command and Address lines |
| VREFDQ | Not Present | Not Present | Present | Not Present | Not Present | Reference Volatge for Data lines |

####Clock, Command and Control
|  Pins  |      DDR4      |   DDR5    |   Comments    |
| :--------: |:-------------:| :---------:|:---------:|
| CK_t, CK_c  | Present |     Present | Differential Clocks |
| CKE |   Present    |       Not Present | Clock Enable |
| CS_n | Present |     Not Present | Chip Select |
| Command Address |   A0-A17    |      CA0-CA13 |  |
| ACT_n |   Present    |      Not Present | Activation Command Input |
| Bank Group | BG0-BG1 |     Not Present | Bank Group |
| Bank Address | BA0-BA1 |     Not Present | Bank Address |
| CAI | Not Present |     Present | Command Address Mirroring |
| ODT | Present |     Not Present | On-Die Termination Pin |

####Data, Data Strobes, Data Mask
|  Pins  |      DDR4      |   DDR5    |   Comments    |
| :--------: |:-------------:| :---------:|:---------:|
| DQ  | Present |     Present | Data |
| DQS_t, DQS_c, <br> DQSU_t, DQSU_c <br> DQSL_t, DQSL_c |   Present    |       Present | Data Strobe |
| TDQS_t, TDQS_c | Present |     Not Present | Termination Data Strobe |
| DM_n |   A0-A17    |      CA0-CA13 | Data Mask |
| DBI_n |   Present    |      Not Present | Data Bus Inversion |

####Miscellaneous
|  Pins  |      DDR4      |   DDR5    |   Comments    |
| :--------: |:-------------:| :---------:|:---------:|
| ZQ  | Present |     Present | Data |
| LBDQ |   Not Present    |       Present | Loopback Data Output |
| LBDQS | Not Present |     Not Present | Loopback Data Strobe |
| TEN |   Present    |      Present | Connectivity Test Mode |
| ALERT_n |   Present    |      Not Present | Data Bus Inversion |
| RESET_n | Present |     Present |  |

####DIMM Pins
|  Pins  |      DDR4      |   DDR5    |   Comments    |
| :--------: |:-------------:| :---------:|:---------:|
| VTT  | Present |     Present |  |
| VDDSPD |   Present    |       Present |  |
| V_12, V_BULK | Present |     Present |  |
| SCL |   Present    |      Present |  |
| SDA |   Present    |      Present |  |
| SA | Present |     Present |  |

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
