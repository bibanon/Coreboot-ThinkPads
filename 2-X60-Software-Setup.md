Flashing Coreboot on the X60 for the first time involves a lot of hard work. However, Coreboot on the X60 can be installed entirely through software, no special hardware is necessary.

A Linux distribution is required, and Ubuntu/Debian based distros are recommended. Make sure it is installed on the hard drive before continuing.

## Install Dependencies (for Ubuntu/Debian based systems)

> **Note:** While these commands are specifically designed for Ubuntu/Debian based distros, the dependencies usually have equivalent packages on other distros, with different names.

Subversion and Git are required to download source code:

    sudo apt-get -y install subversion git

`build-essential` contains tools used for compiling source code:

    sudo apt-get -y install build-essential

These packages are required for building Coreboot:

    sudo apt-get -y install libncurses5-dev doxygen iasl gdb flex bison

These packages are required for building `flashrom`:

    sudo apt-get -y install libpci-dev pciutils zlib1g-dev libftdi-dev

These packages are required for building GRUB2:

    sudo apt-get install bison libopts25 libselinux1-dev autogen m4 autoconf help2man libopts25-dev flex libfont-freetype-perl automake autotools-dev libfreetype6-dev texinfo

Unifont is required to display text in GRUB2:

    sudo apt-get -y install ttf-unifont 

## Download Libreboot Source Code

The Libreboot project has a convenient source code package with all the tools needed to flash Coreboot, as well as a prebuilt coreboot image.

[Download the package from the Libreboot site.](http://libreboot.org/#releases) Make sure to *Download Source*.

Then, extract the archive, which will give you an `X60_source` folder.

Enter the `X60_source` folder, it contains everything that you need.