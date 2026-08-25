# Sources and Design Documents

This page collects the primary and secondary sources used in the CR-10 Smart upgrade research.

## Creality CR-10 Smart / CRC-2405V1.1

### Community teardown and documented board replacement

Sebastiaan D. / Damsteen: CR-10 Smart motherboard replacement with BTT SKR-CR6

- https://damsteen.nl/blog/2021/04/25/creality-cr10-smartening-up-with-btt-skr-cr6-motherboard
- GitHub source of the article: https://github.com/Sebazzz/sebazzz.github.io/blob/master/_posts/2021-04-25-creality-cr10-smartening-up-with-btt-skr-cr6-motherboard.md

Important evidence from this source:

- identifies the original CR-10 Smart mainboard as `CRC-2405V1.1` in the documented machine;
- reports the BTT SKR-CR6 as completely pin-compatible with the CR-10 Smart board installation;
- reports matching screw holes, SD-card slot and USB opening.

USB/internal electronics article:

- https://damsteen.nl/blog/2021/04/24/connect-creality-cr-10-to-computer-or-octopi
- GitHub source: https://github.com/Sebazzz/sebazzz.github.io/blob/master/_posts/2021-04-24-connect-creality-cr-10-to-computer-or-octopi.md

### Missing primary document

A complete manufacturer-released `CRC-2405V1.1` schematic has **not yet been located**. This is an explicit gap in the evidence base.

---

## BIGTREETECH Octopus Pro

Official repository:

- https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-Pro

Official hardware directory:

- https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-Pro/tree/master/Hardware

Key design documents:

- Octopus Pro V1.1 schematic PDF:
  https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-Pro/blob/master/Hardware/BIGTREETECH%20Octopus%20Pro%20V1.1-sch.pdf
- Octopus Pro V1.1 pin diagram JPG:
  https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-Pro/blob/master/Hardware/BIGTREETECH%20Octopus%20Pro%20V1.1-Pin.jpg
- Octopus Pro pin PDF:
  https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-Pro/blob/master/Hardware/BIGTREETECH%20Octopus%20Pro%20-%20PIN.pdf
- Octopus Pro size PDF:
  https://github.com/bigtreetech/BIGTREETECH-OCTOPUS-Pro/blob/master/Hardware/BIGTREETECH%20Octopus%20Pro%20-%20SIZE.pdf

BTT documentation:

- https://global.bttwiki.com/Octopus%20Pro.html

---

## BIGTREETECH TMC2209

Official BTT TMC2209 repository / schematic:

- https://github.com/bigtreetech/TMC2209-V1.1
- https://github.com/bigtreetech/TMC2209-V1.1/blob/master/Schematic/TMC2209-V1.1.pdf

TMC2209 is used as a removable driver module in the current plan.

---

## BIQU H2 V2S

Product/documentation starting points:

- https://biqu.equipment/
- Search product documentation for `H2 V2S` on BIQU/BIGTREETECH official documentation before final wiring and Klipper configuration.

Current plan: 24 V H2 V2S complete toolhead, plus separate part-cooling blower.

---

## BIGTREETECH Eddy Duo

Official documentation starting points:

- https://github.com/bigtreetech/docs/blob/master/docs/Eddy.md
- https://neo.bttwiki.com/en/docs/accessories-docs/sensor/eddy/eddy-hardware

Current plan: Eddy Duo on PEI spring-steel bed; USB first, CAN optional later.

---

## Alternative mainboards researched

### FLY Super8 Pro

- https://docs.3dmellow.com/en/docs/ProductDoc/MainBoard/fly-super/fly-super8-pro/
- https://mellow.klipper.cn/en/docs/ProductDoc/MainBoard/fly-super/fly-super8-pro/

### FYSETC Spider H7

- https://docs.fysetc.com/
- https://wiki.fysetc.com/docs/SPIDERV3H7

### MKS Monster8

- https://github.com/makerbase-mks/mks-monster8

---

## Klipper

Official documentation:

- https://www.klipper3d.org/
- CAN bus: https://www.klipper3d.org/CANBUS.html
- TMC drivers: https://www.klipper3d.org/TMC_Drivers.html

---

## Source-preservation policy

Where the upstream project already hosts a design drawing or schematic in a public GitHub repository, this repository should prefer:

1. preserving the canonical upstream URL;
2. noting the exact filename/version used;
3. only mirroring binary assets when redistribution is clearly permitted and doing so adds real archival value.

This avoids accidentally detaching schematics from their license/version history.
