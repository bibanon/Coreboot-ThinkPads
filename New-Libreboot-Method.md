The select few ThinkPads with Libreboot(a fork of Coreboot) installed are one of the few [endorsed by the Free Software Foundation](http://www.fsf.org/news/gluglug-x60-laptop-now-certified-to-respect-your-freedom). Once Libreboot is installed and the original Intel wifi card is replaced, no proprietary software of any kind is utilized on the computer.

The BIOS is the last remaining piece of proprietary software required to be installed on an X86 computer. Coreboot replaces the BIOS with an partially open-source replacement; however, many critical features still use proprietary blobs extracted from the original BIOSes. 

Libreboot goes further by reverse-engineering these proprietary blobs, and writing replacements for them, creating a fully open source BIOS.

The importance of using of fully open source software on all components of a computer has become dreadfully apparent, now that it is public knowledge that the NSA installs backdoors onto popular proprietary software. This is not just an issue affecting the "enemies" of the US Government; it is a critical, unpatched security breach that can be exploited by any group that discovers it.

Although installing Coreboot is only a matter of a few software steps, the official Coreboot guide lacks detailed information about patching flashrom, and the official Libreboot documentation can be mindboggling to read. This guide is designed to clarify every step for the general public, so that hobbyists can install Libreboot on their own ThinkPad laptops (an even a Macbook 2,1).

## Purchasing Preflashed Libreboot Laptops

Because of the significant danger involved in flashing Libreboot laptops, many users might prefer to purchase a preflashed one.

These vendors offer

* Gluglug
  * [ThinkPad X60](http://shop.gluglug.org.uk/product/ibm-lenovo-thinkpad-x60-coreboot/) 198 Pounds - Pretty expensive for this sort of laptop, but currently it's the only choice around.

## Supported Laptops

A larger list of supported laptops can be found at the [Libreboot website.](http://libreboot.org/)

* ThinkPad X60 Series
* ThinkPad T60/T60p Series with Intel GPU
  * T60 systems with **XGA (1024x768) screens** are NOT supported for some reason. But you'd definitely want to upgrade them to SXGA+, UXGA, or even QXGA Flexview displays anyway. Follow the **T60 Screen Upgrade Guide** for more details.
  * T60 systems with **ATI discrete GPU** are NOT supported by Libreboot, since they require proprietary blobs to display graphics. You could still try and install Coreboot on them, by extracting proprietary VGA code from the Lenovo BIOS. See **T60 ATI GPU Coreboot** for more details.
  * **Widescreen T60 laptops** have never been tested with Libreboot before. We assume that they might work; but who knows, maybe the screens might not display anything just like the XGA models. Install at your own risk.
* MacBook 2,1

Make sure you identify the exact model of the laptop you have! This will be important later.

## Disclaimer

* This guide may mention some nonfree software (such as Ubuntu or Debian, *oh noes*).
* It will recommend that users back up their proprietary Lenovo BIOS.
* This guide prefers ready-made binaries from the Libreboot website, rather than fresh source code builds, for safety reasons.
* It even has optional tutorials on how to get ATI GPU systems working with Coreboot. 

If you take moral issue with this practice, please don't complain to us, just stick to [the official Libreboot Documentation](http://libreboot.org/docs/). 

The authors of this guide would definitely prefer that users run free software as much as possible.

But our policy is that if users choose to run proprietary blobs on their system to restore functionality to certain features, let them be free to do what works for them.

## Preperation

* For best results, install a **Debian-based Linux distro.** The Libreboot team recommends **Trisquel 6** (32-bit), but plain-old Ubuntu or Debian would work.
* Replace the Intel mPCI wifi card with an **Atheros Wireless-N mPCI card** ($10-15). The Intel card requires proprietary blobs, and only supports Wireless G; so you might as well upgrade anyway.

### Download Libreboot

1. [Download the Libreboot Binaries](http://libreboot.org/#releases).
2. Open a Terminal and navigate to the Downloads folder (or wherever else)
3. Extract the Libreboot folder:

    tar -xvf libreboot_bin.tar.gz

4. Navigate to the `libreboot_bin` folder:

    cd libreboot_bin

### Install Dependencies (DEB-based Distros)

The `flashrom` and `bucts` programs require a few dependencies.

Use the following commands for DEB-based distros (Trisquel, Ubuntu, Debian, etc):

    $ sudo ./builddeb-flashrom
    $ sudo ./builddeb-bucts

Other Linux distros will need to find the corresponding dependencies on their own.

### Find the Right ROM

Under the `bin/` folder in `libreboot_bin/`, there are a multitude of Libreboot ROMs sorted by motherboard.

Choose the ROM with your laptop's keyboard layout (US or UK, QWERTY or DVORAK). For ThinkPads, choose a `serial` ROM if dock and serial port support is needed.

* `bin/x60/` - ThinkPad X60/X60s
* `bin/x60t/` - ThinkPad X60 Tablet
* `bin/t60/` - ThinkPad T60 Series with Intel GPU (14" 4:3, 15" 4:3)
* `bin/macbook21/` - MacBook2,1

Once you know which ROM to use, remember it's directory path for the next step (ex. `bin/x60/libreboot_serial_usqwerty.rom` )

Check [the ROMs section](http://www.libreboot.org/docs/#rom) from the official Libreboot Documentation for the latest list.

## Flashing Libreboot on Lenovo ThinkPad BIOSes

> **Note:** For a detailed explanation of how this process works, see the file `Software-Flashing-Process-i945.md` .  

> **Note:** The BIOS chip no longer needs to be visually identified before installation. Libreboot now offers two prepatched `flashrom` binaries, `flashrom_lenovobios_sst` and `flashrom_lenovobios_macronix`.

If you are flashing Libreboot for the first time, on an unmodified ThinkPad running Lenovo's BIOS, you will need to follow this special process.

---

* Source: [Libreboot Documentation - How to flash Libreboot on a Lenovo BIOS](http://www.libreboot.org/docs/#flashrom_lenovobios)

### Back up Official Lenovo BIOS

It is strongly recommended to back up the BIOS image from the motherboard, just in case the Lenovo BIOS needs to be restored. 

This BIOS image is **unique to every motherboard**. It will be impossible to restore the Lenovo BIOS once it is lost. Do not use another laptop's BIOS image.

1. From the `libreboot_bin/` folder, enter the `flashrom/` folder.

    cd flashrom

2. Run *both* of these commands to backup the BIOS to `factory.bin` (don't panic, nothing is being installed):

    sudo ./flashrom_lenovobios_sst -p internal -r factory.bin
    sudo ./flashrom_lenovobios_macronix -p internal -r factory.bin

3. If a `factory.bin` file was created in the `flashrom/` folder, the Lenovo BIOS has been backed up successfully. If not, try the commands again. Copy this dump to a safe place.

4. Return to the `libreboot_bin/` folder.

    cd ..

### Libreboot First Flash

1. Run the first flash script for Lenovo BIOSes:

> **Note:** Replace `bin/YOURBOARD/YOURROM` in the command below with the path to the ROM you selected.

    $ sudo ./lenovobios_firstflash bin/YOURBOARD/YOURROM. 

2. Wait for the process to finish. Expect to see "critical errors" during flashing, but don't panic; proceed to the next step to check if the flash ran correctly.

3. The line below is displayed if `bucts 1` was enabled successfully.

> **Warning:** If `BUC.TS=1` was *not enabled*, *do not continue, do not turn off your laptop*; run the flash script again.

    Updated BUC.TS=1 - 64kb address ranges at 0xFFFE0000 and 0xFFFF0000 are swapped.

4. The following "errors" are displayed if `flashrom` installed Libreboot correctly. The output must be very similar (later versions of `flashrom` may have minor differences).

> **Warning:** If the "errors" do not match, *do not continue, do not turn off your laptop*. Run the script again. If the output still doesn't match, something is wrong; reinstall the `factory.bin` image.

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

5. **If the "errors" closely match the lines above**, shut down the laptop (don't restart).

6. Wait a few seconds, and then boot. Libreboot will start up.

7. Use the `Search for GRUB configuration on local storage` option if the normal menu options don't work.

8. After booting into Linux, proceed to **Libreboot Second Flash**.

> **Note:** If you boot and you see nothing, try turning up the backlight `Fn+Home`. For ThinkPad X60 models, if the backlight resets to zero when turning it up while at max, consult Libreboot's [TFT Brightness Guide.](http://www.libreboot.org/docs/#tft_brightness)  

> **Note:** If the laptop turns on, but doesn't boot at all; and there is absolutely no sounds or sign of life, Libreboot might have been flashed incorrectly. It is still possible to return to the Lenovo BIOS by disabling `bucts 1`. Disassemble the laptop and unplug the CMOS battery for at least 5 seconds. Plug the CMOS back in, reassemble the laptop, and turn it on. The Lenovo BIOS should appear, though it will require you to set the time in BIOS Setup. Boot into Linux, and start over from step 1 immediately.  

> **Note:** If the laptop emits three loud beeps when turned on, it has been fully bricked. A hardware BIOS flasher is required to restore it to working order. We recommend a $40 Bus Pirate and $20 8-pin Pomona Clip from eBay.

---

* Source: [Flashrom Mailing List - ThinkPad X60: Is it safe to reboot with errors in flashrom?](http://thread.gmane.org/gmane.linux.bios.flashrom/575.)

### Libreboot Second Flash

Now that Libreboot has been installed and is up and running, it must be flashed a second time to fully remove the Lenovo BIOS.

1. Open a Terminal and navigate to the `libreboot_bin` directory.
2. Run the following command to flash Libreboot a second time.

> **Note:** Replace `bin/YOURBOARD/YOURROM` in the command below with the path to the ROM you selected.

    sudo ./lenovobios_secondflash bin/YOURBOARD/YOURROM

3. The following line will be displayed if `bucts` was set back to `0` again. If it was not set to 0, run the script again.

    Updated BUC.TS=0 - 128kb address range 0xFFFE0000-0xFFFFFFFF is untranslated

4. The following should also be displayed, without any errors:

    Verifying flash... VERIFIED.

5. Shut down again, wait a few seconds, and then boot. Libreboot has been successfully installed.

---

* Source: [Libreboot Documentation - Installing Libreboot a Second Time on ThinkPad X60/T60 systems](http://www.libreboot.org/docs/#flashrom)

## Flashing Libreboot on an Apple MacBook 2,1

(stuff)

> I assume that you just use the `./flash` script; but we have to be sure.

## Custom Built Coreboot ROMs

Make your own coreboot roms, if you want to customize GRUB, or use SeaBIOS. Be sure to follow this procedure correctly, and test everything frequently; the laptop will be effectively bricked if the Coreboot ROM or the payload doesn't work.

## Flashing Coreboot on T60 Systems with ATI GPU

Unfortunately, ThinkPad T60 laptops with an ATI GPU require proprietary VGA blobs, extracted from the Lenovo BIOS, to function.

If you do not want any proprietary blobs on your T60, you'll just have to replace the motherboard with an Intel board. Not to worry, though; T60 Intel GPU boards go for $10-30.

### Back up Official Lenovo BIOS

The Lenovo BIOS must be backed up, to extract the proprietary VGA blobs needed that allow Coreboot to use the ATI GPU.

This BIOS image is **unique to every motherboard**. It will be impossible to restore the Lenovo BIOS once it is lost. Do not use another laptop's BIOS image.

The factory image also comes in handy just in case the Lenovo BIOS needs to be restored.

1. From the `libreboot_bin/` folder, enter the `flashrom/` folder.

    cd flashrom

2. Run *both* of these commands to backup the BIOS to `factory.bin` (don't panic, nothing is being installed):

    sudo ./flashrom_lenovobios_sst -p internal -r factory.bin
    sudo ./flashrom_lenovobios_macronix -p internal -r factory.bin

3. If a `factory.bin` file was created in the `flashrom/` folder, the Lenovo BIOS has been backed up successfully. If not, try the commands again.
4. Return to the `libreboot_bin/` folder.

    cd ..

### Extract Proprietary VGA Blob from Lenovo BIOS

### Build Coreboot with the Blob

### Flash Libreboot

Refer to `Flashing Libreboot on Lenovo ThinkPad BIOSes/Libreboot First Flash`, but use your custom ROM instead of the Libreboot ROMs.



## T60 Screen Upgrade

For some reason, Libreboot does not support XGA (1024x768) screens.

However, it supports any resolution higher than XGA, such as SXGA+, UXGA, even QXGA.

And it's definitely worth the $50 to upgrade to a higher resolution anyway; especially if you have a 15" T60 system, capable of using high resolution Flexview IPS displays.

### 14.1" 4:3 T60 SXGA+ Upgrade

On the 14.1" 4:3 (non-widescreen) T60 models, there is an SXGA+ screen. Unfortunately it is not an Flexview IPS screen, but the expansive resolution is a plus.

### 15" 4:3 T60 Flexview IPS Upgrade

The 15" 4:3 (non-widescreen) T60 models are especially desirable, since they can use high resolution 4:3 Flexview IPS screens, which are critically acclaimed by ThinkPad fans for their great viewing angles, 4:3 format tuned for document reading, and high resolution rarely seen in today's budget laptops.

Here are the parts numbers for the Flexview screens:

* SXGA+
* UXGA ($50)

If upgrading from an XGA screen, you must also replace the inverter (just under the panel, in the lid):

* The Inverter ($15) - There are mixed reports about whether the XGA inverter works or doesn't work with the Flexview panels. But for now, replace them with these known working ones:

## Libreboot on Apple Macbook 2,1

* **Compatible Parts Numbers:** Apple MacBook2,1 (MA699LL/A, MA701LL/A, MB061LL/A, MA700LL/A, MB063LL/A, MB062LL/A)
  * Source: Libreboot Website

One particularly interesting Libreboot system is an **Apple MacBook 2,1**. It goes for around $100-200 complete on eBay, since it has been completely abandoned by Apple.

It has a very nice 13.3 inch IPS 1280x800 screen, and even a Core 2 Duo processor.

Documentation is very sparse on this system, and I'm finding it difficult to know what works, what doesn't work, and how to safely flash. But the Libreboot developers are working hard on getting it running, and everything "should" work.

* [H-Node - Apple MacBook 2,1 Coreboot Description](http://h-node.org/notebooks/view/en/1135/MacBook2-1---Mac-F4208CAA)