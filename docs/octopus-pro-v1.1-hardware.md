# BIGTREETECH Octopus Pro V1.1 hardware notes

## Source material

This note records the user-provided **Octopus Pro V1.1 pinout image** and the **5-page schematic PDF** (`BIGTREETECH Octopus Pro V1.1-sch.pdf`).

Official upstream hardware repository:

- https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-Pro/tree/master/Hardware
- Schematic: https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-Pro/blob/master/Hardware/BIGTREETECH%20Octopus%20Pro%20V1.1-sch.pdf
- Pin map: https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-Pro/blob/master/Hardware/BIGTREETECH%20Octopus%20Pro%20V1.1-Pin.jpg

## MCU / board revision

The V1.1 schematic contains the H723 implementation. Page 5 identifies the MCU as **STM32H723ZET6**. The same schematic also keeps notes for older F446/F429 clock variants, but the H723 device is explicitly instantiated on the V1.1 sheet.

## Stepper-driver / motor-output topology

Page 1 of the schematic is the stepper-driver sheet. Each pluggable driver socket exposes the four motor outputs:

- `B1`
- `B2`
- `A2`
- `A1`

The schematic repeatedly labels each driver channel in that order, e.g. `DRIVER0_B1`, `DRIVER0_B2`, `DRIVER0_A2`, `DRIVER0_A1`.

The user-provided V1.1 pinout drawing shows the physical motor connector, viewed as drawn on the board, labelled:

```text
A1  A2  B2  B1
```

This is the same four electrical nets, simply read from the opposite physical end compared with the schematic net-list grouping.

### Important consequence

For a 4-wire bipolar stepper motor, the required pairing on Octopus Pro is:

```text
A1 -- A2   = one winding
B1 -- B2   = the other winding
```

Therefore the two centre pins in the pinout are `A2` and `B2`; **do not infer compatibility with a Creality harness merely from connector shape or wire colour**. The next comparison should use the SKR-CR6/CRC-2405V1.1 board-level motor connector definition and compare winding pairs pin-by-pin.

## Driver sockets

The board provides eight pluggable driver channels (`DRIVER0` ... `DRIVER7`). The schematic shows STEP, DIR, EN, DIAG, SPI/UART-related nets, VM and 5 V logic support at each socket. This is why modules such as TMC2209 can be replaced independently instead of being soldered permanently to the mainboard.

## Power domains

The schematic separates:

- `MOTOR_POWER` / `VM` for stepper drivers
- main `POWER` / `V_FUSED`
- `BED_POWER`

For the planned CR-10 Smart conversion, the machine remains a **24 V system**, so TMC2209 drivers should be operated from the normal 24 V motor supply; the high-voltage capability of Octopus Pro is not needed.

## Heater / fan outputs

The V1.1 board exposes four heater outputs (`HE0` ... `HE3`) and eight fan headers (`FAN0` ... `FAN7`). The fan-voltage selector circuitry allows fan rails to be selected from 5 V, 12 V or VIN, which is useful for a 24 V H2 V2S + 24 V 5015 setup.

## CAN / USB

The pinout exposes CAN-H/CAN-L and a selectable 120-ohm termination. USB-C D+/D- are connected to MCU PA12/PA11 in the schematic. This makes Octopus Pro suitable for a conventional external Klipper host connected over USB, with CAN available later if desired.

## Current decision for this project

Planned machine configuration:

- Creality CR-10 Smart, original board `CRC-2405V1.1`
- 24 V power system retained
- Octopus Pro V1.1, preferably H723
- TMC2209 x5
  - X
  - Y
  - Z-left
  - Z-right
  - BIQU H2 V2S extruder
- BIQU H2 V2S direct-drive toolhead
- BIGTREETECH Eddy Duo probe
- no EBB36 initially

## Open item

The remaining wiring question is whether the **original CR-10 Smart motor plugs can be moved directly to the Octopus Pro without repinning**. The correct way to settle this is to compare the CRC-2405V1.1-equivalent SKR-CR6 motor connector schematic/pinout with the Octopus Pro physical order `A1 A2 B2 B1`.