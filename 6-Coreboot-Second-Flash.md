After the first flash has succeeded, the last 64K of the BIOS flashchip will now be writable. Flash Coreboot a second time to complete the installation.

We recommend using Libreboot's binary `coreboot.rom` for a second flash.

## Flash Coreboot a Second Time

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