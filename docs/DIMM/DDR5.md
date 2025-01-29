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

##RCD

##Temperature Sensors

There are 2 sensors present on the DDR5 DIMM. There might be a temperature sensor present inside the SPD hub, but in this we talk about the TS0 and TS1 specifically. The temperature can be read from the TS in multiple of 0.25C ranging from -256.00C to +255.75C.  

###Reading Temperature
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

###Interrupts
Temperature sensor doesnt have a dedicated interrupt  or alert pin but supports interrupt generation using In Band Interrupts (IBI) on the SDA pin. It should be noted that IBI is supported only during I3C mode of operation. There are generally 2 events for which IBI is used:

* Error Event: Event corresponding to Parity or PEC error.
* Temperature Event: Event corresponding to temperature falling below Lower Temp limit / Crtical Lower Temp limit or rising above Higher Temp limit / Crtical Higher Temp Limit. There are multiple registers to use this feature. There are 4 mode registers to set the threshold for these interrupts.
    * **Temperature above High Limit**: Threshold temperature set in **MR28** ((Thermal Sensor High Limit Configuration - Low Byte) and **MR29** (Thermal Sensor High Limit Configuration - High Byte)
    * **Temperature below Low Limit**: Threshold temperature set in **MR30** ((Thermal Sensor Low Limit Configuration - Low Byte) and **MR31** (Thermal Sensor Low Limit Configuration - High Byte)
    * **Temperature above Crtical High Limit**: Threshold temperature set in **MR32** (Thermal Sensor Critical Temperature High Limit Configuration - Low Byte) and **MR33** (Thermal Sensor Critical Temperature High Limit Configuration- High Byte)
    * **Temperature below Critical Low Limit**: Threshold temperature set in **MR34** (Thermal Sensor Critical Temperature Low Limit Configuration - Low Byte) and **MR35** (Thermal Sensor Critical Temperature Low Limit Configuration- High Byte)

####Reading Interrupt/Event Cause
To read the cause of the interrupt/event, there are multiple Read Only registers bits as follows:

* MR51[0] TS_HIGH_STATUS = 1, Temperature is above the limit set in MR28 and MR29.
* MR51[1] TS_LOW_STATUS = 1, Temperature is below the limit set in MR30 and MR31.
* MR51[2] TS_CRIT_HIGH_STATUS = 1, Temperature is above the limit set in MR32 and MR33.
* MR51[3] TS_CRIT_LOW_STATUS = 1, Temperature is below the limit set in MR34 and MR35.
* MR52[0] PAR_ERROR_STATUS = 1, Parity error in one or more bytes.
* MR52[1] PEC_ERROR_STATUS = 1, PEC Error in one or more packets.


##Communication to Components


![](../images/dimm/ddr5_dimm_spd_connections.drawio)