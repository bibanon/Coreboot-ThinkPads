

### Method 2 - Verbose Search

> **Note:** This method is kept only for reference, since it was the method used to discover the `model_id` values of the BIOS chips.

1. First, test if the BIOS Chip is `SST25VF016B`. Patch the BIOS Chip for `SST25VF016B` and run this command to try and dump the factory BIOS. Don't worry, nothing is being flashed.

    sudo ./flashrom -p internal -r factory.bin -V | grep "0xbf"

2. If your BIOS Chip is a `SST25VF016B`, something similar to the following will be displayed (ignore extraneous warning messages):

    Probing for SST SST25LF040A, 512 kB: probe_spi_res2: id1 0xbf, id2 0x41
    Probing for SST SST25VF016B.RES2, 2048 kB: probe_spi_res2: id1 0xbf, id2 0x41
    probe_spi_res1: id 0xbf

---

1. If nothing was displayed in the previous test, patch the BIOS Chip for `MX25l1605D` , and run this command try and dump the factory BIOS:

    sudo ./flashrom -p internal -V | grep "0x14"

2. If your BIOS Chip is a `MX25l1605D`, something similar to the following will be displayed (look for for the line `probe_spi_res1: id 0x14`):

    0x14: 0x00000000 (SPID1+4)
    Probing for PMC Pm25LV512(A), 64 kB: probe_spi_res3: id1 0x1414, id2 0x14
    Probing for SST SST25LF080(A), 1024 kB: probe_spi_res2: id1 0x14, id2 0x14
    probe_spi_res1: id 0x14

4. If your BIOS chip is neither of these types, the output will look very different from the three shown above, and a `factory.bin` may or may not be created. If the `factory.bin` was created, make note of the output, and continue with caution. If it was not, do not continue; you will have to visually identify your BIOS chip.