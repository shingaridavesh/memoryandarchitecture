#DDR5

##New Features
###Higher Speed
###Lower Voltage
###Pin

####Power and VREF
|  Pins  |      DDR4      |   DDR5    |   Comments    |
| :--------: |:-------------:| :---------:|:---------:|
| VDDQ  | 1.2V |     1.1V | DQ Power Supply |
| VDD |   1.2V    |       1.1V | Core Power Supply |
| VSS | GND |     GND | Ground |
| VSSQ |   GND    |      Not Present | DQ Ground |
| VPP | 2.5V |     1.8V | DRAM Activating Power Supply |
| VREFCA | External |     Internally Generated | Reference Volatge for CA lines |

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

###DIMM Architecture
###Burst Length
DDR5 supports BL16 as native option. It also supports BC8 OTF, Fixed BL32, BL32 OTF (OTF = On the Fly). In comparison, DDR4 used BL8 as native option but also supported Fixed BC4, BC4 OTF and BL8 OTF.

##Trainings
###Command Bus Training
Command Bus Training has been added to DDR5. It is derived from LPDDR4 but is slighlty different from LPDDR4. It consists of 2 trainings:
1. CS Training
2. CA Training
###Read Preamble Training
###Write Leveling (External + Internal)
###Read Training
###Write Training
