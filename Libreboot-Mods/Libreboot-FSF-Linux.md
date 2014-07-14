## (FSF-Approved Linux) Trisquel GNU/Linux

Trisquel, as a FSF-approved GNU/Linux distribution, only includes Free/Libre/Open-Source applications and libraries. The ThinkPad X60 with Libreboot is one of the few laptops capable of eschewing all nonfree software, and thus has the best support for Trisquel Linux of any laptop around. These FSF-approved OSes only contain free software, avoiding nonfree blobs entirely.

It's worth taking a look at, even if FSF-approved GNU/Linux might be too restrictive for most tastes.

### What Works

* MP3s and many other patented codecs can be played (the players themselves are free software), but you cannot encode them, due to patent restrictions.
  * Encode your music in OGG, or better yet, lossless FLAC.
* You can watch or rip DVDs using [the open-source `libdvdcss`.](http://trisquel.info/en/wiki/enable-dvd-playback) However, in some countries, laws such as the US's DMCA may make it illegal to use this library (but the library itself is not proprietary software). [Follow this guide to install support.](https://www.thinkpenguin.com/gnu-linux/playing-digitally-restricted-dvds-gnulinux)

### Restrictions 

* No Adobe Flash. Thankfully, since iPhones and iPads can't run flash either, websites have been migrating away from the dated technology.
  * Enable [the HTML5 player on YouTube](http://youtube.com/html5) (it works for almost all videos)
  * If a site you need still uses Flash, try to find an iPad version of the site, which will use HTML5.
  * For anything else that requires Flash, try Gnash or Lightspark.
  * [More information...](http://trisquel.info/en/wiki/play-videos-without-using-flash)
* [Here is a list](http://trisquel.info/en/wiki/software-does-not-respect-free-system-distribution-guidelines) of other software that is proprietary and cannot be included in Trisquel. There's not as many as you might think!

### Useful apps

* **ABrowser** - Debranded version of Firefox, renamed to A Web Browser (ABrowser); otherwise, it's exactly the same. The Add-ons Manager will direct you to Trisquel's repository of known free software add-ons (there are quite a lot of them), rather than the often proprietary addons that Firefox recommends.

### Replace the WiFi card with a Free Alternative

The internal Intel wireless chipset on the ThinkPad X60 requires the use of non-free binary blobs, and there is no known free software driver. It also uses Wireless-G, rather than Wireless-N, so it's in your interest to replace it anyway.

Therefore, the first step in installing Trisquel is to upgrade the card with a Wireless-N WiFi card that can use free software drivers, such as **Atheros** cards. There might even be one in a broken laptop from 2006-2011 lying around; but if not, these cards can be found on Amazon for only $15.

And don't worry about the WiFi card whitelist; Coreboot removes any such restrictions.

Make sure that the replacement card is full height, or at least has a half size adapter.

* Atheros - Atheros cards are all supported out of the box with the open source `ath9k` driver.
  * Atheros AR5B91 (abgn) - A full size card that can be cheaply procured.

After you recieve the Atheros card, follow [the official Lenovo guide to replace the wifi card on your laptop.](http://support.lenovo.com/en_US/product-and-parts/detail.page?&LegacyDocID=MIGR-64322)

### Alternative Desktop Environments

By default, Trisquel comes with a very nice looking LXDE setup, which is great for low end systems. However, it's not for everyone; some might want a more fully featured Desktop Environment, such as GNOME or KDE.

The packages are generally the same as Ubuntu, just that any proprietary Linux apps (which are few) have been removed.

* GNOME3 `gnome` - This will install the basic components of GNOME.
* KDE `kde-standard` - This will install the entire KDE desktop.
  * We recommend disabling Nepomuk File Indexing.

After installing a desktop environment, log out. When logging in again, select your chosen desktop environment in the **Session**.