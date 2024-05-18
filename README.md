# CGB-BTDT
Been There, Done That (Board test, Deceptive Thinker)

I don't think there's anything new on this board - just stuff other people have already done.
This board takes the work I did converting Bucket Mouse's MGBC to CGB form factor and iterates on it further to create something almost entirely new. Due to the components used and design choices I've made, it sets itself as a bit of a hybrid between Bucket Mouse's design and LeggoMyFroggo's design. However, all IC choices are my own, and all circuit design not taken from the MGBC schematic I've engineered myself based on datasheets (and math).

## What is changed from the MGBC?
Power delivery and indicator circuitry is entirely new. The 5v regulator, USB Charging circuit, LiPo connector, power indicators, power switch and supporting circuit are all designed from the ground-up (albeit taking heavy influence from LeggoMyFroggo's FBC board and IC choice). It supports simultaneous play and charge via USB-C, and does not support AA batteries.
The board is a 4-layer with a dedicated ground layer for one of the inner layers, and second inner layer primarily utilized for power delivery.
Fewer OEM components required: the only required OEM components are the CPU, RAM, and Clock Crystal. Every other component is new, or available aftermarket (Link Port, Volume Knob, Cartridge Slot, and FFC connector can be reused). IR port requires the original IR LED and Phototransistor but I'm working on sourcing a replacement.
Power switch, USB port and headphone jack are new parts chosen for this build.

## Power Switch and UVLO

The power switch is a momentary "spring return" power switch, electrically compatible with a DS Lite power switch. The power delivery no longer travels through the switch, instead the activation triggers the input of an ON/OFF controller IC - the output of which is connected to the enable pin of the primary power regulator. A 2.9V low voltage lockout chip runs from VCC to the clear line of the ON/OFF controller. When the voltage output from the charging/load sharing IC drops below 2.9v, this will shut the system off, and keep it off until the switch is triggered again. This ensures the lithium polymer battery does not drop to exceptionally low levels.

## USB Charging

## 5v Regulator

## Power Indicator

