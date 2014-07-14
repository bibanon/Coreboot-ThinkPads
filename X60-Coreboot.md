## ThinkPad X60 Coreboot

Installing Coreboot on the ThinkPad X60 makes the good ol' computer into one of the few fully open-source laptop, [endorsed by the Free Software Foundation](http://www.fsf.org/news/gluglug-x60-laptop-now-certified-to-respect-your-freedom). When the Libreboot fork of Coreboot is used and the original Intel wifi card is replaced, no proprietary blobs are used on this computer.

All ThinkPad X60 variants, from the X60, X60s, X60 Tablet are supported. (though the wacom pen lacks support, it shouldn't be too hard to support, just nobody bothered) The ThinkPad T60 and T60p are in the process of joining the X60 as [an officially supported platform for Libreboot.](http://trisquel.info/en/forum/best-free-laptop); a godsend, as one with Intel graphics and QXGA resolution (2056x1536) makes it the best Libre laptop you can buy. The R60 uses a similar method.

Although installing Coreboot is only a matter of a few software steps, the official guide is very hard to follow, and Gluglug hasn't released their own documentation publicly.

Libreboot actually does have it's [own official documentation](http://libreboot.org/howto.html), though it is very obscure and I have only recently found it.

## Steps

The full list of steps is shown below:

(You need Linux installed on your laptop before proceeding)

1. Fully disassemble the laptop, until you can flip over the motherboard.
  * Dust off the fan (recommended)
  * Reapply Thermal Paste (recommended)
2. Visually identify the BIOS Chip using a magnifying glass.
  * Download Libreboot Source Code
3. Patch flashrom to work with the BIOS Chip through SPI interfacing.
4. Build flashrom
5. Build Coreboot (optional)
6. Backup your original BIOS
7. Modify Coreboot ROM for first 64K of BIOS Chip
8. Coreboot First time flash
  * Recover from Bucts bad flash
9. Coreboot Second time Flash
  * Boot LiveUSB from Libreboot GRUB2 Payload
  * Compile Libreboot with SeaBIOS (optional)
  * Install Trisquel Linux (Optional)

## Disassemble

Disassemble the ThinkPad X60 all the way down to the motherboard.

[Source: Patterns in the Void - Replacing an X60 Bootflash Chip](https://blog.patternsinthevoid.net/replacing-a-thinkpad-x60-bootflash-chip.html)

### Step 1 - Keyboard and Palmrest

Remove the keyboard and the palmrest.

(add images below the gif!)

![](https://blog.patternsinthevoid.net/static/images/2013/12/001-025.removing-keyboard_small.gif)

### Step 2 - Remove Motherboard Cards

Unplug the speaker, and remove the Wifi and 3G cards.

![](https://blog.patternsinthevoid.net/static/images/2013/12/051-056.remove-radios_small.gif)

### Step 3 - Remove Mainboard and Power Adapter

![](https://blog.patternsinthevoid.net/static/images/2013/12/056-111.remove-mainboard_small.gif)

### Identifying the BIOS Chip

(There might be a way to find the BIOS chip without disassembly, since the model ID signature does appear in Flashrom, if not auto detected)

1. The best way to identify the BIOS chip is to visually identify it. Unfortunately, this means that you will need to disassemble the X60 down to the motherboard to even get a glimpse.
2. Remove the black plastic covering on the underside of the right USB ports.
3. There are three chips next to each other: one large "Lenovo" labeled ASIC chip in the middle, a medium sized BD4175KVT chip on it's right; and finally, the BIOS chip on the left.
4. Remove the sticker covering the BIOS chip. If it doesn't come off easily, get a Q-tip with rubbing alcohol.
5. The name of the BIOS chip is printed in very tiny letters on top of it. You may need a magnifying glass and good lighting to see it. It is either `MX25L1605D` or `SST25VF016B` for the X60 series.

* [Patterns in the Void - How to Access BIOS Chip](https://blog.patternsinthevoid.net/static/images/2013/12/x60-bootflash-location-small.jpg)
* [Coreboot Mailing List - Complete How-to install Coreboot on X60](http://www.coreboot.org/pipermail/coreboot/2013-June/076070.html)
* [New Mexico GNU & Linux Users Group - VIsually Identify coreboot BIOS on lenovo x60 tablet](http://nmglug.org/coreboot-bios-on-lenovo-x60-tablet/)

### Dust off the Fan

If the fan on your X60 is caked with dust, it cannot cool the system as effectively.

Get some compressed air, hold a small screwdriver in the gap of the fan to prevent it from turning, and blow away all that gunk.

### Reapply Thermal Paste

Since your motherboard is already disassembled, we strongly recommend taking the time to reapply the thermal paste on the CPU. 

After 6 years of use, the paste has probably worn out, which compounds the X60's existing tendency to overheat. Overheating will cause the computer to shut down randomly, not to mention induce possible damage to system components. 

Worse yet, Libreboot itself has issues with CPU load handling, further straining the CPU to breaking point.

We recommend Arctic Silver, using the surface spread method. Make sure to keep the thermal paste on the CPU, and don't let it get anywhere else on the computer.

* [mm0hai - Servicing a ThinkPad X60s fan](http://mm0hai.net/blog/2012/08/06/Servicing-a-Thinkpad-x60s-fan.html)
* [Arctic Silver - Intel CPU Thermal Paste Application Method](http://www.arcticsilver.com/pdf/appmeth/int/ss/intel_app_method_surface_spread_v1.1.pdf)
* [Markus's Journey to the CCIE - X60 Tablet Fan Replacement](http://chasingmyccie.wordpress.com/2012/05/05/lenovo-x60-tablet-x60t-disassembly-fan-replacement/)
* [Triathlon Mike - Thermal Paste Does Make a Difference](http://www.michaelm.info/blog/?p=1332)

### Put it all back together

![](https://blog.patternsinthevoid.net/static/images/2013/12/160-175.reassemble_small.gif)

## Flashing Coreboot

Flashing Coreboot on the X60 for the first time involves a lot of hard work. However, Coreboot on the X60 can be installed entirely through software, no special hardware is necessary.

A Linux distribution is required, and Ubuntu/Debian based distros are recommended. Make sure you have one installed on the hard drive before continuing.

### Download Libreboot Source Code

The Libreboot project has a convenient source code package with all the tools needed to flash Coreboot, as well as a prebuilt coreboot image.

[Download the package from the Libreboot site.](http://libreboot.org/#releases) Make sure to *Download Source*.

Then, extract the archive, which will give you an `X60_source` folder.

Enter the `X60_source` folder, it contains everything that you need.

### Patching `flashrom` to work with the BIOS chip

Enter the `flashrom` source code folder under `X60_source`.

Change the corresponding values under `flashchips.c` for your chip model:

* `MX25l1605D` - Actual family name in `flashchips.c` is `MX25l1605`
  * `.probe` - `probe_spi_res1`
  * `.model_id` - `0x14`
  * `.write` - `spi_chip_write_1`
* `SST25VF016B`
  * `.probe` - `probe_spi_res1`
  * `.model_id` - `0xbf`
  * `.write` - `spi_chip_write_1`

> **Note:** At the time of this writing, I actually had to edit ".name = "MX25L1605" instead of ".name = "MX25l1605D" because 605D falls under the 605 family, so 605D does not exist as a stand alone ruleset. However, this may change in future flashrom versions.

---

* [Flashrom Pastebin - Thinkpad R60 Flashrom Output](http://paste.flashrom.org/view.php?id=1454) - Probing for SST SST25VF016B.RES2, 2048 kB: probe_spi_res2: id1 0xbf, id2 0x41
* [Coreboot Mailing List - Invalid OPCODE](http://www.coreboot.org/pipermail/coreboot/2013-June/075962.html) - Allows us to infer that the model_id for `SST25VF016B` is 0xbf
* [Coreboot Mailing List - Bricked Lenovo T60](http://coreboot.org/pipermail/coreboot/2013-June/076027.html)
* [Donderclumpen - Coreboot on Macbook 2,1](http://macbook.donderklumpen.de/coreboot/)] -  Found `id1 0xbf, id2 0x2541` there, which corroborates with the inference from Peter Stuge.
* [SST - SST25VF016B Official Datasheet](http://ww1.microchip.com/downloads/en/DeviceDoc/S71271_04.pdf)

### Building Flashrom

1. Install all the critical build dependencies:

    sudo apt-get install pciutils pciutils-dev zlib1g-dev

2. Run the following commands to download the `flashrom` source code:

    svn co svn://flashrom.org/flashrom/trunk flashrom
    cd flashrom
    make

3. The `flashrom` binary will be placed into the source code folder. You will need to add a `./` before the command, so that the terminal will use the `flashrom` binary in the current folder.

    sudo ./flashrom

### Use `flashrom` to Identify BIOS Chip (experimental)

Below is an experimental method to determine the type of the BIOS chip in the X60. Be warned that while in theory, there is no reason why it should not work, it has not been fully tested for all use cases, so beware.

1. First, we will test if the BIOS Chip is `SST25VF016B`. Patch the BIOS Chip for `SST25VF016B` and run this command to search for it's identifier. Don't worry, nothing is being flashed.

    sudo ./flashrom -p internal | grep "0xbf"

2. If your BIOS Chip is a `SST25VF016B`, something similar to the following will be displayed:

    Probing for SST SST25LF040A, 512 kB: probe_spi_res2: id1 0xbf, id2 0x41
    Probing for SST SST25LF080A, 1024 kB: probe_spi_res2: id1 0xbf, id2 0x41
    Probing for SST SST25VF016B.RES2, 2048 kB: probe_spi_res2: id1 0xbf, id2 0x41
    probe_spi_res1: id 0xbf
    probe_spi_res1: id 0xbf
    probe_spi_res1: id 0xbf

3. If nothing was displayed, patch the BIOS Chip for `MX25l1605D` , and run this command to search for it's identifier:

    sudo ./flashrom -p internal | grep "0x14"

4. If your BIOS Chip is a `MX25l1605D`, something similar to the following will be displayed:

    0x14: 0x00000000 (SPID1+4)
    Probing for PMC Pm25LV512(A), 64 kB: probe_spi_res3: id1 0x1414, id2 0x14
    Probing for PMC Pm25LV010, 128 kB: probe_spi_res3: id1 0x1414, id2 0x14
    Probing for SST SST25LF040A, 512 kB: probe_spi_res2: id1 0x14, id2 0x14
    Probing for SST SST25LF080(A), 1024 kB: probe_spi_res2: id1 0x14, id2 0x14
    probe_spi_res1: id 0x14
    probe_spi_res1: id 0x14
    probe_spi_res1: id 0x14
    probe_spi_res1: id 0x14

5. If your BIOS chip is neither of these types (a very rare situation), only the following will be displayed. Make sure flashrom was patched correct.

    0x14: 0x00000000 (SPID1+4)

* [Flashrom Pastebin - Thinkpad R60 with SST25VF016B Flashrom Output](http://paste.flashrom.org/view.php?id=1454) - Probing for SST SST25VF016B.RES2, 2048 kB: probe_spi_res2: id1 0xbf, id2 0x41
* [Coreboot Mailing List - ThinkPad T60 with Macronix Flashrom Output](http://www.coreboot.org/pipermail/coreboot/2013-June/075961.html)

### Backup your Original BIOS

In case something goes wrong, it is a good idea to keep a backup of the original BIOS software.

    sudo ./flashrom -p internal -r factory.bin

This step is IMPORTANT since the factory BIOS in your machine is tied to your particular system board (or "planar" in IBM FRU terms) with a unique ID not present in factory BIOS updates.

### Compile Coreboot with SeaBIOS (optional)

Libreboot uses the GRUB2 payload in it's Coreboot image, but for the first flash, it is preferable to use SeaBIOS.

1. Install the packages needed to build Coreboot.

    sudo apt-get install libncurses-dev iasl libc6-dev-i386 svn

2. Grab a copy of the coreboot source code:

3. In the `coreboot` folder, change the entire `Payload` section in `.config` to the text shown below. This will embed SeaBIOS into coreboot, rather than GRUB2.

    #
    # Payload
    #
    CONFIG_PAYLOAD_SEABIOS=y
    CONFIG_SEABIOS_STABLE=y
    CONFIG_PAYLOAD_FILE="$(obj)/seabios/out/bios.bin.elf"
    CONFIG_COMPRESSED_PAYLOAD_LZMA=y

4. Then, just run `make` to build the `coreboot.rom` file.

    make

5. Copy the compiled `coreboot.rom` file to the `flashrom` source code folder.

* [Trisquel Forums - X60 Flashing Guide is hard to understand](http://trisquel.info/en/forum/coreboot-flashing)
* [Coreboot Wiki - Compiling Coreboot](http://www.coreboot.org/Build_HOWTO)
* [Coreboot Wiki - Fchmmr's Guide to compiling Coreboot for X60 with GRUB2 Payload and no nonfree binaries](http://www.coreboot.org/User:Fchmmr)

### Modify Coreboot ROM

1. Copy the `coreboot.rom` file in `X60_source` to the `flashrom` source code folder.

2. Run the `dd` command below to backup the first 64K of data from `coreboot.rom` .

    dd if=coreboot.rom of=top64k.bin bs=1 skip=$[$(stat -c %s coreboot.rom) - 0x10000] count=64k

3. Run the dd command below to display the first 64k of `coreboot.rom` .

    dd if=coreboot.rom bs=1 skip=$[$(stat -c %s coreboot.rom) - 0x20000] count=64k | hexdump

4. Verify that the complete range is filled with `ff` bytes before proceeding.

> The output of the `dd` command above must EXACTLY match the text below. If not, the coreboot image needs to be rebuilt with the second-to-last 64kbyte block unused.

    0000000 ffff ffff ffff ffff ffff ffff ffff ffff 
    *
    0010000

5. Run the `dd` command below:

    dd if=top64k.bin of=coreboot.rom bs=1 seek=$[$(stat -c %s coreboot.rom) - 0x20000] count=64k conv=notrunc

* [gmane.linux.bios Mailing List - LinuxBIOS on T60](http://comments.gmane.org/gmane.linux.bios/69354) - Peter Stuge's Method of installing Coreboot on the X60.

### Flash Coreboot for the First Time

1. Run the `bucts` command to make the first 64K of the BIOS bootable.

> **Warning:** This step is extremely important, because the remaining read-only 64K acts as a safety net in case Coreboot is flashed incorrectly.

    sudo ./bucts 1

2. Run this `flashrom` command to install Coreboot to your BIOS chip.

    sudo ./flashrom -p internal -w coreboot.rom

> **Note:** This flash will take a while, and will output errors for addresses 0x0 and 0x1f0000 if working with a 2 Mbyte flash chip.

3. At the end, flashrom may output `FAILED!` (since the last 64K is write protected). No need to panic; errors are normal. However, the error output **must match** the text shown below:

    Reading old flash chip contents... done.
    Erasing and writing flash chip... spi_block_erase_20 failed during command execution at address 0x0
    Reading current flash chip contents... done. spi_block_erase_52 failed during command execution at address 0x0
    Reading current flash chip contents... done. Transaction error!
    spi_block_erase_d8 failed during command execution at address 0x1f0000
    Reading current flash chip contents... done. spi_chip_erase_60 failed during command execution
    Reading current flash chip contents... done. spi_chip_erase_c7 failed during command execution
    FAILED!
    Uh oh. Erase/write failed. Checking if anything changed.
    Your flash chip is in an unknown state.

> **Note:** If you get a different set of errors, the flash has failed. DO NOT TURN OFF YOUR COMPUTER!  

> **Tip:** One of the most common issues is failing to set `.write` - `spi_chip_write_1` in `flashchips.c`, so fix that, rebuild flashrom with `make`, and try again.


4. Run the `bucts` command below to check whether it was set successfully. If the line with `Current BUC.TS=1` is identical, you may continue safely.

    sudo ./bucts

---

    bucts utility version '2'
    Using LPC bridge 8086:27b9 at 0000:1f.00
    Current BUC.TS=1 - 64kb address ranges at 0xFFFE0000 and 0xFFFF0000 are swapped

* If the flash was **NOT** completed correctly, *do not turn off the computer, and run the following command to restore the original BIOS.*

    sudo ./flashrom -p internal -w factory.rom

4. If you are sure that the flash was completed correctly, fully turn off the computer (don't just restart). Then remove and reinsert the battery.

5. Your computer will boot into a basic version of coreboot now, but you're not done yet. We will need to flash the full version of `coreboot` in the next section.

* [Coreboot Mailing List - Peter Stuge: Flashrom failures can be expected](http://www.flashrom.org/pipermail/flashrom/2012-April/009124.html)

#### Recovering from a bad flash (First Flash Only)

> **Note:** This unbrick method only works on the first flash of Coreboot, when `bucts 1` is enabled; since the factory BIOS still exists.  
> After Coreboot is flashed for the second time, the factory BIOS will be overwritten, so this method wouldn't work anymore.

If `flashrom` was misconfigured or the coreboot ROM is corrupt, and you turned off the computer, it will be bricked upon startup. 

Thankfully, the factory BIOS has not been fully overwritten yet. By unplugging the CMOS battery, `bucts 1` will be unset, and the computer will boot normally again.

1. Unplug the CMOS battery for about 10 seconds. The CMOS battery is found under the keyboard.
2. After the CMOS battery has been removed, the factory BIOS will boot again.
3. Press the ThinkVantage button once you see the ThinkPad bootsplash.
4. Wait a while, and there will be an error informing you to set the correct date.
5. Press Enter to continue and set a correct date and time in the BIOS
6. Save your changes and restart. Wait for the BIOS to boot the hard drive. (this will take longer than normal)
7. Return to your Linux OS and go back to the beginning of this guide.

> **Tip:** One of the most common issues is failing to set `.write` - `spi_chip_write_1` in `flashchips.c`, so fix that, rebuild flashrom with `make`, and try again.

### Flash Coreboot a Second Time

After the first flash has succeeded, the last 64K of the BIOS flashchip will now be writable. Flash Coreboot a second time to complete the installation.

We recommend using Libreboot's binary `coreboot.rom` for a second flash.

1. Download a fresh copy of the `flashrom` source code using this command:

    svn co svn://flashrom.org/flashrom/trunk flashrom
    cd flashrom
    make

> **Tip:** Keep the first modified `flashrom` somewhere safe, (in case you ever want to install coreboot on another fresh X60) but don't use it on this computer any longer.

2. Compile `flashrom` using `make`:

    make

3. Run this `flashrom` command, which will now successfully overwrite the entire flash chip, including the last 64k that were write protected with the factory BIOS. 

    sudo ./flashrom -p internal -w coreboot.rom

> Note: This time, `flashrom` will run without errors and verify itself successfully.

4. Finally, set `bucts` back to `0`. (If you are on your third flash and so on, `bucts` is already set to `0`, skip this step) Bucts is no longer necessary after this point, now that Coreboot has been flashed to the entire chip.

    sudo ./bucts 0

5. Coreboot has now been successfully installed. Now you can just repeat the steps in **Flash Coreboot a Second Time** whenever you want to update Coreboot, or install Libreboot.

## Libreboot

Libreboot, the fork of Coreboot best supported on the X60 and FSF approved, eschews all binary blobs to create a fully free BIOS. 

While it does have a few compromises, we strongly recommend using Libreboot, as it is the best maintained fork of Coreboot on the X60.

### Libreboot Workarounds

Since Libreboot abandons all proprietary blobs, there are a few issues with it compared to Coreboot:

* Most of the critical function keys work now, sound buttons as well
* Suspend to RAM works, but Hibernate (suspend to disk) requires a few modifications
* The kernel argument `processor.max_cstate=2` is required to keep the system running cool, since the CPU scaling in Libreboot does not work well without binary blobs
* Brightness is stuck at 100% and cannot be set lower with function key combo. Currently no one is working on fixing this.

### Working with Libreboot's GRUB2 Payload

Libreboot's default configuration has GRUB2 installed onto the BIOS chip, which can be a new experience.

(copy from Libreboot README)

#### Booting from LiveUSBs with GRUB2

Since GRUB2 on Coreboot does not work like a traditional BIOS, special steps need to be taken to boot typical LiveUSB disks.

Try the `Parse ISOLINUX menu (USB)` option first. It will usually autodetect the ISOLINUX menu and things will work out.

If not, you will have to use SeaBIOS; probably as a GRUB2 payload.

* [John Lewis - Boot LiveUSB from GRUB2 Coreboot Payload](http://johnlewis.ie/booting-fedora-19-live-usb-from-within-grub2/)
* [LiveUSBs with GRUB2](http://forums.debian.net/viewtopic.php?f=20&t=111322)

### Compile Coreboot with GRUB2 as SeaBIOS payload

SeaBIOS is an open source BIOS that works exactly like the typical old legacy BIOS. Since SeaBIOS is guaranteed and tested to work, we strongly recommend using it.

First, build Libreboot with SeaBIOS as shown above. Then, compile GRUB2.

Next, use these commands to add GRUB2 as a payload of SeaBIOS.

    build/cbfstool build/coreboot.rom add-payload -n img/grub2 -f grub2.elf -t raw
    build/cbfstool build/coreboot.rom print

* [Coreboot Wiki - GRUB2: Combining with Coreboot](http://www.coreboot.org/GRUB2#combining_with_coreboot)


### Testing GRUB2 Payload with QEMU

If you ever recompile GRUB2 and plan to use it as the Coreboot payload, it is strongly recommended that you thoroughly test it using the QEMU emulator. 

If GRUB2 doesn't work, your BIOS will be bricked, and external hardware flashing is required for recovery.

* [Coreboot Wiki - QEMU](http://www.coreboot.org/QEMU)

### Compile Coreboot with SeaBIOS as GRUB2 Payload

To get traditional liveUSB boot support and the option of booting Windows, you will want to add SeaBIOS as a GRUB2 Payload (not a Coreboot payload)

(needs some way of testing the coreboot image on QEMU, so we can avoid bricks)

Add this to `grub.cfg`

    menuentry 'SeaBios' {
	    set root='memdisk'
	    echo    'Loading SeaBios ...'
	    chainloader /bios.bin.elf
    }

Now put the following into a bash script, make it executable, and run it:

    #!/bin/sh
    rm -f grub2-x60.elf
    #copy the payloads you want
    cp ../../seabios-x60/out/bios.bin.elf ./memdisk/
    cp ../../coreboot-qemu/payloads/nvramcui/nvramcui.elf ./memdisk/
    cp ../../coreboot-x60/payloads/coreinfo/build/coreinfo.elf ./memdisk/
    cp ../../memtest86+-4.20/memtest ./memdisk/memtest.elf
    #and some files
    cp ../../coreboot-x60/bootsplash.jpg  ./memdisk/
    cd memdisk
    grub-mkstandalone -O i386-coreboot -o ../grub2-x60.elf $(find -type f)
    echo "--RESULT--"
    ls -l -h ../grub2-x60.elf

Afterwards, recompile Coreboot using the generated `grub2-x60.elf` .

* [Payloads to GRUB2](http://www.coreboot.org/GRUB2#Payloads)

#### Set New SeaBIOS Bootsplash

You can add your own SeaBIOS bootsplash using cbfstool. Make sure the image is named `background.png` or `background.jpg`, and is `1024x768`.

Then use cbfstool to embed the image in a compiled `coreboot.rom`.

    ./cbfstool coreboot.rom add -f background.png -n background.png -t bootsplash

Finally, confirm that they exist inside the rom:

    ./cbfstool coreboot.rom print

### (FSF-Approved Linux) Trisquel Linux

The ThinkPad X60 with Libreboot is one of the few laptops capable of eschewing all nonfree software, so to complete FSF certification, try out Trisquel Linux.

### Replace the WiFi card with a Free Alternative

Since the default Intel wireless chipset requires the use of binary blobs, it is advised to replace it with a Wireless-N WiFi card that can use free software drivers, such as Atheros cards. You might even have one in a broken laptop that's lying around; but if not, they're usually only $15. 

And don't worry about the mPCI whitelist; Coreboot has no such restrictions.

Make sure that the replacement card you get is full height, or at least has a half size adapter.

* Atheros - Atheros cards are all supported out of the box with the open source `ath9k` driver.
  * Atheros AR5B91 (abgn) - A full size card that can be cheaply procured.

## Sources

* [X60 Status](http://www.coreboot.org/Board:lenovo/x60)
* [Build Coreboot](http://www.coreboot.org/Build_HOWTO)
  * [GNUToo's Custom Coreboot](http://www.coreboot.org/User:GNUtoo#Lenovo_x60)
  * [Alternate Coreboot GRUB installation Guide](http://www.coreboot.org/User:Fchmmr)]
* [Flash Coreboot on X60](http://www.coreboot.org/Board:lenovo/x60/Installation)
* [Install and maintain Libreboot](http://libreboot.org/)

### Discussion

* [Coreboot Mailing List - Help! Need to find BIOS chip on X60](http://list-archives.org/2013/01/29/coreboot-coreboot-org/help-finding-bios-chip-on-thinkpad-x60/f/3318873537)