# CR-10 Smart CRC-2405V1.1 board layout

Source: user-provided CR-10 Smart `Circuit Wiring` board-layout figure for the CRC-2405V1.1 generation.

## What this figure is

This is a board-level interface/layout diagram, not a complete schematic. It is nevertheless useful for identifying the physical connector groups and the high-level system architecture of the CR-10 Smart mainboard.

## Interfaces visible in the figure

- Cable Port1
- Cable Port2
- Y axis motor / limit-switch combined connector
- Z1 axis motor connector
- Z2 axis motor connector
- Storage Card Slot
- WiFi Port
- Screen Interface
- Fuse
- Power Input
- Hot Bed Power Output
- Bed Thermistor
- Normal Fan
- Shutdown Module
- Z-axis photoelectric switch
- Reserved Port
- WiFi Reset

## Important architectural observations

1. The CR-10 Smart mainboard exposes separate Z1 and Z2 motor connectors at the board level. This is important when migrating to an Octopus-class controller: the two physical Z motors can be retained and placed on separate drivers if desired.
2. The Y-axis motor and Y-limit-switch signals are grouped into a single board connector in the Creality design. This is not directly equivalent to Octopus Pro, where motor and endstop connectors are separate. Therefore the original CR-10 Smart harness cannot be assumed to plug directly into Octopus Pro without either repinning, an adapter/breakout, or rewiring.
3. The two large `Cable Port1` and `Cable Port2` connectors are Creality-specific harness interfaces. They carry multiple functions and are a major reason that a board swap is not a simple one-for-one connector replacement.
4. The board has dedicated connectors for the shutdown module, WiFi, screen, bed thermistor, fan, and Z photoelectric switch. These functions need to be mapped individually when replacing CRC-2405V1.1 with Octopus Pro.
5. The board layout confirms that the stock CR-10 Smart electronics use a more integrated harness architecture than a conventional RepRap/Voron-style controller board.

## Comparison target: Octopus Pro V1.1

The Octopus Pro V1.1 motor outputs use the connector order shown in BTT's pinout as:

`A1 A2 B2 B1`

The Octopus Pro schematic identifies the two motor coils as `A1/A2` and `B1/B2`. The CR-10 Smart board-layout figure does not expose the individual phase labels for its motor connectors, so this figure alone cannot prove that the stock CR-10 Smart motor plugs have identical pin order.

## Practical consequence for the upgrade

The original X/Y/Z stepper motors are electrically compatible with Octopus Pro + TMC2209, but the stock CR-10 Smart harness should not be treated as a direct plug-and-play harness. The safest upgrade plan is either:

- use a documented breakout/adapter for the Creality harness, or
- rewire the motors, endstops, heaters, fans, and sensors to standard Octopus connectors.

For the planned CR-10 Smart upgrade (H2 V2S + Eddy Duo), this second approach is likely cleaner because the toolhead wiring is already being redesigned.
