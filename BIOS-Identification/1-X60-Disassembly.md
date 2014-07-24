The best way to identify the BIOS chip is to visually identify it. Unfortunately, this means that you will need to disassemble the X60 down to the motherboard to even get a glimpse.

(There might be a way to find the BIOS chip without disassembly, since the model ID signature does appear in Flashrom, if not auto detected)

[Source: Patterns in the Void - Replacing an X60 Bootflash Chip](https://blog.patternsinthevoid.net/replacing-a-thinkpad-x60-bootflash-chip.html)

## Step 1 - Keyboard and Palmrest

Remove the keyboard and the palmrest.

(add images below the gif!)

![](https://blog.patternsinthevoid.net/static/images/2013/12/001-025.removing-keyboard_small.gif)

## Step 2 - Remove Motherboard Cards

Unplug the speaker, and remove the Wifi and 3G cards.

![](https://blog.patternsinthevoid.net/static/images/2013/12/051-056.remove-radios_small.gif)

## Step 3 - Remove Mainboard and Power Adapter

![](https://blog.patternsinthevoid.net/static/images/2013/12/056-111.remove-mainboard_small.gif)

## Visually identifying the BIOS Chip

1. Remove the black plastic covering on the underside of the right USB ports.
2. There are three chips next to each other: one large "Lenovo" labeled ASIC chip in the middle, a medium sized BD4175KVT chip on it's right; and finally, the BIOS chip on the left.
3. Remove the sticker covering the BIOS chip. If it doesn't come off easily, get a Q-tip with rubbing alcohol.
4. The name of the BIOS chip is printed in very tiny letters on top of it. You may need a magnifying glass and good lighting to see it. It is either `MX25L1605D` or `SST25VF016B` for the X60 series.

### Sources

* [Patterns in the Void - How to Access BIOS Chip](https://blog.patternsinthevoid.net/static/images/2013/12/x60-bootflash-location-small.jpg)
* [Coreboot Mailing List - Complete How-to install Coreboot on X60](http://www.coreboot.org/pipermail/coreboot/2013-June/076070.html)
* [New Mexico GNU & Linux Users Group - VIsually Identify coreboot BIOS on lenovo x60 tablet](http://nmglug.org/coreboot-bios-on-lenovo-x60-tablet/)

## Dust off the Fan

If the fan on your X60 is caked with dust, it cannot cool the system as effectively.

Get some compressed air, hold a small screwdriver in the gap of the fan to prevent it from spinning, and blow away the dust bunnies.

## Reapply Thermal Paste

Since your motherboard is already disassembled, we strongly recommend taking the time to reapply the thermal paste on the CPU. 

After 6 years of use, the paste has probably worn out, which compounds the X60's existing tendency to overheat. Overheating will cause the computer to shut down randomly, not to mention induce possible damage to system components. 

Worse yet, Libreboot itself has issues with CPU load handling, further straining the CPU to breaking point.

We recommend Arctic Silver, using the surface spread method. Make sure to keep the thermal paste on the CPU, and don't let it get anywhere else on the computer.

### Sources

* [mm0hai - Servicing a ThinkPad X60s fan](http://mm0hai.net/blog/2012/08/06/Servicing-a-Thinkpad-x60s-fan.html)
* [Arctic Silver - Intel CPU Thermal Paste Application Method](http://www.arcticsilver.com/pdf/appmeth/int/ss/intel_app_method_surface_spread_v1.1.pdf)
* [Markus's Journey to the CCIE - X60 Tablet Fan Replacement](http://chasingmyccie.wordpress.com/2012/05/05/lenovo-x60-tablet-x60t-disassembly-fan-replacement/)
* [Triathlon Mike - Thermal Paste Does Make a Difference](http://www.michaelm.info/blog/?p=1332)

## Put it all back together

![](https://blog.patternsinthevoid.net/static/images/2013/12/160-175.reassemble_small.gif)