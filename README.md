# CGB-BTDT
Been There, Done That (Board test, Deceptive Thinker)
<p><img src="/images/front_built.jpg" width="425" /><img src="/images/rear_built.jpg" width="425" /></p>

What started with a simple thought: "I don't think there's anything new on this board - just stuff other people have already done", has become somewhat of an understatement in the end.

This board takes the work I did [converting Bucket Mouse's MGBC to CGB form factor](https://github.com/ConsolesandCasks/Game-Boy-Pocket-Color) and iterates on it further to create something almost entirely new. 
Due to the components used and design choices I've made, it sets itself as a bit of a hybrid between Bucket Mouse's design and LeggoMyFroggo's FBC/TBC design. However, with those caveats, all IC choices are my own, and all circuit design not taken from the MGBC schematic or OEM GBC I've engineered myself based on datasheets, my own math, and testing.

This is a high skill level build with multiple no-lead package ICs that require the use of hot air, hot plate, or a reflow oven. Additionally there are some other tight quarter components. Nearly everything, however, is on one side of the board to assist with hot-plate assembly.

## What is changed from the MGBC (and my GBC form factor variant)?
Power delivery and indicator circuitry is entirely new. The 5v regulator, USB Charging circuit, LiPo connector, power indicators, power switch and supporting circuit are all designed from the ground-up, even though much of my IC choice was influenced by the aformentioned creators' decisions. 
<br/>This board supports simultaneous play and charge via USB-C, and does not support AA batteries. The regulator components are onboard and not a seperate PCB.
<br/>The board is a 4-layer with a dedicated ground layer for one of the inner layers, and second inner layer primarily utilized for power delivery.
<br/>Fewer OEM components required: the only required OEM components are the CPU, RAM, and Clock Crystal. Every other component is new, or available aftermarket (Link Port, Volume Knob, Cartridge Slot, IR LED/phototransistor, and FFC connector can be reused). I've even found a suitable replacement IR LED and photo-transistor that works with the original schematic.
Power switch, USB port and headphone jack are new parts chosen for this build.

## Technical Details
### PCB Images
<p><img src="/images/front_pcb.jpg" width="425" /><img src="/images/rear_pcb.jpg" width="425" /></p>

### Power Switch and UVLO

The power switch is a momentary "spring return" power switch, electrically compatible with a DS Lite power switch. My work with this circuit became the basis for my [SwiftSwitch](https://github.com/ConsolesandCasks/SwiftSwitch-GBC) mod, which required some minor component changes due to the requirements of the stock GBC power delivery circuitry.
<br/>It uses a "replacement" Nintendo DS lite switch component. 
<br/>My footprint is based on the Diptronics SSS-12RG-V-T/R switch (available on Mouser). The C&K JSM08022SAQNR (available on Digikey) and XKB Connection SK-1391L-2 (available on LCSC) are dimensionally identical, but do not have the grounding tabs. Most of the generic replacement switches on Aliexpress have these tabs.

The power delivery no longer travels through the switch, instead the activation triggers the input of an ON/OFF controller IC - the output of which is connected to the enable pin of the primary power regulator. A 3V low voltage lockout chip (TPS3840) runs from VCC to the clear line of the ON/OFF controller. There is a lowpass filter on the input of the TPS3840 chip to handle voltage transients that happen on initial powerup. When the voltage output from the charging/load sharing IC drops below 3v, this will shut the system off, and keep it off until the switch is triggered again. This ensures the lithium polymer battery does not drop to exceptionally low levels. I still recommend using a battery with an internal DW01 protection IC as extra security to prevent over-discharge.
<br/>The pushbutton IC also controls a mosfet connected to a test pad that I've added specifically to power the Hispeedido Q10 screen (there is also a pad that can be used to connect the screen to the 3.3v output instead, but I have not yet tested the reliability or efficiency of using one of these methods over the other)

### USB Charging and Load Sharing

USB Charging and Battery Management are controlled by the TI BQ24072 IC. This chip has a Dynamic Power-Path Management feature to allow safe simultaneous play and charge. Charge rate is capped at 500mA per USB standards. 
<br/>The battery positive rail is only connected to this IC and the battery life indicator LED circuit, through a fuse. 
<br/>The USB port component I've chosen has a higher off-board rise compared to most of those available. The height on this part aligns perfectly the cutout on the FPGBC shell, making those shells ideal to use with this board.

### 5V Regulator

The switching regulator in this circuit is a TPS63802. This is a buck-boost regulator with high efficiency and low noise. It supports as low as 1.3v input voltage, and as high as 5.5v - both far beyond the expected range of the battery input. This regulator has the added benefit of a relatively low required external component count with small size. I've been on the hunt for the perfect GBC regulator IC and this one is my favorite so far. The addition of an open-drain power-good output removes the need for an external power discharge mosfet. My design utilizes this pin to sink 0.5mA during power-down to speed up the discharge of the power circuitry upon turning the system off and to prevent odd reset cases. The theory behind this is outlined [here](https://www.ti.com/lit/an/slvafk9/slvafk9.pdf) (using a different boost converter IC with a PG pin that has a higher current sink capacity)
<br/>There is a jumper to swap this IC between power-save mode (jump right and middle) and PWM-only mode (jump left and middle). You might find a reduction in audible noise with power save mode disabled.

### Power Indicator

The power indicator is a 2-stage indicator circuit using an analog comparator. The first stage is lit when the device is powered on, and the battery is above nominal level. Once the battery drops below the nominal level, this LED will shut off, and the low battery LED will turn on. Additionally, there is a third LED indicator for battery charging.

### Amp and Headphone Jack

The Amp circuit is identical to the MGBC amp circuit. 
<br/>I've used a new headphone jack component that is electrically compatible with the OEM headphone jack instead of the OEM component. The footprint is different, and the pins are in different locations, even though it has the same functional pins.

### IR Circuit

The IR circuit is identical to OEM and the OEM parts can be reused. I've also tested an available off-the-shelf IR LED and IR photodiode that work as drop-in replacements. I have not yet tested the overall reliability of those components, only that they function. It is possible that some adjustments to the other passive components in the circuit would improve performance. 

## BOM
Preliminary BOM for current revision is [here](https://github.com/ConsolesandCasks/CGB-BTDT/blob/main/BTDT_BOM_r5v1.xlsx)

## Production Files
To Be Added - 11/24

## Credits
[Bucket Mouse](https://github.com/MouseBiteLabs) - base circuit design that I've massacred: [MGBC-MBL](https://github.com/MouseBiteLabs/Game-Boy-Pocket-Color)
<br/>[LeggoMyFroggo](https://github.com/leggomyfroggo/) - inspiration towards some component selection and circuit design: [FBC](https://github.com/leggomyfroggo/FBC)
<br/>[Zekfoo](https://github.com/Zekfoo/) - doing basically the same thing, years ago, before I did, probably better: [CGZ](https://github.com/Zekfoo/CGZ) (been there, done that)
<br/>[Modded Gameboy Club](https://moddedgameboy.club/) - providing whatever support, advice, and other input
<br/>[b23n](https://b-2-3-n.mmm.page/) - shell polishing [technique](https://mmm.page/b-2-3-n/mod-polishing) 
