##Components
There are 5 major components present on DIMMs other than the DDR dies.

* SPD Hub
* PMIC
* RCD
* Temperature Sensors

##Specifications
* SPD Hub
    * SPD5118 HUB and SERIAL PRESENCE DETECT DEVICE STANDARD (JESD300-5B.01)
* SPD Content
    * DDR5 SERIAL PRESENCE DETECT (SPD) CONTENTS (JESD400-5C)
* PMIC
    * PMIC50x0 Power Management IC Standard (JESD301-1A.02 Rev. 1.8.5). NOTE: PMIC5020 specification is for PMIC for MRDIMMs.
* RCD
    * DDR5 Registering Clock Driver Definition - DDR5RCD04 (JESD82-514.01)
    * DDR5 Registering Clock Driver Definition - DDR5RCD03 (JESD82-513)
    * DDR5 Registering Clock Driver Definition - DDR5RCD02 (JESD82-512)
    * DDR5 REGISTERING CLOCK DRIVER DEFINITION - DDR5RCD01 (JESD82-511)
* Temperature Sensors
    * TS511X, TS521X Serial Bus Thermal Sensor Device Standard (JESD302-1A)


##SPD Hub

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

###Input & Output Voltage

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


##RCD

##Temperature Sensors
There are 2 sensors present on the DDR5 DIMM. There might be a temperature sensor present inside the SPD hub, but in this we talk about the TS0 and TS1 specifically.  Some important features of the TS are as follows:

* TS typically provide temperature measurement from -40C to 125C in granularity of 0.25C. 
* Uses I2C or I3C interface and can operate at 1MHz on I2C and 12.5MHz on I3C interface.


![](../images/dimm/ts5.drawio)

###Reading Temperature
The temperature can be read from the TS in multiple of 0.25C ranging from -256.00C to +255.75C. Even though this range is supported but actual device might support smaller temperature range, typially -40C to 95C or 125C. 

There are 2 registers (both 8 bits) which are used in conjuction to read the temperature. In the JEDEC spec it is RO register MR49 and MR50 titles "TS Current Sensed Tempertaure - Low Byte" and "TS Current Sensed Temperature - High Byte".

* High Byte: Top 3 bits [7:5] are reserved and are crossed out below. Next bit [4] is sign bit where 0 means positive and 1 means negative. Remaining 4 bits [3:0] are upper bits indicating temperature along with lower bits from Low Byte.
* Low Byte: Top 6 bits [7:2] are lower bits of temperature and remaining 2 bits [1:0] are reserved and are crossed out below.

|High Byte| Low Byte | Temperature (C)|
|:-:|:-:|:-:|
| ~~000~~ ==0== 1111 | 111111 ~~00~~ | +255.75 |
| ~~000~~ ==0== 0101 | 111100 ~~00~~ | + 95.00 |
| ~~000~~ ==0== 0101 | 010100 ~~00~~ | + 85.00 |
| ~~000~~ ==0== 0100 | 101100 ~~00~~ | + 75.00 |
| ~~000~~ ==0== 0000 | 000100 ~~00~~ | +  1.00 |
| ~~000~~ ==0== 0000 | 000011 ~~00~~ | +  0.75 |
| ~~000~~ ==0== 0000 | 000010 ~~00~~ | +  0.50 |
| ~~000~~ ==0== 0000 | 000001 ~~00~~ | +  0.25 |
| ~~000~~ ==0== 0000 | 000000 ~~00~~ |    0.00 |
| ~~000~~ ==1== 1111 | 111111 ~~00~~ | -  0.25 |
| ~~000~~ ==1== 1111 | 111110 ~~00~~ | -  0.50 |
| ~~000~~ ==1== 1111 | 111101 ~~00~~ | -  0.75 |
| ~~000~~ ==1== 1111 | 111100 ~~00~~ | -  1.00 |
| ~~000~~ ==1== 1101 | 100000 ~~00~~ | - 40.00 |
| ~~000~~ ==1== 0000 | 000000 ~~00~~ | -256.00 |

###Interface

* There are multiple Temperature Sensors on DDR5 DIMM, so each of them should have a unique address (LID). HID for the TS can be set through **MR7[3:1]**. As per JEDEC spec, TS0 LID = 0010 and TS1 LID = 0110.
* Communication can be made via I2C or I3C. The link comes up in I2C by default and can be changed to I3C. You can read current interface through **MR18[5]** INF_SEL to see if it is I2C protocol or I3C protocol. 
* At bringup when TS is in I2C mode, only 3 CCCs (DEVCTRL, SETHID and SETAASA) are allowed.
* Controller can put the TS device into I3C mode by issuing SETAASA CCC.
* Controller can set the HID of the TS device by issuing SETHID CCC.


###Interrupts
Temperature sensor doesnt have a dedicated interrupt  or alert pin but supports interrupt generation using In Band Interrupts (IBI) on the SDA pin. It should be noted that IBI is supported only during I3C mode of operation. There are generally 2 events for which IBI is used:

* Error Event: Event corresponding to Parity or PEC (Packet Error Check) error.
* Temperature Event: Event corresponding to temperature events. There are 4 events specifically:
    * Temperature falling below Lower Temperature limit 
    * Temperature falling below Crtical Lower Temperature limit 
    * Temperature rising above Higher Temperature limit
    * Temperature rising above Crtical Higher Temperature limit

####Enabling Interrupt/Event
To enable the interrupt/event, there are multiple registers bits as follows:

* **MR18[6]** PAR_DISB = 0, Parity Enable
* **MR18[7]** PEC_EN = 1, PEC Enable
* **MR27[3]** IBI_TS_CRIT_LOW_EN = 1, In Band Error Interrupt Enable for Temp Sensor Critical Low
* **MR27[2]** IBI_TS_CRIT_HIGH_EN = 1, In Band Error Interrupt Enable for Temp Sensor Critical High
* **MR27[1]** IBI_TS_LOW_EN = 1, In Band Error Interrupt Enable for Temp Sensor Low
* **MR27[0]** IBI_TS_HIGH_EN = 1, In Band Error Interrupt Enable for Temp Sensor High
    
####Setting Temperature Threshold for Temperature events
There are multiple registers to use this feature. There are 4 mode registers to set the threshold for these interrupts.

* **Temperature above High Limit**: Threshold temperature set in **MR28** ((Thermal Sensor High Limit Configuration - Low Byte) and **MR29** (Thermal Sensor High Limit Configuration - High Byte)
* **Temperature below Low Limit**: Threshold temperature set in **MR30** ((Thermal Sensor Low Limit Configuration - Low Byte) and **MR31** (Thermal Sensor Low Limit Configuration - High Byte)
* **Temperature above Crtical High Limit**: Threshold temperature set in **MR32** (Thermal Sensor Critical Temperature High Limit Configuration - Low Byte) and **MR33** (Thermal Sensor Critical Temperature High Limit Configuration- High Byte)
* **Temperature below Critical Low Limit**: Threshold temperature set in **MR34** (Thermal Sensor Critical Temperature Low Limit Configuration - Low Byte) and **MR35** (Thermal Sensor Critical Temperature Low Limit Configuration- High Byte)

####Reading Interrupt/Event Cause
To read the cause of the interrupt/event, there are multiple Read Only registers bits as follows:

* If **MR51[0]** TS_HIGH_STATUS = 1, Temperature is above the limit set in MR28 and MR29.
* If **MR51[1]** TS_LOW_STATUS = 1, Temperature is below the limit set in MR30 and MR31.
* If **MR51[2]** TS_CRIT_HIGH_STATUS = 1, Temperature is above the limit set in MR32 and MR33.
* If **MR51[3]** TS_CRIT_LOW_STATUS = 1, Temperature is below the limit set in MR34 and MR35.
* If **MR52[0]** PAR_ERROR_STATUS = 1, Parity error in one or more bytes.
* If **MR52[1]** PEC_ERROR_STATUS = 1, PEC Error in one or more packets.


##Communication to Components


![](../images/dimm/ddr5_dimm_spd_connections.drawio)