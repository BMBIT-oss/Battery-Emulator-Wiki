---
title: "Ennoid/MaxkGo"
---

The prototype was built on MaxkGo HV, which is said to be a clone of the Ennoid BMS. The difference is that MaxkGo only has early firmware versions, and they are no longer being developed. The BMS settings should be made in the Ennoid Configuration Tool according to the battery packs and LTC6811-13 series balancing controllers you have. 

Communication with the Battery Emulator itself should be configured as follows: 

- CAN ID -> 10
- CAN ID Style -> ENNOID/DiebieMS/VESC
- CAN throughput -> 500kbit/s
- Load status protocol -> VESC
- Charge status protocol -> None.

The advantage of MaxkGo is that it can handle all cells even if we connect them into several parallel branches, e.g. 50s2p, 50s3p, etc. The theoretical maximum number of cells is 192. It was tested in an 18s10p and 90s2p configuration. Even though the branches are connected to each other only by the outermost '+' and '-', we have control and balancing of each cell individually (?). 

In my case, ten LTC6813 controllers are working, and all 18 cell inputs and 5 temperature sensor inputs per each LTC are used in them, so a total of 50 temperature measurement points, which provides very precise monitoring. I found a mention that the CAN parameters are the same as for the DEYE HV inverter, but I haven't tested whether it can run on a single interface. I am testing the system on a DEYE SUN-25K-SG01HP3-EU-AM2. Photos will be attached after the work is completed.

#PetrykCom

