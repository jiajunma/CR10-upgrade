# CR-10 Smart Upgrade Research Log

This document records the research findings accumulated during planning of the CR-10 Smart upgrade. It distinguishes confirmed facts from provisional conclusions and open questions.

## Machine identification

### Confirmed

- Printer model: **Creality CR-10 Smart**.
- Original mainboard marking: **`CRC-2405V1.1`**.
- The machine is a **24 V** platform.
- Current build surface: **PEI spring-steel sheet**.

### Important consequence

Older CR-10 / CR-10S upgrade guides cannot be assumed to apply mechanically or electrically to the CR-10 Smart. Any conversion guide must be checked against the CR-10 Smart specifically.

---

## Current upgrade direction

### Decided

- Firmware: **Klipper**.
- Toolhead/extruder-hotend: **BIQU H2 V2S**.
- Bed probe: **BIGTREETECH Eddy Duo**.
- Mainboard family: leaning toward a conventional external-host controller board rather than an SBC-carrier board.
- Mainboard currently favored: **BIGTREETECH Octopus Pro V1.1**, preferably **STM32H723** if the price premium over F446 is modest.
- Stepper-driver family: **TMC2209**.
- No plan to run 48 V motors.
- Original CR-10 Smart X/Y/Z motors are expected to be retained.
- No EBB36 is required for the first build.

### Planned independent motor channels

1. X
2. Y
3. Z-left
4. Z-right
5. H2 V2S extruder

This means **five stepper-driver channels** are sufficient for the present machine. An 8-channel board leaves three spare channels.

---

## Why TMC2209

### Confirmed design rationale

TMC2209 is a removable stepper-driver module used in boards such as Octopus Pro, FLY Super8 Pro and FYSETC Spider.

For this 24 V CR-10 Smart build it provides the useful features without unnecessary high-voltage capability:

- UART configuration from Klipper
- quiet operation / StealthChop support
- current control in software
- StallGuard / diagnostic features
- mature Klipper support
- low cost
- easy replacement if a driver fails

### Current plan

Use **TMC2209 x5**:

- X -> TMC2209
- Y -> TMC2209
- Z-left -> TMC2209
- Z-right -> TMC2209
- H2 V2S -> TMC2209

### Why not TMC5160

TMC5160 can be used at 24 V, but its main advantage is high-voltage / high-current operation, especially for 36/48 V high-speed XY systems. Since this CR-10 Smart build will remain 24 V, the additional cost and complexity provide little practical benefit.

---

## Mainboard research

### Manta M8P V2

The Manta M8P V2 is a strong board electrically, but it combines two roles:

1. printer MCU / I/O board;
2. SBC carrier for CB1/CB2/CM4-style compute modules.

For a build using an **external Linux PC or similar Klipper host**, much of the SBC-carrier functionality is unused. This makes M8P less attractive here than a conventional controller such as Octopus Pro, FLY Super8 Pro or FYSETC Spider.

### Octopus Pro

Advantages for this build:

- conventional external-host architecture;
- USB connection to host;
- 8 removable driver sockets;
- strong Klipper/Voron community documentation;
- CAN support available for future use;
- enough fan/heater/sensor I/O for a heavily modified CR-10 Smart.

Preferred version: **Octopus Pro V1.1**.

Preferred MCU: **H723** when cost difference is small; **F446 is still fully adequate** for the current printer.

### Other boards considered

- **FLY Super8 Pro H723**: very strong alternative, especially in China; 8 drivers, many fan/heater/ADC channels, CAN and good domestic support.
- **FYSETC Spider V3 H7**: strong 8-driver alternative, slimmer PCB and direct external-host architecture.
- **MKS Monster8 V2**: practical low-cost option; lower MCU performance is not a serious limitation for normal Klipper operation.
- **BTT Kraken**: unnecessary for this build because its integrated high-current/high-voltage drivers target much more aggressive 48/60 V systems.

---

## Octopus Pro F446 vs H723

See [octopus-pro-f446-vs-h723.md](octopus-pro-f446-vs-h723.md).

Summary:

- F446: STM32F446, Cortex-M4, 180 MHz.
- H723: STM32H723, Cortex-M7, 550 MHz.
- Both are more than capable of running this printer under Klipper.
- H723 mainly provides more processing and I/O timing headroom; it does **not** directly improve print quality.
- Prefer **V1.1 H723** if the price difference is modest.

---

## H2 V2S toolhead

### Decided

Use **BIQU H2 V2S** rather than Orbiter/Revo/Rapido or Smart Orbiter.

Reasons:

- compact integrated direct-drive extruder + hotend;
- suitable for a CR-10 Smart conversion;
- fewer separate mechanical interfaces;
- easier first conversion than a fully modular Voron-style toolhead;
- sufficient performance for a 24 V bed-slinger.

### Additional cooling

The H2's hotend heatsink fan is not the same as part cooling. A **24 V 5015 blower** is still required for part cooling. One 5015 is a sensible initial configuration; dual 5015 can be added later if needed.

---

## Eddy Duo

### Decided

Use **BIGTREETECH Eddy Duo**.

Why it fits this machine:

- PEI spring-steel build plate is well suited to eddy-current sensing;
- fast continuous bed scanning is attractive on a 300 x 300 mm bed;
- no mechanical probe pin;
- Duo supports both USB and CAN, preserving future flexibility.

### Initial connection strategy

For first bring-up, prefer the simpler topology:

- host USB -> mainboard
- host USB -> Eddy Duo

CAN can be explored later after the printer is stable.

### Safety note

Eddy Duo is a low-voltage device; do not feed it the printer's 24 V supply directly unless explicitly through a supported regulator/interface configuration from the official documentation.

---

## EBB36 / CAN toolboard decision

### Current decision

Do **not** require EBB36 for the first build.

Without EBB36, the Octopus Pro directly drives:

- H2 stepper motor
- hotend heater
- thermistor
- hotend heatsink fan
- part-cooling blower(s)
- other toolhead I/O as required

The tradeoff is a larger moving cable bundle. This is acceptable on a CR-10 Smart bed-slinger and avoids adding another MCU, CAN bootloader configuration and another fault domain during initial conversion.

EBB36 remains a future option if wiring cleanliness becomes important.

---

## Original motor compatibility

### Confirmed at the electrical-class level

The CR-10 Smart X/Y/Z motors are standard 2-phase 4-wire stepper motors and can be driven by TMC2209 modules on Octopus Pro/FLY/Spider-class boards.

No motor replacement or electrical conversion module is inherently required.

### Still to verify before claiming plug-and-play

The exact **four-pin connector pin order** of `CRC-2405V1.1` versus Octopus Pro has not yet been proven from two complete manufacturer schematics, because a complete public CRC-2405V1.1 schematic has not been located.

Do not record wire-color guesses as a fact.

The correct comparison should be based on board connector pin definitions / schematics only.

---

## CRC-2405V1.1 and SKR-CR6 relationship

A documented real-world CR-10 Smart conversion reports that the **BTT SKR-CR6 is completely pin-compatible with the CRC-2405V1.1 installation**, including mechanical alignment of screw holes, SD-card slot and USB opening.

This makes SKR-CR6 documentation useful as an indirect reference when analyzing CRC-2405V1.1 interfaces.

However, this is still an indirect evidence chain, not a substitute for a complete Creality schematic.

---

## Mainboard mechanical mounting

### Octopus Pro dimensions

- PCB: approximately **160 x 100 mm**.
- mounting-hole rectangle: approximately **150 x 90 mm**.

### CR-10 Smart conclusion

Octopus Pro should be treated as requiring a **mounting adapter plate** in the CR-10 Smart electronics bay rather than assuming direct screw-hole compatibility with `CRC-2405V1.1`.

Recommended approach:

- keep the original enclosure / electronics bay;
- use a flat ABS/ASA/PETG or aluminum adapter plate;
- use M3 standoffs between the adapter and Octopus Pro;
- ensure USB, SD, power terminals and driver cooling remain accessible.

---

## Architecture discussion: CR-10 Smart vs Voron/Switchwire

Voron concepts were investigated mainly to understand future reuse of electronics.

### CoreXY

CoreXY keeps the bed out of XY high-speed motion and moves the toolhead in both X and Y. Voron Trident and Voron 2.4 use CoreXY-style XY motion.

### Switchwire

Voron Switchwire uses CoreXZ, not CoreXY, and still moves the bed in Y. A CR-10 conversion to Switchwire is possible, but conversion files for older CR-10 models should **not** automatically be assumed compatible with the CR-10 Smart.

### Current conclusion

Do not perform a full Switchwire conversion as the first step. Modernize the CR-10 Smart electronics and toolhead first. Reusable components can later migrate to another DIY printer if desired.

---

## Open questions / next research tasks

1. Find the strongest available documentation for the `CRC-2405V1.1` stepper-output connector pin order.
2. Compare SKR-CR6 motor connector pin ordering with Octopus Pro motor connector pin ordering as an indirect but documented bridge.
3. Determine the exact Octopus Pro orientation and adapter geometry inside the CR-10 Smart electronics bay.
4. Locate or design a CR-10 Smart-specific H2 V2S carriage/mount.
5. Design/choose the H2 V2S 5015 duct and Eddy Duo mount.
6. Record exact original CR-10 Smart motor models and establish conservative Klipper `run_current` starting values.
7. Produce a complete wiring map before powering the converted machine.

---

## Research discipline

For this repository:

- label manufacturer-confirmed facts as confirmed;
- label community reports as community evidence;
- label deductions as deductions;
- do not convert uncertain pinouts or dimensions into definitive wiring instructions;
- preserve source links for every important hardware claim.
