
What is Loopback?
Loopback is a feature present on DDR5, which allows DRAM device to feed a received signal/data back out to external receiver. This allows to monitor the signal/data that was just sent to DRAM without having to store the data in the DRAM or use read operations to retreive data which was sent to DRAM device.

Why is Loopback needed?

How does Loopback work?


|  Function  |      Mode Register      |   Operand    |      Data      | 
| :--------: |:-------------:| :---------:| :---------| :--------: |
| Loopback Output Select | MR53 | OP[4:0] | 00000B: Loopback Disabled (Default) <br> 00001B: Loopback DML (X8 and X16 only)  <br> 00010B: Loopback DMU (X16 only)  <br> 00011B: Vendor Specific <br> 00100B: Vendor Specific <br> 00101B: RFU <br> ...thru <br> 01111B: RFU <br> 10000B: Loopback DQL0 <br> 10001B: Loopback DQL1 <br> 10010B: Loopback DQL2 <br> 10011B: Loopback DQL3 <br> 10100B: Loopback DQL4 (X8 and X16 only) <br> 10101B: Loopback DQL5 (X8 and X16 only) <br> 10110B: Loopback DQL6 (X8 and X16 only) <br> 10111B: Loopback DQL7 (X8 and X16 only) <br> 11000B: Loopback DQU0 (X16 only) <br> 11001B: Loopback DQU1 (X16 only) <br> 11010B: Loopback DQU2 (X16 only) <br> 11011B: Loopback DQU3 (X16 only) <br> 11100B: Loopback DQU4 (X16 only) <br> 11101B: Loopback DQU5 (X16 only) <br> 11110B: Loopback DQU6 (X16 only) <br> 11111B: Loopback DQU7 (X16 only) |
| Loopback Select Phase | MR53 | OP[6:5] | 00B: Loopback Select Phase A <br> 01B: Loopback Select Phase B (4-way and 2-way interleave only) <br> 10B: Loopback Select Phase C (4-way interleave only) <br> 11B: Loopback Select Phase D (4-way interleave only) |
| Loopback Output Mode | MR53 | OP[7] | 0B: Normal Output (Default) <br> 1B: Write Burst Output |

* NOTE 1 When Loopback is disabled, both LBDQS and LBDQ pins are either at HiZ or Termination Mode per MR36:OP[2:0]. Loopback Termination default value is 48 ohms
* NOTE 2 When Loopback is enabled, both LBDQS and LBDQ pins are in driver mode using default RON of 34 ohms
* NOTE 3 Phase A through D selects which bit in the multiplexer is being selected for Loopback output
* NOTE 4 This configures the DRAM Loopback output to either send data out every time the DQS toggles in Normal Output Mode, or to only send data out when enabled by the Write command, so that only write burst data is send out via Loopback.
* NOTE 5 The DM function shall be enabled (MR5:OP[5]=1) when loopback output from the DML or DMU pin is selected for Loopback measurement (MR53:OP[4:0]=00001B or 00010B).