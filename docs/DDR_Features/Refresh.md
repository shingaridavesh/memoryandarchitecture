
|  Types  |      DDR1      |   DDR2    |      DDR3      |      DDR4      |   DDR5    |
| :--------: |:-------------| :---------| :---------| :-------- | :-------------| 
| Refresh Command | <ul><li>Auto Refresh</li></ul> | <ul><li>Refresh</li></ul> | <ul><li>Refresh</li></ul> |  |<ul><li>Refresh All (REF~ab~)</li><li>Refresh Management All (RFM~ab~)</li><li>Refresh Same Bank (REF~sb~)</li><li>Refresh Management Same Bank (RFM~sb~)</li></ul>|  |
| Refresh Features |  |  |  |  | <ul><li>Refresh Modes - Normal and Fine Granularity</li><li>Refresh Management</li><li>Directed Refresh Management (DRFM) - Optional</li></ul>| 

Refresh Management (RFM)

DDR5

MR58

|  Function  |     Register Type      |   Operand    |      Data      |
| :--------: |:-------------:| :---------:| :---------|
| RFM Required | R | OP[0] | 0B: Refresh Management not required <br> 1B: Refresh Management required|
| Rolling Accumulated ACT <br> Initial Management Threshold (RAAIMT) | R | OP[4:1] | 0000B- 0011B: RFU <br> 0100B: 32 (Normal), 16 (FGR) <br> 0101B: 40 (Normal), 20 (FGR) <br> ... <br> 1001B: 72 (Normal), 36 (FGR) <br> 1010B: 80 (Normal), 40 (FGR) <br> 1011B-1111B: RFU|
| Rolling Accumulated ACT  <br> Maximum Management Threshold (RAAMMT) | R | OP[7:5] | 000B-010B: RFU <br> 011B: 3x (Normal), 6x (FGR) <br> 100B: 4x (Normal), 8x (FGR) <br> 101B: 5x (Normal), 10x (FGR) <br> 110B: 6x (Normal), 12x (FGR) <br> 111B: RFU |

