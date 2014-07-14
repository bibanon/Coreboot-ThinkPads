## Compile Coreboot with SeaBIOS (optional)

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

### Sources

* [Trisquel Forums - X60 Flashing Guide is hard to understand](http://trisquel.info/en/forum/coreboot-flashing)
* [Coreboot Wiki - Compiling Coreboot](http://www.coreboot.org/Build_HOWTO)
* [Coreboot Wiki - Fchmmr's Guide to compiling Coreboot for X60 with GRUB2 Payload and no nonfree binaries](http://www.coreboot.org/User:Fchmmr)

## Modify Coreboot ROM

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

### Sources

* [gmane.linux.bios Mailing List - LinuxBIOS on T60](http://comments.gmane.org/gmane.linux.bios/69354) - Peter Stuge's Method of installing Coreboot on the X60.