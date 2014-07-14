> **Note:** These steps are only to be used when Coreboot is flashed for the first time, since only 32K of the 64K BIOS ROM is writable in the beginning.

## Flash Coreboot for the First Time

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

### Sources

* [Coreboot Mailing List - Peter Stuge: Flashrom failures can be expected](http://www.flashrom.org/pipermail/flashrom/2012-April/009124.html)

## Recovering from a bad flash (First Flash Only)

> **Note:** This unbrick method only works on the first flash of Coreboot, when `bucts 1` is enabled; since the factory BIOS still exists.  
> After Coreboot is flashed for the second time, the factory BIOS will be overwritten, so this method wouldn't work anymore.

If `flashrom` was misconfigured or the coreboot ROM was corrupt when the computer was turned off, it will be bricked upon startup. 

Thankfully, the factory BIOS has not been fully overwritten yet. By unplugging the CMOS battery, `bucts 1` will be unset, and the computer will boot normally again.

1. Unplug the CMOS battery for about 10 seconds. The CMOS battery is found under the keyboard.
2. After the CMOS battery has been removed, the factory BIOS will boot again.
3. Press the ThinkVantage button once you see the ThinkPad bootsplash.
4. Wait a while, and there will be an error informing you to set the correct date.
5. Press Enter to continue and set a correct date and time in the BIOS
6. Save your changes and restart. Wait for the BIOS to boot the hard drive. (this will take longer than normal)
7. Return to your Linux OS and go back to the beginning of this guide.

> **Tip:** One of the most common issues is failing to set `.write` - `spi_chip_write_1` in `flashchips.c`, so fix that, rebuild flashrom with `make`, and try again.