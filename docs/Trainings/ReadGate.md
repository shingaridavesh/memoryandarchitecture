Read Gate Training or Read Preamble Training

Read Gate/Preamble training is used to train the DQS receivers for Read operation. First we need to understand why we need to train it and how it is trained. 

##Need of Training

Let us look at a system we have a MC+PHY connected to a DIMM. Overe here we see that PHY has multiple DQS Receivers connected to DQS signal coming from DIMM. Now because of Fly-by routing, all the DIMMs will receive the command at slight skew from each other. Let us consider we are doing a Read command. DDR chip closest to RCD i.e. ECC chip will receive the Read command 1st followed by adjacent chip i.e. DQ Byte 3 chip and so on. Since CL/RL is same for all chips, chip which received the command first will also start transmitting data earlier than other chip, and farthest chip (DQ Byte 0) will transmit data at the very last. Now considering that trace length for DQS/DQ signals is same for all data bytes, these signals will also reach at different time on PHY end. So we will need to enable the DQS receivers at the right to capture the data.

![](../images/rdgate/rdgateneed2.drawio)

Now the next question is that what if we dont enable the receivers at correct time?
Let us comsider this case. Let us say that we use the same RxEN delay for all receivers. Also lets say that we are reading data all 0. COnsidering DDR4/DDR5 which uses POD, DQ signal will be by default High or 1. So if the receivers turn on at correct timing, we should see b'00000000. But we receiver turn on early then we will see b'11000000, and if receivers are turned on late, then we will see b'00000011. So it is important that read DQS is captured at correct time, so that we read correct valid data.

![](../images/rdgate/rdgateneed.drawio)


The read DQS gate training is used by the PHY to identify the valid interval of read DQS and capture the read data. It is necessary to align the valid read window to the read data burst and exclude the preamble period and any period during which the DQS signal is tri-stated or driven by the PHY itself.