## Overview:
This project is an open-source recreation of the Team Xecuter Demon, a powerful multi-function NAND device for the Xbox 360. Our goal is to rebuild the firmware, fix existing bugs, and potentially develop new hardware add-ons. We hope this project encourages wider community involvement.

## Demon background:
Released in February 2012 alongside a J-Runner update, the Demon offered an all-in-one solution for NAND programming. While initially successful, it was perhaps over-hyped and ultimately didn't achieve the same market penetration as other Team Xecuter devices. The rise of stealth services further diminished its relevance, and the lack of promised add-on hardware hampered its potential. Numerous planned add-ons never saw a public release due to Team Xecuter's internal challenges. Following financial and legal difficulties, ownership transitioned from K3 to Max.

The Demon provides access to the console's UART for direct interaction, along with extensive onboard I/O capable of supporting a separate 8-bit bus. Additional I/O pins are available for bit-banging SPI, I2C, or other protocols. Although Team Xecuter announced Android and iOS remote control apps in July 2012, these were never released. Similarly, LCD support was explored, with prototypes circulated among developers, but only the NAND switching plugin, Switch!, was officially launched.

## Project Background:
Inspired by discussions with fellow enthusiasts and analysis of the Xbox 360 ODE cracking process, this project began with recreating the Demon's schematics. Access to images of a pre-release slim PCB aided in verification.

Understanding the hardware, components, and design is crucial for reverse engineering. One undocumented aspect is controlling the Demon's hardware and I/O from the console. We aim to document the I/O addressing scheme (if implemented) and investigate the potential for a UART control API. A PC-based USB control API already exists. Third-party add-ons could leverage the Demon's I/O, potentially enabling features like an integrated LCD display.

## Theory of operation:
The Demon's NAND interface largely mirrors the console's onboard NAND, using a tri-state bus transceiver to isolate the microcontroller from the NAND control logic. Control appears to rely on the CE and RB signals, with other control lines interrupted by the transceiver. Logic in U3 prevents simultaneous driving of both CE pins. C3 likely introduces timing delays for CE switching to the Demon's NAND via NOR gates in U4. This, combined with RB, likely determines chip readiness before OE activates, hijacking CE to enable the Demon's NAND. OE also enables the tri-state transceiver and Demon's NAND CE logic when the motherboard's CE is high.

The design seemingly intercepts NAND control between read/write cycles, asserting and locking control while the chip is busy. Analyzing microcontroller-to-NAND communication is necessary for confirmation, but this theory appears plausible.

## Challenges:
The AT90USB microcontroller's bootloader employs custom encryption (believed to be RSA-2048), posing significant challenges. Existing Team Xecuter firmware is unanalyzable, current devices require a bootloader update for compatibility, and a new bootloader would break existing firmware support. Developing new firmware for the current bootloader is blocked by signature checks.

Project advancement hinges on recovering the bootloader or its RSA key. Collaboration with experienced individuals is sought. Donations are welcome to support this effort. While various options have been considered, we remain open to suggestions. Contact is being attempted with the original Team Xecuter developer. Interest exists in manufacturing the device if the bootloader is recovered. Due to on-the-fly firmware unpacking, direct firmware analysis is infeasible. A brown-out reset glitch or physical access is likely required for bootloader recovery. Cracking the RSA key is computationally infeasible.

For this project to go forward I would like to recover the bootloader or at least the RSA key by attacking the microcontroller if any one has experience with this or would be willing to try this it would be awesome if you would contact me.
I don't fancy dropping $1400usd to a chineese firm to do this I would rather give this to a fellow Xbox or scene hacker as this is beyond my skill and a very special area.
I already have a few buddies who would be willing to donate to the cause, So far I have resisted going down the route of croud sourcing the project as realistically I don't think niche projects like this work well on that platform and i'd like to aviod the disapointment for everyone when it fails.
You are welcome to donate to the cause if you wish id love a failed demon hardware so i can fully recreate PCB's.

I'd like to keep it all open source if possible, or it close it for a time to make hardware to recoup costs if we were to fund bootloader recovery there is a whole lot of options I have not explored here and I'm open to suggestions.

I have atempted to contact the original Team Xecuter dev involved with the design and programming of the demon project but have had no luck in getting a reply sadly so it is what it is.

I already have people intrested in manufacturing the device if we do manage to progress futher and recover the bootloader there is intrest to recreate them.

The demon seems to unpack the firmware on the fly and write it to the onbord flash so there is no attack vector there and we would need to preform brown out reset glitch or physical access to recover the bootloader for reverse engineering to get the RSA key.

RSA is currently secure and would take a supercomputer decades to crack the firmware update key embeded within the bootloader.


## Project Outline/ Future Plans:
The future plan is to recrate the 
The primary goal is an open-source Demon recreation, initially compatible with existing hardware. A hardware revision is likely due to component discontinuation. A more ambitious objective involves a new FPGA-based device with a microcontroller. This would enable on-the-fly NAND image creation, kernel patching, and a console configuration app. A shared memory window could facilitate console-microcontroller communication for RAM offloading, code relocation, and coprocessing.

Eaton's testing indicates a NAND bus speed of approximately 1.4 Mbps, suitable for low-bandwidth operations. The impact of using faster media is unknown. This advanced design requires skilled FPGA/CPLD developers. More information is available on 

Eaton's blog:
[eaton works](https://eaton-works.com/2023/01/09/how-microsoft-attempted-to-make-the-xbox-360-dashboard-load-faster/)

The schematics were designed using Eagle:
[Autodesk Eagle]( https://www.autodesk.com/products/eagle/free-download)

## Licensing
This project is open-source. Hardware and schematics are licensed under the [CERN OHL v.2.0.](https://ohwr.org/cernohl).

Donations are welcome to support XBMC4XBOX, Team Resurgent, or other Xbox resources.

[Teamresurgent donate]( https://www.patreon.com/teamresurgent)
[xbmc4xbox donate](https://www.xbmc4xbox.org.uk/donate/)

## Disclaimer
This project is a work in progress and may contain inaccuracies. Feedback and corrections are welcome. Development is ongoing, and Gerber files for all Demon variants will be released eventually. A preliminary Phat PCB design is available.
This project is by no meany complete or possibly accurate it is my own account of insight may be incorect, and I'm open to suggestions and correctons.

## Acknowledgements
We thank Coz, Goobycorp, and Eaton for their invaluable contributions.

# Open Demon Project
![sch](https://github.com/professor-jonny/open_demon/blob/main/pics/demon.png)
![pcb](https://github.com/professor-jonny/open_demon/blob/main/pics/phatpcb.png)



