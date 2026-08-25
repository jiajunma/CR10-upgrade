# CR-10 Smart hidden USB / external computer connection

Source: https://damsteen.nl/blog/2021/04/24/connect-creality-cr-10-to-computer-or-octopi

## What the article confirms

The CR-10 Smart contains a Creality WiFi box, a power relay, a Creality-branded Meanwell LRS-350-24 PSU, the CRC-2405V1.1 motherboard, and a separate USB junction board.

The USB junction board is important because the printer's motherboard USB is not simply exposed on the chassis. The junction board provides 5 V power to the Creality WiFi box and switches/reroutes the USB data connection.

The article identifies three relevant USB-side connections on the junction board:

- a connection to the Creality WiFi box;
- a connection from the junction board to the motherboard's internal USB header;
- a separate PC-side header / micro-USB connector for an external computer.

To connect an external computer directly to the stock CR-10 Smart motherboard, the documented procedure is:

1. disconnect the cable leading to the motherboard from the junction board connector labelled `MCU`;
2. move that cable to the connector labelled `PC`;
3. connect an external USB cable to the junction board's micro-USB connector;
4. route that cable outside the electronics compartment.

The author also warns that this external USB cable has no proper strain relief in this arrangement.

## Relevance to the Octopus Pro upgrade

This article shows that the stock CR-10 Smart USB path is not a simple motherboard USB socket exposed to the outside. It is mediated by a separate junction board originally intended to work with the Creality WiFi box.

For an Octopus Pro conversion, the simplest architecture is therefore likely to bypass the stock USB junction logic and connect the external Klipper host directly to the Octopus Pro USB-C port.

This avoids depending on Creality's WiFi-box USB routing and makes the topology straightforward:

```text
External Klipper host
        |
       USB
        v
Octopus Pro USB-C
        |
        v
STM32 MCU
```

The stock USB junction board may still be physically useful for other purposes, but it is not required for Octopus Pro communication.

## Confirmed hardware identifiers from the article

- Mainboard: `CRC-2405V1.1`
- PSU: Creality-branded Meanwell `LRS-350-24`
- Separate Creality WiFi box board
- Separate USB junction board
- Separate power relay

## Caveat

The author explicitly states that his printer may have contained pre-release hardware, so later production units could differ. Our machine is confirmed to use the same `CRC-2405V1.1` motherboard, which makes the article especially relevant, but the exact USB junction-board revision should still be visually checked before relying on its connector labels.
