The ThinkPad X60 with Libreboot (a fork of Coreboot) installed is one of the few laptops [endorsed by the Free Software Foundation](http://www.fsf.org/news/gluglug-x60-laptop-now-certified-to-respect-your-freedom). Once Libreboot is installed and the original Intel wifi card is replaced, no proprietary software of any kind is utilized on the computer.

The BIOS is the last remaining piece of proprietary software required to be installed on an X86 computer. Coreboot replaces the BIOS with an partially open-source replacement; however, many critical features still use proprietary blobs extracted from the original BIOSes. 

Libreboot goes further by reverse-engineering these proprietary blobs, and writing replacements for them, creating a fully open source BIOS.

The importance of using of fully open source software on all components of a computer has become dreadfully apparent, now that it is public knowledge that the NSA installs backdoors onto popular proprietary software. This is not just an issue affecting the "enemies" of the US Government; it is a critical, unpatched security breach that can be exploited by any group that discovers it.

Although installing Coreboot is only a matter of a few software steps, the official guide is very hard to follow, and Gluglug hasn't released their own documentation publicly. This guide is designed to clarify every step, so that hobbyists can install Libreboot on their own ThinkPad X60 laptops.

## Steps

The full list of steps is shown below:

(You need Linux installed on your laptop's hard drive before proceeding)

1. Fully disassemble the laptop, until you can flip over the motherboard.
  * Dust off the fan (recommended)
  * Reapply Thermal Paste (recommended)
2. Visually identify the BIOS Chip using a magnifying glass.
  * Download Libreboot Source Code
3. Patch flashrom to work with the BIOS Chip through SPI interfacing.
4. Build flashrom
5. Backup your original BIOS
6. Build Coreboot with SeaBIOS (optional)
7. Modify Coreboot ROM for first 64K of BIOS Chip
8. Coreboot First time flash
  * Recover from Bucts bad flash
9. Coreboot Second time Flash
  * Boot LiveUSB from Libreboot GRUB2 Payload
10. Libreboot Mods (optional)
  * Compile Libreboot with SeaBIOS (optional)
  * Install Trisquel Linux (Optional)

## Supported Models

All ThinkPad X60 variants, from the X60, X60s, X60 Tablet are supported. (though the wacom pen lacks support, it shouldn't be too hard to set up) 

The ThinkPad T60 is in the process of joining the X60 as [an officially supported platform for Libreboot.](http://trisquel.info/en/forum/best-free-laptop); a godsend, as one with Intel graphics and QXGA resolution (2056x1536) makes it the best Libre laptop you can buy. The R60 uses a similar method.

## The Best ThinkPad X60 Variant

There are three basic variants of the X60, all of which support Coreboot.

The best version to get is the **X60 Tablet**. While Coreboot currently lacks support for the tablet pen (may be fixed later), that model includes an XGA IPS Flexview screen by default. (if you're lucky or can shell out $200, you might be able to install an SXGA+ screen from an X61 Tablet)

Flexview screens are almost equal in quality to today's smartphone screens, with vivid colors and multiple viewing angles. They are far better than the terrible TN panels that are used in the X60 and X60s, which are plagued with dim lighting, muted colors, and washing out if the screen is viewed from the wrong angle.

---

However, the X60 Tablet comes with an ULV low power Core Duo processor, rather than a full mobile processor (but an SSD and more RAM would probably give more speed increases). Not to mention that it is worth $100-200, rather than $50-130 for the more common X60/X60s.

If that extra CPU power is essential to you (which it probably isn't), or you can't really justify paying $100-200 for a computer from 2006, get the X60/X60s.

---

Gluglug also sells ThinkPad X60 laptops with Libreboot and Trisquel preinstalled (or some other FSF-approved version of GNU/Linux). It also comes with an Atheros WiFi card, which is supported by open-source drivers. This is the option to take if BIOS flashing is a bit overwhelming for you.

The downside is that they only sell X60/X60s laptops, not the rarer X60 Tablet.

## ThinkPad T60/X60 Battery Recall

If your battery is one of the models being recalled by Lenovo, [as of 2013](http://forums.lenovo.com/t5/T61-and-prior-T-series-ThinkPad/Thinkpad-Battery-Recall-Status/td-p/1226259), you can still [request a free genuine replacement using their automated battery checker.](http://support.lenovo.com/en_US/detail.page?LegacyDocID=BATT-LENOVO) The recall is in effect regardless if your laptop is still under warranty. In fact, the new battery will even have a one year warranty on it.