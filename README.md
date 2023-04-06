# Open Demon Project
![sch](https://github.com/professor-jonny/open_demon/blob/main/pics/demon.png)
![pcb](https://github.com/professor-jonny/open_demon/blob/main/pics/phatpcb.png)


## Overview:
This is an in progress open source recreation project of Team xecuter Dual Nand device. it is the most powerfull multi function nand device created for the xbox 360.
hopefully by starting this project we may get more onboard in recreating the firmware and fixing a few bugs and possible addon companion hardware devices.

## Demon background:
The Demon was first released in febuary 2012 in conjunction with an update to J-Runner that included features to control and flash the demon for an all in one solution for programming it was revolutionary at the time and sold well during the initial stages but possibly over hyped and did not actually sell that well compaired to other xecuter devices.
With the advent of stealth services it semi become redundant but it's true potential was cut short from lack of continuation and release of addon hardware that was promised but never arrived.

Many add on projects were planned but never made it outside of the Team xecuter dev team, sadly fincinal and legal issues were plagued by K3 (owner of TX at the time) after his move from the UK evenuually leading  to the sale of Team xecuter to Max.

The Demon has access to the uart of the console that could interact with the console, along with plenty of aditional onboard I/O enough to run a complete seperate 8bit bus and a few spare I/O that could be bitbanged to create spi i2c or for any other potential use...
On July 11th 2012 Team Xecuter announces an Android and iOS app to remotely control the Xecuter Demon over Bluetooth or WiFi but sadly this never made it.
LCD support was also investigated and a few prototypes were made and distributed between the TX devs to play with but sadly only Switch! the nand switching plugin made it to the platform.

## Project Background:
After discussing the project with fellow enthusists and reading the details of the long winded process of breaking the xbox 360 ode device recently and being given some insite to the hardware I went to work recreating schematics.
I was also given pictures of a pre release slim PCB un populated to crosscheck my schematics with a bit of help.

The schematic and hardware information gives us a bit of insite into how the device operates and talks to the flash controller and knowing the hardware and its components and its's design is a big step in the corect direction to be able to reverse engineer.

One thing that was never released was how to control the hardware and I/O on the demon from the console I would like to document how the I/O on the hardware is addressed (if it made that far) and if there is some sort of API to control it over uart there is a API for PC based usb control.
Third party addons could be realised and give applications access to the I/O on the device what would be better than having a LCD on the fromt of your xbox 360 ?

## Theory of operation:
The demon nand address lines and control is mostly in paralell with the consoles onboard nand but a try state bus tranciever is used control reading and writing to isolate the micro from the nand control logic.

It seems only CE and RB is used to control the nand switching functions as all the other control lines are broken by the bus tranciever there is some Nand gate logic in U3 to ensure both CE pins are not driven at the same time.
C3 seems to be used as timing to delay the switching on and off of CE with the NOR gates in U4 to the Demon's nand, this is likely used with RB to to know if the chip is busy before driving OE to enable hyjacking CE to enable the Demon's onboard nand.
OE is used to enable the onboard tristate bus tranciever as well as the CE logic to the demon's nand when CE of the mother board is in a high state.

The way it is designed it looks like it captures control of the nand between read/ write cycles aserts control and the other signals lock it while it is busy.
I really need to capture the micro to nand chat and see how it really works but the theory seems legit :-).

## Caveats:
The bootloader of the at90usb microcontroller was customised with encription (RSA-2048 so we believe) to secure the device from copying the firmware and installing unofficial firmware.
This poses us some challanges as none of the existing firmware from TX are able to be reverse engineered.
All existing devices would be incompatable with software we would create without updating the bootloader
Installing a new bootloader would break the ability to use existing firmware images.
We can't build firmware to work with the current bootloader as it will fail sigunature checks.

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
The future plan is to recrate the demon opensource from the ground up with compatibility of the existing device in the short term, with a hardware revision likely required as the main micro and nand while available online is discontinued by the manufactures.

But the real motavation behind this project is to create completely a new device using FPGA in conjunction with a microcontroller with this we could also attatch ram or use embeded sram in the fpga to on the fly build nand images and patch the kernel at boot time with a small configuration app on the console

This fpga/ micro could also give us a shared memory window attached in the nand address space for console to microcontroller communication from there we can offload segments of ram, relocate code, coprocessing etc...

From testing by Eaton the Nand bus operates at around 1.4mbps not flash by any means but will be suitable for low bandwith peherfials and information.
It is unknown if removing nand chip timing by hosting it in faster media will increase this.
At the moment this is a pipe dream but it is in the realms of possible if we can get devs motivated and onboard, if you have experence in CPLD or Fpga design and programming and be willing to lend a hand great, I'm by no means a skilled dev just a tinkerer and Xbox enthusist like many.
more on that on eaton blog here:
[eaton works](https://eaton-works.com/2023/01/09/how-microsoft-attempted-to-make-the-xbox-360-dashboard-load-faster/)

I have used Eagle for the Schematic design you can download a free version of eagle here:
[Autodesk Eagle]( https://www.autodesk.com/products/eagle/free-download)

## Licensing
This Project is free and open source.
  *  Hardware and schematics are shared under the [CERN OHL version 2.0.](https://ohwr.org/cernohl).

If you like my work please consider a small donation to XBMC4XBOX, Team Resurgent or one of the many Great Xbox resources out there you may also send me sample of clones of this project or buckets or cash or what ever :-)
[Teamresurgent donate]( https://www.patreon.com/teamresurgent)
[xbmc4xbox donate](https://www.xbmc4xbox.org.uk/donate/)

## disclaimer
This project is by no meany complete or possibly accurate it is my own account of insight may be incorect, and I'm open to suggestions and correctons.
I lack time to commit to this project but will slowly progress in time and I will have gerbers available for all the demon variants.
I started drawing up a Phat PCB but is not quite finished but I have provided what I have done so far that may be of assistance.

## Shoutouts
Id' Like to thank Coz, Goobycorp and Eaton for their help and info it would not of made it anywhere with out their assistance and info.




