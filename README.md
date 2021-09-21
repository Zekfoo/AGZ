# AGZ-CPU-01

![](https://github.com/Zekfoo/AGZ/blob/main/Images/AGZ_Front_2.jpg)
![](https://github.com/Zekfoo/AGZ/blob/main/Images/AGZ_Back_2.jpg)

To commemorate the 20th year since the Game Boy Advance’s release, I have designed a full revamp of its PCB, using only the most essential components from the original hardware, and completely redesigning the remainder of its circuits. 

Although the PCB has a GBA form factor, the CPU is taken from a GBA SP for a greater design challenge. The PCB itself has been entirely assembled and soldered by hand. As a play on the original model numbers *“AGB-CPU-01”* for the PCB and *“AGB-001”* for the console itself, I have dubbed my design the *“AGZ-CPU-01”* and *“AGZ-001”* respectively (bonus points if you can guess what the Z stands for). 

![](https://github.com/Zekfoo/AGZ/blob/main/Images/PCB_Front.jpg)

This is a one-off production of what I envisioned as my ideal GBA and has been submitted as an entry to r/Gameboy’s summer build contest. It is a one-of-a-kind Game Boy in the most literal sense.

### Features

+ 4-layer 1.2mm PCB, blue silkscreen, ENIG surface finish
![](https://github.com/Zekfoo/AGZ/blob/main/Images/PCB_Front_Bare.jpg)
![](https://github.com/Zekfoo/AGZ/blob/main/Images/PCB_Back_Bare.jpg)
+ GBA SP CPU (CPU AGB B E)

+ 1700mAh LiPo battery

  + USB-C rechargeable with charge LED indicator

  + Supports safe charge-and-play

  + On-board battery protections – short-circuit, over-charge, over-discharge, over-temperature

+ Completely redesigned power delivery

  + Power switch debouncing

  + 5V boost converter

  + 3.3V buck converter

  + 2.5V buck converter + 1.8V linear regulator

  + Full ground plane
  
+ Completely redesigned audio circuits

  + Stereo volume potentiometer

  + 2-stage active filtering

  + Dedicated speaker and headphone amplifiers

  + 1W 8-ohm speaker

+ Tactile buttons

  + Snaptron tactile domes for DPAD and A/B

  + Tactile switches for START/SELECT

+ FunnyPlaying IPS V2 display

+ Hand-polished clear shell, dipped and coated with floor polish

+ Blue buttons, bumpers, speaker ring insert, and battery cover

## The lengthy write-up

### Why redesign the GBA?

I really want to like the Game Boy Advance. Growing up with a GBA SP, I was spoiled by its clicky buttons, rechargeable battery, and illuminated screen. When I finally got my hands on one, I couldn’t be more disappointed by the stark difference in feel and function. While today’s retro modding scene has produced many improvements for the GBA (referred to from now on as its codename AGB), the console still has many quirks that simple modding hasn’t been able to fix, but that can be addressed in a circuit redesign. In no particular order, here are the problems I have with the Game Boy Advance:

**It runs on AA batteries.** While this certainly wasn’t an issue 20 years ago, and many won’t find it an issue today, we now live in a rechargeable world. The devices I use day-to-day are all plug-in rechargeable, and the idea of having to swap out batteries, even if to recharge them externally, feels very antiquated. 

**Its buttons are mushy.** Like all the Game Boy iterations before it, the AGB uses conductive button membranes which, when pressed, bridge contacts on the circuit board underneath. Only the GBA SP (AGS) uses actual tactile switches, which are what give it its clicky feel. 

**Its screen is unlit.** While the AGS comes with either a frontlit (AGS-001) or backlit (AGS-101) screen, the AGB’s screen is neither. Anyone who has owned an AGB would understand the struggle of trying to play under poor lighting. This problem is more-or-less resolved now that IPS screen replacements are commonplace in the modding scene, however the higher power consumption of these IPS screens lead to other problems.

**Its power delivery is lacking.** The AGB uses a monolithic power management IC to regulate its system voltages – 2.5V, 3.3V, 5V, as well as +13V and -15V to bias the stock LCD. My measurements show that the 3.3V and 5V rails are regulated at about a 90kHz switching frequency, which is very low for switching regulators by today’s standards (regulators used today typically switch in the 1MHz range). I won’t discuss how switching regulators work here, but it suffices to say that this low switching frequency necessitates large electrolytic capacitors, which aren’t best suited for producing clean voltage supplies. 

![](https://github.com/Zekfoo/AGZ/blob/main/Images/3V3_OEM.png)
> 90kHz switching frequency on the stock 3.3V rail

**Its audio is noisy.** The AGB is known to have a persistent buzz and hum from its speaker and headphone output. This noise is even more pronounced when the power circuitry is pushed to its limits by power-hungry mods and flash carts. I theorize this to be due to interference caused by the low frequency switching of the power rails. This issue isn’t helped by the fact that the AGB’s PCB has poor ground pours, limited by its 2-layer PCB design, which can exacerbate noise issues.

### A few last nitpicks

The low-battery light on the AGB is powered directly from its battery voltage. This means that as the batteries deplete, the light dims, sometimes to the point of being unlit entirely. While this seems to be a feature intended by the original hardware designers, I frankly don’t like it.

The power switch carries the full system current, making it the most common point-of-failure for all Game Boy handhelds, to the extent that the first step in troubleshooting a “dead” Game Boy is cleaning the power switch. Even a small amount of gunk buildup on the switch contacts can cause a considerable voltage drop across the switch, which is commonly the cause of flickering LEDs, reboots upon being bumped, and consoles that won’t turn on.

### The AGZ-001 design

The AGZ-001 design addresses all the above-mentioned issues. First off, the AGZ-CPU-01 is a 4-layer PCB, allowing for simpler power and signal trace routing and a dedicated ground plane layer. The board uses a CPU AGB B E salvaged from a GBA SP, which is functionally identical to a GBA CPU (CPU AGB A) but requires an extra 1.8V supply voltage and has a narrower pin pitch, adding some more challenge to the design and assembly – you could also say that this is a GBA SP in a GBA! 

##### Power

The power circuits support a 1700mAh LiPo battery, with USB-C charging and a charge LED indicator like the GBA SP’s. The charge circuit is designed to safely charge the battery while playing – the battery is isolated from the rest of the system when charging, allowing the system to be powered directly from USB voltage. The battery uses a 3-pin connector which includes a thermistor to support overtemperature protection during charging.

The AGZ-CPU-01 uses discrete voltage regulators for each power rail, in place of the original power management IC. All the switching regulators switch in the 1MHz range, which greatly reduces the capacitance needed on each rail. A boost converter supplies the 5V rail while the 3.3V and 2.5V rails are generated by buck converters, and a linear regulator is used to further drop from 2.5V to produce the 1.8V rail. This design omits the +13V and -15V rails and the AGB-REG output voltages since they are not needed when using an IPS display. As opposed to the AGB, the AGZ-CPU-01 powers the low-power LED off of the 3.3V rail, so that it no longer dims as the battery voltage drops. The power switch has also been changed to no longer carry the full system current, and instead is part of a larger switch circuit with debouncing to mitigate the effects of a dirty switch.

![](https://github.com/Zekfoo/AGZ/blob/main/Images/Exploded_View.jpg)

##### Audio

I want to preface this next section by saying I am not an audio engineer or even an audiophile, but as an electrical engineer, I know the fundamentals well enough to produce a functional audio circuit. I have no doubt that this design is overkill for the desired outcome. The CPU outputs a 65kHz PWM audio signal. To get a clean audio waveform in the 20Hz-20kHz range, this signal must be low-pass filtered with a steep cut-off between 20kHz and 65kHz. To achieve this cut-off, I opted for a 2-stage active filter (Butterworth Sallen-Key) along with some additional passive RC filters. Overall, this produces the following frequency response curve. 

![](https://github.com/Zekfoo/AGZ/blob/main/Images/Audio_FR.png)
> Flat frequency response over audible range (20Hz-20kHz), sharp cut-off past 20kHz

The volume of the audio is adjusted using a stereo wheel potentiometer, an aftermarket replacement for those used in Game Boy Colors. Dedicated speaker and headphone amplifier ICs amplify the filtered audio for either output. Here’s a [line-out recording](https://github.com/Zekfoo/AGZ/blob/main/Audio/Line_Out_Sample.wav) from the headphone jack.

##### Buttons

Many tactile button mods exist for the AGB which use tactile switches, including my own attempt at the concept. From my experience, while these tactile switches produce a satisfying click, they tend to be very stiff and fatiguing to play on. For the AGZ-001, I opted instead to use tactile domes from Snaptron for the DPAD and A/B buttons, with ENIG surface finish on the PCB to improve the contacts. These domes have a lower actuation force than typical tactile switches while still producing a strong click when they bottom out. That said, the START and SELECT buttons still use tactile switches since they are what I found to feel the best for these two buttons.

##### Exterior

The AGZ-001 uses a clear aftermarket shell from FunnyPlaying, which has been sanded and polished, then dipped in floor gloss for a smooth transparent finish. Holes were also drilled and filed for the USB-C port and charge LED. Blue components were chosen to accent with the blue PCB, and custom labels printed by [BluishSquirrel](https://bluishsquirrel.co.uk/) make for a finishing touch on this one-of-a-kind GBA.

![](https://github.com/Zekfoo/AGZ/blob/main/Images/AGZ_Back_1.jpg)

### Acknowledgements

Thanks to all the members of the [Gameboy Discord](https://discord.gg/gameboy) community for their support and encouragement.

Special thanks to [Gekkio](https://gekkio.fi/) for his [AGS circuit schematics](https://github.com/Gekkio/gb-schematics).

Special thanks to [HDR](https://martinrefseth.com/) for compiling the [AGB circuit schematics and board scans](https://nintenfo.github.io/repository/systems/GBA/documentation/schematics/).

Special thanks to [b23n](https://mmm.page/b_2_3_n) for sharing his [shell polishing method](https://mmm.page/b_2_3_n.mod_polishing).
