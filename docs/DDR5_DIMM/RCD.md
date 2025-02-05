##RCD

* DDR5RCD04 is a registering clock driver used on DDR5 RDIMMs.
* Primary function of RCD is to buffet the Clock, Chip-Select and Command/Address bus between the MemoryController/PHY and DRAm device.
* RCD contains 2 separate channels which have some common lopgic as clocking but otherwise operate indepdently of each other.
* RCD has common clock input and PLL but produces separate clock outputs to the DRAM channels.

###Input/Output

![](../images/dimm/rcd_input_output_pins.drawio)

**Clocks**

|Input/Output| Signal Name | Type | Description | 
|:-:|:-:|:-:|:-|
|Input | DCK_t/DCK_c | POD differential | Differential system clock input pair to the PLL. The clock is common to both RCD channels, CH_A and CH_B. |
|Output | Q[D:A]CK_[B:A]_t/_c | POD differential | Clock outputs to the DRAMs. <br> 4 copies of Clock per channel. |

**Control/Chip-Selects**

|Input/Output| Signal Name | Type | Description | 
|:-:|:-:|:-:|:-|
|Input | DCS[1:0]_[B:A]_n | POD VREF based | Chip Select inputs to the RCD. Two inputs per DRAM channel.|
|Output | Q[B:A]CS[1:0]_[B:A]_n | POD | Chip Select signals to the DRAMs. <br> 2 copies of each signal.|

**Command Address**

|Input/Output| Signal Name | Type | Description | 
|:-:|:-:|:-:|:-|
|Input | DCA[6:0]_[B:A] | POD VREF based | Command/Address bus inputs to the RCD. Separate sets per channel.|
|Input | DPAR_[B:A] | POD VREF based | Command Address input parity is received on the DPAR pin and should maintain even parity across the CA inputs. DPAR is sampled  at the rising and falling edges of the input clock.|
|Output | Q[B:A]CA[13:0]_[B:A] | POD | Command/Address bus outputs to the DRAMs, valid after the specified clock count and immediately following a rising edge of the clock. Two copies of each signal. When Output Inversion is enabled in RW00[5], Copy B output signals get inverted during all commands other than Deselect. Both copies drive High during idle cycles (i.e., Deselect).|

**Loopback**

|Input/Output| Signal Name | Type | Description | 
|:-:|:-:|:-:|:-|
|Input | DLBD_[B:A] <br> DLBS_[B:A] | POD VREF based | Loopback Inputs to RCD <br> DLBD - Loopback Data <br> DLBS - Loopback Strobe|
|Output | QLBD <br> QLBS | POD | Loopback outputs to Host <br> QLBD - loopback Data <br> QLBS - loopback Strobe|

**Reset**

|Input/Output| Signal Name | Type | Description | 
|:-:|:-:|:-:|:-|
|Input | DRST_n| CMOS | Active LOW asynchronous reset input. When LOW, it causes a reset of the internal latches and disables the outputs, thereby forcing the outputs to float|
|Output | QRST_[B:A]_n | CMOS | Re-driven or CMD based reset. This is an asynchronous output. It is the responsibility of the RCD QRST_n to reset the DDR5 SDRAM on all DIMM topologies. The QRST outputs are asserted at power up. RCD requires MRW to independently de-assert to allow staggering of sub-channels|

**Error**

|Input/Output| Signal Name | Type | Description | 
|:-:|:-:|:-:|:-|
|Input | DERROR_IN_[B:A]_n | Low voltage swing POD input | DRAM CRC ALERT_n output is connected to this input pin, which in turn is buffered and redriven to the ALERT_n output of the register. There is a separate signal per channel|
|Output | ALERT_n | POD | When LOW, this output indicates that a parity error was identified associated with the CA inputs when parity checking is enabled or that the DERROR_IN_n input was asserted, regardless of whether parity checking is enabled or not. One signal for the two channels.|

**Sideband Bus**

|Input/Output| Signal Name | Type | Description | 
|:-:|:-:|:-:|:-|
|Power Input | VDDIO | Power input | Sideband Bus Power Input|
|Input | SCL | CMOS input | Sideband Bus Clock|
|Input/output | SDA | Open drain or push pull IO | Sideband Bus Datal|

###Operations


###Training Modes

RCD supports multiple trainings:

* CS Training Modes
    * DCS Training Mode
    * QCS Training Mode
* CA Training Modes
    * DCA Training Mode
    * QCA Training Mode
* DFE Training Modes
    * DCA Decision Feedback Equalization
    * DCS Decision Feedback Equalization

###Control Words

###Settings Delays

There are multiple delay control for output signals of RCD:

* RW12 - QACK Output Delay
* RW13 - QBCK Output Delay
* RW14 - QCCK Output Delay
* RW15 - QDCK Output Delay
* RW17 - QACS0 Output Delay
* RW18 - QACS1 Output Delay
* RW19 - QBCS0 Output Delay
* RW1A - QBCS1 Output Delay
* RW1B - QACA Output Delay
* RW1C - QBCA Output Delay

All the above delays have granularity from (0/64)*t~CK~ to (63/64)*t~CK~.

|OP7    OP6 OP5 OP4 OP3 OP2 OP1	OP0|Encoding| 
|:-:|:-|
|x	x	0	0	0	0	0	0| Delay outputs by +(0/64)*t~CK~|
|x	x	0	0	0	0	0	1| Delay outputs by +(1/64)*t~CK~|
|x	x	0	0	0	0	1	0| Delay outputs by +(2/64)*t~CK~|
|x	x	.	.	.	.	.	.| ..............................|
|x	x	1	1	1	1	0	1| Delay outputs by +(61/64)*t~CK~|
|x	x	1	1	1	1	1	0| Delay outputs by +(62/64)*t~CK~|
|x	x	1	1	1	1	1	1| Delay outputs by +(63/64)*t~CK~|
|x	0	x	x	x	x	x	x| Reserved |
|x	1	x	x	x	x	x	x| Reserved |
|0	x	x	x	x	x	x	x| Default - Feature Disabled|
|1	x	x	x	x	x	x	x| Feature Enabled|






