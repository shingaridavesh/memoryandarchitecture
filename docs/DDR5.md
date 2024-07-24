#DDR5

##New Features
###Higher Speed
###Lower Voltage
###Pin

####Power
|  Pins  |      DDR4      |   DDR5    |   Comments    |
| :--------: |:-------------:| :---------:|:---------:|
| VDDQ  | 1.2V |     1.1V | DQ Power Supply |
| VDD |   1.2V    |       1.1V | Core Power Supply |
| VSS | GND |     GND | Ground |
| VSSQ |   GND    |      Not Present | DQ Ground |
| VPP | 2.5V |     1.8V | DRAM Activating Power Supply |

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
