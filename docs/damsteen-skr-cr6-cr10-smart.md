# Damsteen CR-10 Smart -> BTT SKR CR-6 article: extracted findings

Source:

- https://damsteen.nl/blog/2021/04/25/creality-cr10-smartening-up-with-btt-skr-cr6-motherboard
- Published: 2021-04-25

This article is unusually useful for the present project because it documents a real CR-10 Smart motherboard swap and explicitly compares the stock `CRC-2405V1.1` board with the BTT SKR CR-6.

## High-confidence findings from the article

### 1. SKR CR-6 is reported as completely pin-compatible with CR-10 Smart

The author states that the CR-10 Smart cable connections looked very similar to the CR-6 SE/MAX and checked the SKR CR-6 pin diagram. He concluded that the pinout matched and writes that the **BTT SKR CR-6 motherboard is completely pin-compatible with the CR-10 Smart**.

This is important because it gives us a documented bridge:

```text
CRC-2405V1.1 / CR-10 Smart wiring
            <->
       BTT SKR CR-6
```

This does not by itself prove that every connector can be moved unchanged to an Octopus Pro, but it makes the SKR CR-6 pinout a useful reference for reconstructing the CR-10 Smart interface definitions.

### 2. Mechanical alignment is also reported to match

When the stock board and SKR CR-6 are placed side-by-side, the author reports:

- screw holes line up perfectly;
- SD-card slot lines up perfectly;
- USB connector lines up perfectly.

This strongly supports using SKR CR-6 mechanical drawings as a proxy for the mounting geometry of the stock `CRC-2405V1.1` board.

### 3. Stock CR-10 Smart stepper-driver arrangement is mixed

The article reports that:

- the extruder and Z-axis use older-style Allegro non-silent drivers;
- the stock motherboard is presumed to have TMC2208 drivers for the other axes;
- the stock drivers run in standalone mode;
- SKR CR-6 uses TMC2209 drivers with firmware control.

This explains why the original CR-10 Smart can have quiet X/Y motion but noisy extruder/Z behavior.

For the planned Octopus Pro conversion, this entire integrated-driver arrangement disappears and all five required channels can instead use removable TMC2209 modules under Klipper control.

### 4. CR-10 Smart has a special power relay / self-power-off circuit

The CR-10 Smart has a relay that allows firmware-controlled power-off.

The article establishes an important electrical detail:

- the relay needs **24 V** to remain engaged after startup;
- 3.3 V and 5 V are not sufficient;
- the side power button initially engages the machine;
- firmware can later release the relay and power the printer down.

The author describes the yellow wire as the control wire and notes that the relay-board logic includes an inversion stage.

This is a major point for the Octopus Pro conversion: the CR-10 Smart is not just a generic 24 V printer with a simple PSU switch. If we want to preserve the original push-button / auto-shutdown behavior, the relay circuit must be handled deliberately.

### 5. During firmware flashing, relay behavior can matter

When the relay was powered from a firmware-controlled fan port, the board could lose relay power while the bootloader was flashing. The author therefore had to keep the power button held during flashing in that configuration.

For our Klipper/Octopus design, we should avoid placing the CR-10 Smart relay-hold circuit on an output that may be inactive during MCU reset/bootloader states unless we explicitly design around that behavior.

### 6. The original CR-10 Smart contains an internal USB path / junction board

The article references the internal electronics arrangement:

- connection to the Creality Wi-Fi box;
- connection from a junction board to the motherboard's internal USB header;
- an additional header / micro-USB path on the stock electronics.

The SKR CR-6 does not have the same internal USB header, so the author had to expose its USB connector mechanically.

For an Octopus Pro conversion this reinforces the decision to treat USB-host access as a separate mechanical requirement rather than expecting the original internal USB routing to map directly.

### 7. Power-input and heated-bed wire ends are tinned from the factory

The author specifically recommends removing the tinned wire ends and using bare copper or, preferably, ferrules.

This is relevant for the Octopus Pro conversion because the high-current power and heater wires will be re-terminated anyway. The new build should use correctly crimped ferrules for screw terminals.

### 8. CR-10 Smart dimensions used in firmware

The article changes the CR-6 configuration to:

- X bed size: 300 mm
- Y bed size: 300 mm
- Z max: 400 mm
- X min: -5 mm
- Y min: -2 mm

These values are useful as historical reference only; the final Klipper motion limits should be measured again after installing the H2 V2S and Eddy Duo because toolhead geometry and offsets will change.

## Implications for the Octopus Pro conversion

### What this article confirms strongly

- The documented stock CR-10 Smart mainboard is `CRC-2405V1.1`.
- SKR CR-6 can replace it with matching printer-side connectors.
- SKR CR-6 mounting / SD / USB geometry matches the stock-board installation.
- The CR-10 Smart power relay requires special attention and operates from 24 V.
- The original printer contains nontrivial internal USB / Wi-Fi wiring.

### What it does NOT prove

It does **not** prove that a CR-10 Smart motor connector can be plugged into an Octopus Pro unchanged merely because both are four-pin connectors.

To prove that, we still need to compare:

1. SKR CR-6 motor-output connector pin order (using its official schematic/pin map);
2. Octopus Pro motor-output connector pin order;
3. use the documented CR-10 Smart <-> SKR CR-6 pin compatibility as the bridge.

This remains an explicit research task.

## Design recommendation derived from this article

For the planned build:

```text
CR-10 Smart
  stock CRC-2405V1.1 removed
       |
       +--> Octopus Pro V1.1
       |      +--> TMC2209 x5
       |      +--> original X/Y/Z motors
       |      +--> H2 V2S
       |      +--> heater / fans / thermistors
       |
       +--> Eddy Duo (initially USB to host)
```

The original CR-10 Smart relay circuit should be documented separately before final power wiring is committed.

## Confidence labels

- **Confirmed by article / real machine:** SKR CR-6 pin compatibility, mechanical alignment, relay behavior, stock wiring observations.
- **Author inference:** exact stock driver identities where the article itself says "presumably".
- **Still open:** exact CRC-2405V1.1 motor connector phase ordering relative to Octopus Pro.
