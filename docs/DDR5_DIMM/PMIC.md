##PMIC
From DDR5 onwards, there is a PMIC present on the DIMM itself, which provides the power all the voltage rails of DDR devices and all the other peripheral devices on the DIMM (TS, SPD, RCD etc). There are variants of PMIC for RDIMMs and LRDIMMs namely PMIC5000 and PMIC5010. Some important features of PMIC are as follows:

* Features 4 step down switching regulator and 3 LDO egulators. Supports ~15W of power.
* Has input from VIN_Bulk for switching regulators and VIN_Mgmt for rest of the PMIC. PMIC do support automatic switchover from VIN_Mgmt to VIN_Bulk.
* Uses I2C or I3C interface and can operate at 1MHz on I2C and 12.5MHz on I3C interface.
* Programmable output voltages, power up and power down sequence for switch regulators.
* Input and output power good status reporting mechanism.
• VIN_Bulk input supply protection feature: Input over voltage.
• Output switch regulators protection feature: Output over voltage, output under voltage, output current limiter.
• Output current and power measurement, output current threshold mechanism.
• Provides temperature measurement, temperature warning threshold, critical temperature shutdown.

![](../images/dimm/pmic.drawio)

###Voltage and Initialization

* PMIC has 2 input supplies from platform: VIN_Bulk and VIN_Mgmt.
* VIN_Bulk is used by PMIC for all output regulators except for VOUT_1.8V and VOUT_1.0V LDO output regulators (In switchover mode, even these are supplied by VIN_Bulk)
* VIN_Mgmt is used to read out its internal non-volatile memory content adn to supply VOUT_1.8V and VOUT_1.0V to other devices such as SPD, TS and RCD on DIMM.
* At first power on, VIN_Mgmt supply shall reach minimum of 2.8V before it can be detected as a valid input supply to PMIC. Once it reaches 2.8V, VOUT_1.8V and VOUT_1.0V is valid and CAMP signal is driven low by PMIC (it is floating before).

![](../images/dimm/pmic_init.drawio)


###Register Space
PMIC register space is divided into 3 broad categories:

* Host Region Registers
* DIMM Vendor Region Registers
* PMIC Vendor Region Registers

|Register Type| Register Address Range | Comments | 
|:-:|:-:|:-|
| Host Region Registers | 0x00 - 0x3F |  Host Accesible Registers <br> Some registers can only be modified when PMIC is not in Secure state but can be read anytime. |
| DIMM Vendor Region Registers | 0x40 - 0x6F | DIMM Vendor Accesigble Registers <br> Allows DIMM vendor to program PMIC for their specif DIMM/DRAm device <br> Password protected and incorrect password will result in returned value of 0 |
| PMIC Vendor Region Registers | 0x70 - 0xFF | PMIC Vendor Accesible Registers <br> Password protected and incorrect password will result in returned value of 0 <br> JEDEC doesnt even define what registers can/should be. |


###Measuring Current/Power
To understand the measurement capability of SWs, we need to first see whether PMIC can provide current or power measurement. We can check that through 0x1B[6]. If it provides power measurement then we need to check whether it provides individual measurement or only supports sum of all SWs. This can be known by reading register 0x1A[1]. Both the bit fields are shown below.

|Register| Bit | Attribute | Description |
|:-:|:-:|:-:|:-:|
| 0x1B | 6 | RW | CURRENT_OR_POWER_METER_SELECT <br> PMIC Output Regulator Measurement - Current or Power Meter <br> 0 = Report Current Measurements in registers <br> 1 = Report Power Measurements in registers|
| 0x1A | 1 | RW | OUTPUT_POWER_SELECT <br> Switch Regulator Output Power Select <br> 0 = Report individual power for each rail in Reg 0x0C, Reg 0x0D, Reg 0x0E, and Reg 0x0F <br> 1 = Report total power of each rail in Reg 0x0C|