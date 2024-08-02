#DDR5

##Higher Speed
* Data Rates have been increased from DDR4 to DDR5. This leads to increased performance and bandwidth.
* DDR5 also removed option of DLL off, so it doesnt support lower speed (for low speed DLL off was used on previous generation DDRs).

|  Feature  |      DDR4      |   DDR5    |    
| :--------: |:-------------:| :---------:|
| Data Rate | 1600-3200 MT/s | 3200-6400 MT/s <br> Latest JEDEC specs have increased it to 8800 MT/s | 



##Lower Voltage
* Voltages have been decreased from DDR4 to DDR5. This leads to lower power on DDR5 as compared to DDR4.

|  Feature  |      DDR4      |   DDR5    |    
| :--------: |:-------------:| :---------:|
| VDD | 1.2V | 1.1V | 
| VDDQ | 1.2V | 1.1V | 
| VPP | 2.5V | 1.8V | 


##Densities
* DDR5 devices have more densinites as compared to DDR4, meaning more memory.

|  Feature  |      DDR4      |   DDR5    |    
| :--------: |:-------------:| :---------:|
| Density | 2Gb-16Gb| 8Gb-64Gb | 

##Prefetch
* Prefetch width has increased on DDR5. DDR3 & DDR4 didnt change Prefetch width but DDR4 introduced concept of Bank Group. But DDR5 has increased Prefetch width. This enables higher data rates while keeping internal core clock similar to DDR4.

|  Feature  |      DDR4      |   DDR5    |    
| :--------: |:-------------:| :---------:|
| Prefetch | 8n| 16n | 

##Bank Groups
* DDR4 introduced the concepts of Bank Groups. Bank Groups allow to have faster burst access, with latency between Bank Groups smaller than latency between Banks. DDR added more Bank Groups. 

|  Feature  |      DDR4      |   DDR5    |    
| :--------: |:-------------:| :---------:|
| Bank Group and Banks | 4GB x 4 Banks = 16 Banks| 8BG x 2 Banks = 16 Banks (8Gb x4/x8) <br> 4BG x 2 Banks = 8 Banks  (8Gb x16) <br> 8BG x 4 Banks = 32 Banks (16-64Gb x4/x8) <br> 4BG x 4 Banks = 16 Banks (16-64Gb x16) | 

##Burst Length
* DDR5 architecture allows for BL16 Burst Length as compared to BL8 Burst Length on DDR4.

|  Feature  |      DDR4      |   DDR5    |    
| :--------: |:-------------:| :---------:|
| Burst Length | BL8 (Native) <br> BC4 Fixed or BC4 OTF or BL8 OTF <br> BC4 Fixed| BL16 (Native) <br> BC8 OTF <br> BL32 Fixed <br> BL32 OTF | 

##Command Address Pins
* On DDR5, 

|  Feature  |      DDR4      |   DDR5    |    
| :--------: |:-------------:| :---------:|
| Burst Length | BL8 (Native) <br> BC4 Fixed or BC4 OTF or BL8 OTF <br> BC4 Fixed| BL16 (Native) <br> BC8 OTF <br> BL32 Fixed <br> BL32 OTF | 

##Commands
* DDR5 adds PRE~sb~(Precharge Same Bank) which enables precharging of a specific bank in each bank group, thus keeping active state of all others banks unchanged.
* DDR5 adds REF~sb~(Refresh Same Bank) which enables refreshing of a specific bank in each bank group, thus keeping all other banks in the bank group free to access.

|  Feature  |      DDR4      |   DDR5    |    
| :--------: |:-------------:| :---------:|
| Precharge Command | PRE~ab~: Precharge All Bank <br> PRE~pb~: Precharge Per Bank  | PRE~ab~: Precharge All Bank <br> PRE~pb~: Precharge Per Bank <br> PRE~sb~: Precharge Same Bank | 
| Refresh Command | REF~ab~: Precharge All Bank <br> REF~pb~: Precharge Per Bank  | REF~ab~: Precharge All Bank <br> REF~pb~: Precharge Per Bank <br> REF~sb~: Precharge Same Bank | 

##Commands

|  Feature  |      DDR4      |   DDR5    |    
| :--------: |:-------------:| :---------:|
| Mode Registers |  |  | 

##Commands

|  Feature  |      DDR4      |   DDR5    |    
| :--------: |:-------------:| :---------:|
| Mode Registers |  |  | 

##Commands

|  Feature  |      DDR4      |   DDR5    |    
| :--------: |:-------------:| :---------:|
| Mode Registers |  |  | 

##Commands

|  Feature  |      DDR4      |   DDR5    |    
| :--------: |:-------------:| :---------:|
| Mode Registers |  |  | 

##Commands

|  Feature  |      DDR4      |   DDR5    |    
| :--------: |:-------------:| :---------:|
| Mode Registers |  |  | 


##Mode Registers


|  Feature  |      DDR4      |   DDR5    |    
| :--------: |:-------------:| :---------:|
| Mode Registers |  |  | 

##DIMM Architecture







##Trainings
###Command Bus Training
Command Bus Training has been added to DDR5. It is derived from LPDDR4 but is slighlty different from LPDDR4. It consists of 2 trainings:
1. CS Training
2. CA Training
###Read Preamble Training
###Write Leveling (External + Internal)
###Read Training
###Write Training
