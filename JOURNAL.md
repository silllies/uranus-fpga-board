---
title: "uranus-fpga-board"
github: "https://github.com/ChoccoAerospace/uranus-fpga-board/tree/main"
description: "Found out about FPGAs and thought they were very cool—decided to try making a board for a nice one. Calling it Uranus because Uranus is a goated planet (YOOR-uh-nús)"
created_at: "2026-10-04"
total_time: "25h 30m"
---

# October 4, 2026: FPGAs are Actually Pretty Hype
<!-- fabricate:entry 138 -->

![Pinout of Xilinx Kintex 7 XC7K160T-2FFG676C](https://github.com/Silllies/uranus-fpga-board/blob/main/images/160Tpinout.jpg)

Okay, the most important part of an FPGA board is the FPGA, so what is it, exactly? An FPGA stands for "Field-Programmable Gate Array," and if you are like me, that provides no usable information. FPGAs are a type of Programmable Logic Device (PLD). Other PLDs, like CPLDs, typically have a coarser, simpler architecture than an FPGA. FPGAs use Hardware Description Languages (HDLs) to control how their many programmable logic blocks work with other blocks to perform functions. The Field Programmable part of an FPGA comes from how its wiring can change based on instructions from something written in an HDL, as opposed to an Application-Specific Integrated Circuit (ASIC) that cannot change its logic wiring. FPGAs are often used to prototype before going all in on production of an ASIC. In my case, I am using an FPGA because I want to be able to change it to whatever I want and don't want to order a huge load of ASICs. An FPGA is NOT a microcontroller, but you can describe its hardware (lol) to make it work like a microcontroller, or even a processor.

**What makes an FPGA board a Tier 2 project, offering $250 in grant funding?**  
While putting this in the same tier as a robot arm, handheld console, or battle bot might seem strange, as the others have far more physical possibilities and polish (maybe), an FPGA board is very complex and requires precise PCB design. FPGAs often require several different voltages, and they have so many pins that a 4- or 6-layer PCB is necessary, as well as DDR RAM if you want storage that is slightly longer than the BRAM. Obviously, there is more, but I don't know enough.

**What FPGA will I use on my board?**  
Uhhhhh idk yet, but I have a good idea. I want to be able to do some cool stuff with electricity and lots of data, so I want something on the higher end of the speed scale. I think I would get a Xilinx Kintex-7 XC7K70T-2FBG676C or XC7K160T-2FFG676C. The former is about $35, and the latter is more like $80. If I can find a way to level up this board to Tier 1, I may go even further because I think that sounds cool.

**Other stuff that would probably go on the board**  
RAM, I haven't researched enough about RAM yet. Capacitors, lots of them. JTAG adapter. Ports going out to connect with other devices. Headers, if I want to connect another module directly if I fear the cables. Lots of power stuff and regulators. Clock generators. Buttons and LEDs.

I intend to supply power to this board with a bench power supply (OOH, I could make that!), so it doesn't require an extreme amount of power thingamabobs, just enough to handle the power degradation from traveling over a cable.

___

## Brainthinks from the same day

![Mezzanine connector](https://github.com/Silllies/uranus-fpga-board/blob/main/images/mezzanine.jpeg)

I don't think headers would be the best to preserve signal integrity. Upon doing more research, it sounds like edge connectors or mezzanine connectors will be the best. Both for signals and power. Edge connectors put the card at a 90-degree angle, while mezzanine connectors put it parallel. I think a mezzanine connector would be better, even for power. The large number of pins on a mezzanine connector helps reduce power quality degradation by lowering parasitic inductance, canceling out the magnetic field (if PWR and GND are alternated), and having a large cumulative surface area. Surface area is important because, while power will be sent as DC, the FPGA will pull power in very fast, erratic pulses, creating AC noise. I will have a mezzanine connector for both power and a peripheral board for testing.


___

**Total time spent: 2h 30m**

# October 5, 2026: That's, ummmm, a lot of pins
<!-- fabricate:entry 152 -->

![Schematic of the previously mentioned Kintex 7 160T chip](https://github.com/Silllies/uranus-fpga-board/blob/main/images/kintexSchematic.png)

I mean I did say that PCB design is comforting, but that was with a simple board that connects some preassembled boards. Now I have to learn what all these pins mean.

Okay, I did some more research and have decided that this project likely belongs in Tier 1. An FPGA board is provided as a Tier 2 example for simpler FPGAs, but I am using a far more complex FPGA, as well as DDR RAM. A similar FPGA board from Blueprint that also had RAM was rated as Tier 1. I also did some more research on the processor and found a better and cheaper model! The XC7K160T-3FFG676E is faster (Speed grade 3 instead of 2) and has a higher maximum operating temperature (E has a max of 100 degrees while C maxes out at 85), and LCSC sells it for $73.50, as opposed to $96 for the other. I don't really need the 676 ball package, but the 484 version doesn't have an equivalent sold by LCSC.

NEVERMIND! LCSC sells an XC7K325T-3FFG900E for nearly the same price! I do not need this, but why not pay the same price for something FAR better! Now I have to redesign my schematic (At least where I got to) and get less sleep (I need more sleep; it's Klausur season)

Okay, there are a lot of power lines. I don't understand it all, but there are several voltages the chip takes. VCCO pins can take many voltages, depending on the interface. There is also a lot of stuff for the GTX transceivers, which I also don't fully get. I haven't gotten to labelling all the IO pins yet, (which is more than the 646-ball). I did route the GND, though!

![Schematic of the 325T](https://github.com/Silllies/uranus-fpga-board/blob/main/images/325Tschematic.png)


___

**Total time spent: 3h 20m**

# October 6, 2026: RAMpocalypse
<!-- fabricate:entry 167 -->

![RAM chip](https://github.com/Silllies/uranus-fpga-board/blob/main/images/W632GU6NB-11.png)

Found some RAM I am fine with using. 4 chips of 16 bit DDR3L 933 MHz. Each chip is $11, which I find quite insane. $11 is also the lowest price I found during my searching. The chip is the Winbon W632GU6NB-11, sourced from LCSC, obviously. I think I will use Aisler as they are based in Europe, have a minimum assembly count of 1 (I believe), and can source their parts from LCSC. I will probably see if they can pre-order the FPGA and hold it for me as I design it, as there is only 90 left. I will also have to ask on slack. Parts and manufacture will probably be more expensive, but it will be cheaper than assembling 2 boards from JLCPCB.

I added the RAM to the schematic, routed grounds, and am currently waiting for Vivado to download so that I can use MIG to handle the DDR3 controller. I am doing this now because it also tells what pins I should I use out of the 900 I have to choose from :3

I learned that 16 bit DDR3 chips separate half the bits into "upper" and half into "lower" bits. This is because the 16 bit chip is structured as two 8 bit slices. The signals alternate between the two slices, controlled by upper and lower data strobe differential pairs.

Finished schematicizing (???) the 325T and the 4 RAM chips. I initially tried to make it look nice, but the sheer volume of connections decided that would not happen. Yes I did actually spend 6 hours doing this; of course, not in one sitting. Maybe like 2 or 3. I used MIG to generate the memory interface stuffs, which does include the wiring, which is how I put everything together (and why the connections could seem random). I am dead tired now and there will be tea to wake up to from my rocketry club back home. goodnite!

Upon Looking at the schematic again, I realized that people don't design schematics like this. I will probably make it look passable tomorrow

![Wacky schematic](https://github.com/Silllies/uranus-fpga-board/blob/main/images/325TnRamSchem.png)

**Total time spent: 6h**

___


# October 7, 2026: Restart!

![Nicer RAM schematic](https://github.com/Silllies/uranus-fpga-board/blob/main/images/newRamSchem.png)

I will be making the schematic readable today, but right now, I found a cheaper RAM option. The Winbon W63**1**GU6NB-11 is less than half the price of the 632, but also has half the storage. I have also set out other components I want to include on my board: HDMI port, MicroSD card slot, some LEDs, and some DIP switches.

I was able to successfully make the schematic look nicer! I haven't done the FPGA side, but it is getting late. I was also able to do more pins, such as the VREF pins, which need to be half the voltage of the main RAM supply voltage. The footprint of the new RAM is the same, the only difference being that it has 1 less address pin. There were more differences for MIG. I also switched to dark mode, which is the most important thing done this session.

**Total time spent: 3h 20m**

___

# October 8, 2026: New day, new RAM

![Basic FPGA Schematic](https://github.com/Silllies/uranus-fpga-board/blob/main/images/newNewRamSchem.png)

Realized my RAM was out of stock. found a cheap RAM chip and redesigned the schematic. This one looks even better! It also has decoupling capacitors. I also got a scare when searching for a crystal oscillator, thinking I would have to get a custom one, but realized MIG was being MIG and I could change the RAM speed to get a more favorable frequency.

RAM schematic is all done. FPGA memory interface is almost done, I just need to do some power thingies like decoupling. I also began setting up my mezzanine connector. The really nice thing about it is that there is a standard called FMC (FPGA Mezzanine Connector, wow!). I will be using the FPGA's GTX transceivers, so I have to use the HPC (High Pin Count) variant. Because I want to be extra, I felt the need to use 8 GTX lanes (Out of my 12) for this FMC, meaning I need two of them.

![more pins!!!](https://github.com/Silllies/uranus-fpga-board/blob/main/images/iLoveFMCHPC.png)

You might be able to see the new problem here. I'll figure something out (add a third?). The specific FMC I am using is the ASP-134488-01. 

Another annoying thing with the GTX system is that it requires very precise power supplies.

**Total time spent: 5h 50m**

___

# October 9, 2026: New day, reday

![Schematic for the FMC HPC](https://github.com/Silllies/uranus-fpga-board/blob/main/images/fmcHpcSchem.png)

Get it? It's like a play on words of "redo". Anyways, I'm not using 2 HPC connectors, just 1. Good thing I never started anything intense. I did attached the signal names to all the pins on the FMC to later attach to the FPGA. To find out which pins I'm supposed to attach to what signals, I did some research and found the KC705 Evaluation Board, which documents its full pin assignments. It has an HPC and LPC connector, but many of their pins are left unconnected, so I can't copy it line for line, I will probably group pins by differential pair, routing them to the closest banks available. Higher priority data lines will be assigned to banks closer to the connector's side of the FPGA, which will be on the opposite side from the RAM.

I also thought longer and harder about what I want this project to be. I started this board to test out a few ideas I had in mind, but these ideas don't utilize all the capabilities of the FPGA. I think I will keep this board to its original design intention, but I might make another, new FPGA board that is more intended to be a full dev board. I feel really close to getting out of the schematic phase, I just need to figure out the pins for the HPC connector, add some smaller components, work out the power, then boom! Power won't be too hard since it is just the schematic. PCB will be far more annoying. For the PCB, I am thinking of doing an 8 layer board with 3 ground planes and a power plane that is split into various regions to cover the varying voltages of the board. Layer 6 would have a ground plane on both sides of it, so it would be the high speed or sensitive signal layer. layer 1 and layer 8 are on the surface, but they are also on the surface. They are adjacent to a ground plane, though. Layer 3 is adjacent to a ground and the power plane. The ground plane is closer, but it isn't as good as the others. Or maybe it is fine, I don't know. 

Actually, as I am writing this, I think I might just do a LPC connector. Like I said earlier, this board is being designed for a few specific purposes in mind, and those purposes don't need a whole HPC connector.

**Total time spent: 4h 30m**

___

# October 10, 2026: Planned the FMC connections

![Updated FMC pin jobs](https://github.com/Silllies/uranus-fpga-board/blob/main/images/proprietaryFmc.png)

I am sticking with the HPC connector because it is cheaper than the LPC connector. I also realized that I don't have to follow the standards set in place because I am no longer designing this board to be all encompasing. For the mezzanine card side, the LPC connectors are cheaper, so I will only use the pins on the HPC that an LPC uses.

I began planning the routing of all the pins. I first looked at a map of the chip to determine which banks I should prioritize, then I started mapping out where all of the pin pairs are at in those banks. Quickly into that, though, I realized the futility of it all and that I am going to have to length match no matter what, so I just assigned signals based on the order of the pins in the schematic. I also added more components to my schematic, including the JTAG Converter, flash storage, USB-C connector, 200 MHz crystal oscillator for the DDR3 RAM, a 12 MHz crystal for the JTAG Converter, an EEPROM for the vivado config, and a USB ESD protector. I have not routed them yet, though.

I am now going back to the standardized method because  I don't know what the future will bring, and I want to be able to do cool stuff. Guhhh. I am not an efficient worker. And I also often have a cat on my lap, so that further reduces efficiency. For tommorow, though!

![FPGA w/ decoupling caps](https://github.com/Silllies/uranus-fpga-board/blob/main/images/fpgaWDecoupling.png)

![RAM with an unrouted oscillator](https://github.com/Silllies/uranus-fpga-board/blob/main/images/ramWOscillator.png)

**Total time spent: 6h 10m**
