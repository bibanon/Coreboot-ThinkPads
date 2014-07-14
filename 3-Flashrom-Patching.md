## Patching `flashrom` to work with the BIOS chip

Enter the `flashrom` source code folder under `X60_source`.

Change the corresponding values under `flashchips.c` for your chip model:

* `SST25VF016B`
  * `.probe` - `probe_spi_res1`
  * `.model_id` - `0xbf`
  * `.write` - `spi_chip_write_1`
* `MX25l1605D` - Actual family name in `flashchips.c` is `MX25l1605`
  * `.probe` - `probe_spi_res1`
  * `.model_id` - `0x14`
  * `.write` - `spi_chip_write_1`

> **Note:** At the time of this writing, I actually had to edit ".name = "MX25L1605" instead of ".name = "MX25l1605D" because 605D falls under the 605 family, so 605D does not exist as a stand alone ruleset. However, this may change in future flashrom versions.

### Sources

* [Flashrom Pastebin - Thinkpad R60 Flashrom Output](http://paste.flashrom.org/view.php?id=1454) - Probing for SST SST25VF016B.RES2, 2048 kB: probe_spi_res2: id1 0xbf, id2 0x41
* [Coreboot Mailing List - Invalid OPCODE](http://www.coreboot.org/pipermail/coreboot/2013-June/075962.html) - Allows us to infer that the model_id for `SST25VF016B` is 0xbf
* [Coreboot Mailing List - Bricked Lenovo T60](http://coreboot.org/pipermail/coreboot/2013-June/076027.html)
* [Donderclumpen - Coreboot on Macbook 2,1](http://macbook.donderklumpen.de/coreboot/)] -  Found `id1 0xbf, id2 0x2541` there, which corroborates with the inference from Peter Stuge.
* [SST - SST25VF016B Official Datasheet](http://ww1.microchip.com/downloads/en/DeviceDoc/S71271_04.pdf)

## Building Flashrom

1. Install all the critical build dependencies:

    sudo apt-get install pciutils pciutils-dev zlib1g-dev

2. Run the following commands to download the `flashrom` source code:

    svn co svn://flashrom.org/flashrom/trunk flashrom
    cd flashrom
    make

3. The `flashrom` binary will be placed into the source code folder. You will need to add a `./` before the command, so that the terminal will use the `flashrom` binary in the current folder.

    sudo ./flashrom

## Use `flashrom` to Identify BIOS Chip (experimental)

Below is an experimental method to determine the type of the BIOS chip in the X60 by searching for unique signatures that appear when `flashrom` probes for BIOS chips. (Since it is significantly easier to look at the BIOS chip on the T60, stick to visual identification on that model.)

To the best of our knowledge, there is no reason why this method should not work. But be aware that it has not been fully tested.

### Known Types

* SST25VF016B - Requires patches.
* Macronix MX25l1605D - Requires patches and special arguments.
* Atmel 0711 26DF161 SU -Works out of the box without patches. Usually found only on the T60/T60p. 

### Method

1. First, test if the BIOS Chip is `SST25VF016B`. Patch the BIOS Chip for `SST25VF016B` and run this command to try and dump the factory BIOS. Don't worry, nothing is being flashed.

    sudo ./flashrom -p internal -r factory.bin | grep "0xbf"

2. If your BIOS Chip is a `SST25VF016B`, something similar to the following will be displayed:

    Probing for SST SST25LF040A, 512 kB: probe_spi_res2: id1 0xbf, id2 0x41
    Probing for SST SST25LF080A, 1024 kB: probe_spi_res2: id1 0xbf, id2 0x41
    Probing for SST SST25VF016B.RES2, 2048 kB: probe_spi_res2: id1 0xbf, id2 0x41
    probe_spi_res1: id 0xbf
    probe_spi_res1: id 0xbf
    probe_spi_res1: id 0xbf

---

1. If nothing was displayed in the previous test, patch the BIOS Chip for `MX25l1605D` , and run this command try and dump the factory BIOS:

    sudo ./flashrom -p internal -V | grep "0x14"

2. If your BIOS Chip is a `MX25l1605D`, something similar to the following will be displayed:

    0x14: 0x00000000 (SPID1+4)
    Probing for PMC Pm25LV512(A), 64 kB: probe_spi_res3: id1 0x1414, id2 0x14
    Probing for PMC Pm25LV010, 128 kB: probe_spi_res3: id1 0x1414, id2 0x14
    Probing for SST SST25LF040A, 512 kB: probe_spi_res2: id1 0x14, id2 0x14
    Probing for SST SST25LF080(A), 1024 kB: probe_spi_res2: id1 0x14, id2 0x14
    probe_spi_res1: id 0x14
    probe_spi_res1: id 0x14
    probe_spi_res1: id 0x14
    probe_spi_res1: id 0x14

4. If your BIOS chip is neither of these types, the output will look very different from the three shown above, and a `factory.bin` may or may not be created. If the `factory.bin` was created, make note of the output, and continue with caution. If it was not, do not continue; you will have to visually identify your BIOS chip.

### Sources

* [Flashrom Pastebin - Thinkpad R60 with SST25VF016B Flashrom Output](http://paste.flashrom.org/view.php?id=1454) - Probing for SST SST25VF016B.RES2, 2048 kB: probe_spi_res2: id1 0xbf, id2 0x41
* [Coreboot Mailing List - ThinkPad T60 with Macronix Flashrom Output](http://www.coreboot.org/pipermail/coreboot/2013-June/075961.html)

## Backup your Original BIOS

In case something goes wrong, it is a good idea to keep a backup of the original BIOS software.

    sudo ./flashrom -p internal -r factory.bin

This step is IMPORTANT since the factory BIOS in your machine is tied to your particular system board (or "planar" in IBM FRU terms) with a unique ID not present in factory BIOS updates.