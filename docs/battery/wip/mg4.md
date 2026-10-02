---
title: "MG4"
---

!!! warning "Work in progress"
    This battery is not yet supported by Battery-Emulator, it can't be selected in the Settings. This page collects what is known so far for a future integration, so it may be incomplete or untested. Can you help? See [data needed for a new battery integration](../../setup/contributing/data_needed_for_new_battery_integration.md).

## Specifications

| Year |  Model | Battery capacity | Compatible? | Rated Voltage | Cells| Voltage Range | 
| :--------: | :---------: | :---------: | :----------: | :----------: |  :----------: | :----------:|
| 2024- | MG4 EH32 | 49kWh LFP       |   ✅ | 315V | 100| 250-365V     | 
| 2022- | MG4 EH32 | 51kWh LFP       |   ✅ | 327V | 104| 260-379.6V   |
| 2022- | MG4 EH32 | 64kWh NMC       |   ✅ | 380V | 104|291.2-452.4V |
| 2022- | MG4 EH32 | 77kWh NMC       |   ✅ | 380V | 108| 302.4-469.8V |
| 2026- | MG4 EH?? | 64kWh LFP       |   TBC | 316V | 100| 250-365V          |

## Current status

All packs, except the new 64kWh LFP pack, have now been tested to close contactors when requested, report SoC, SoH, cell voltages and temps and other necessary information, contactor control is also available.  Packs capacity, chemistry and cell counts are autodetected. 

SoC snaps to 100% on LFP packs when the max cell reaches ~3.75v. It is not known whether the packs are balancing, but the LFP packs seem to be 3-5mv accept at the extremes.

The MG4 code has the option to "Use estimated SOC" which will snap to 100% and coulomb count backwards from there, for packs with significant SoC drift the BE SoC is persistent across reboots, but not power cycles.

## Software configuration

Not yet merged, latest builds in:

https://github.com/jonny5532/Battery-Emulator/tree/feature/mg4-working-11i

For this battery type, use the option called "MG4 battery" under the "Battery config" setting.

![be](../../images/mg4-01.jpg)

## Connectors

The MG4 battery has an HV connector (Orange), and a 12 pin Low Voltage signal connector (Black/Red). There are also two coolant ports that can be used for thermal management, left is inlet, right is outlet (optional)

## Low voltage connector
(This is showing the cable viewed end on, not the battery socket)

![ESS_connector_pinout](../../images/mg4-02.jpg)

## Low voltage socket

![39d3687d-7d61-4841-a597-aa59d4bf7a2a](../../images/mg4-03.jpg)

![MG4_LV](../../images/mg4-09.png){ width="545" height="382" }

This is the Low voltage connector plug: [aliexpress](https://www.aliexpress.com/item/1005004677986133.html)

The one you need is the female.

There are non-wired versions available too for doing your own crimping but this looks easier to implement.

![alilvplug](../../images/mg4-10.png){ width="344" height="348" }

Lots of useful information here: [MG4 ESS SM.pdf](https://github.com/user-attachments/files/25114213/MG4.ESS.SM.pdf)

### Power connections

The battery needs a 12V-14V supply to pins 1 & 3, and draws 600mA continuous with the contactors closed, and ~3A briefly when closing the contactors. 

To reduce potential issues with the isolation measurement, it is preferable to have a 12V supply that is isolated from the grid (eg, powered from a 2-pin double-insulated adapter).

### CAN connections

The battery has three CAN buses:

**CAN PT** is the powertrain bus, it is an FD interface which requires a 500kbit CAN 2Mb CANFD connection.  Contactor control, battery statistics and UDS requests (0x7e5 & 0x7DF) all work via this bus.

**CAN PT EXT** is the powertrain extension bus, a regular CAN interface at 500kbit, but is not used for BE.

**CAN BMS** is the BMS bus, a regular CAN interface at 250kbit, but is not used for BE.

## High Voltage Interlock (HVIL)

The HVIL connections don't seem to be an issue.

## Physical Size

The 51 & 64kWh packs are 1880mm x 1440mm x 110mm and ~400kg, the 77kWh are 1880mm x 1440mm x 125mm and 450kg (including the mounting rails).  At the top of the pack is an Energy Distribution Module (EDM) which mounts the contactors, High and Low Voltage connections and the Battery Management Unit (BMU). If getting from a wrecker/breaker try to get the Power Distribution Box (PDU) as it has a number of useful connectors that can be reused.

![image](../../images/mg4-04.webp)

![PXL_20251119_080901770 MP](../../images/mg4-05.webp)

![565167202_24727149553573186_4603669578284620967_n](../../images/mg4-06.jpg)

![goes-here](../../images/mg4-07.jpg)

You will also need to use an isolated 12V power supply (a 2-pin power supply with no earth pin) to power the BMS as well as the Battery Emulator hardware (unless you are using isolated CAN), to ensure that there is no current path between the BMS case and the inverter's ground.
