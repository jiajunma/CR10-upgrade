# CR-10 Smart (CRC-2405V1.1) → Octopus Pro V1.1 wiring map

This note deliberately compares the CR-10 Smart board layout directly with BIGTREETECH Octopus Pro V1.1. It does not rely on any intermediate controller board.

## Current target configuration

- Printer: Creality CR-10 Smart
- Original controller: CRC-2405V1.1
- New controller: BIGTREETECH Octopus Pro V1.1
- Stepper drivers: TMC2209
- Toolhead: BIQU H2 V2S
- Probe: BIGTREETECH Eddy Duo
- Supply: original CR-10 Smart 24 V supply retained

## Proposed driver assignment

| Function | Octopus Pro driver / motor port |
|---|---|
| X motor | MOTOR0 / DRIVER0 |
| Y motor | MOTOR1 / DRIVER1 |
| Z1 motor | MOTOR2 / DRIVER2 |
| Z2 motor | MOTOR3 / DRIVER3 |
| H2 V2S extruder motor | MOTOR4 / DRIVER4 |
| Spare | MOTOR5–MOTOR7 |

The exact driver numbers are a configuration choice; the key point is to use separate drivers for Z1 and Z2.

## Confirmed Octopus Pro motor-output convention

The Octopus Pro V1.1 pinout labels the 4-pin motor output as:

`A1  A2  B2  B1`

The official schematic shows each driver socket exposing the four phase outputs `A1`, `A2`, `B2`, `B1`. Therefore each motor connector contains two adjacent winding pairs: A1/A2 and B2/B1.

Do not infer CR-10 Smart wire order from wire colour alone. Verify the original harness connector pinout before moving a connector directly onto Octopus.

## Direct CR-10 Smart → Octopus Pro mapping

| CR-10 Smart board function | Octopus Pro destination | Status / notes |
|---|---|---|
| Power Input | POWER VIN/GND | Direct functional replacement. Use 24 V. |
| Z1 axis motor | MOTOR2 (suggested) | Direct stepper function; connector phase order must be checked. |
| Z2 axis motor | MOTOR3 (suggested) | Direct stepper function; connector phase order must be checked. |
| Y axis motor | MOTOR1 (suggested) | CR-10 Smart board groups Y motor/limit harness physically; Octopus separates motor and endstop, so breakout/retermination may be needed. |
| X axis motor | MOTOR0 (suggested) | X wiring is carried through the CR-10 Smart cable-port system; exact cable-port pins must be identified before reuse. |
| Original extruder motor | not reused | Replaced by H2 V2S; H2 motor goes to MOTOR4. |
| Hot Bed Power Output | Octopus BED-OUT, with BED-POWER input fed from 24 V PSU | High-current wiring must follow Octopus power architecture, not simply reuse connector location. |
| Bed Thermistor | TB | Octopus pinout provides dedicated bed thermistor input TB. |
| Normal Fan | FAN0 or another configured PWM fan | Function is configurable in Klipper. |
| H2 V2S hotend heater | HE0 | New direct wiring to Octopus is preferred. |
| H2 V2S thermistor | T0 / TH0 | New direct wiring to Octopus is preferred. |
| H2 V2S heatsink fan | FAN output | Choose a fan output and configure it in Klipper. |
| H2 part-cooling 5015 | FAN output | Choose a PWM fan output and configure it in Klipper. |
| Z axis photoelectric switch | Z_STOP input if retained as Z home switch | Whether to retain it depends on final Eddy Duo homing strategy. |
| Shutdown Module | potentially PS-ON, but **not yet electrically verified** | The label alone is insufficient to prove voltage/polarity compatibility. Do not connect until its actual electrical interface is known. |
| Screen Interface | generally not a direct drop-in connection | Creality screen protocol/connector is not assumed compatible with Octopus EXP/TFT headers. |
| WiFi Port | not needed in the planned Klipper architecture | External Linux host provides networking. |
| WiFi Reset | not needed | Original WiFi subsystem can be bypassed. |
| Reserved Port | unknown | Do not reuse without pinout. |
| Cable Port1 / Cable Port2 | no direct Octopus equivalent | These are Creality bundled harness interfaces. Signals must be broken out to individual Octopus motor/heater/fan/thermistor/endstop connections if the harness is retained. |

## Eddy Duo

Recommended first-stage topology:

```text
Linux/Klipper host
├── USB → Octopus Pro
└── USB → Eddy Duo
```

This avoids tying the probe to an ordinary probe input. Eddy Duo can later be moved to CAN if desired.

## Recommended rewiring strategy

For the H2 V2S conversion, the cleanest approach is to bypass much of the original Creality toolhead harness and run known individual circuits from Octopus to the new toolhead:

- H2 motor: 4 wires
- heater: 2 wires
- thermistor: 2 wires
- heatsink fan: 2 wires
- part cooling fan: 2 wires
- optional filament sensor/cutter/etc. separately

This removes ambiguity in Cable Port1/2 and makes the Octopus wiring conventional and serviceable.

## Items still requiring exact pinout confirmation

1. CRC-2405V1.1 Cable Port1 pinout
2. CRC-2405V1.1 Cable Port2 pinout
3. Y-axis combined motor/limit harness pin allocation
4. Shutdown Module voltage, polarity, and signal type
5. Original X motor/endstop/toolhead signals carried through the cable-port system
6. Exact 4-pin phase order of the original X/Y/Z motor harnesses, if those connectors are to be transferred without re-pinning

These should be resolved from a Creality wiring diagram, connector pinout, board continuity test, or harness breakout documentation—not from wire colour guesses.
