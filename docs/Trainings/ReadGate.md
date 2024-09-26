Read Gate Training or Read Preamble Training

Read Gate/Preamble training is used to train the DQS receivers for Read operation. 



The read DQS gate training is used by the PHY to identify the valid interval of read DQS and capture the read data. It is necessary to align the valid read window to the read data burst and exclude the preamble period and any period during which the DQS signal is tri-stated or driven by the PHY itself.