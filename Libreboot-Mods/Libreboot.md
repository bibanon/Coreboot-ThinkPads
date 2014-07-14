Libreboot, the fork of Coreboot best supported on the X60 and FSF approved, eschews all binary blobs to create a fully free BIOS. 

While it does have a few compromises, we strongly recommend using Libreboot, as it is the best maintained fork of Coreboot on the X60.

### Libreboot Workarounds

Since Libreboot abandons all proprietary blobs, there are a few issues with it compared to Coreboot:

* Most of the critical function keys work now, sound buttons as well
* Suspend to RAM works, but Hibernate (suspend to disk) requires a few modifications
* The kernel argument `processor.max_cstate=2` is required to keep the system running cool, since the CPU scaling in Libreboot does not work well without binary blobs
* Brightness is stuck at 100% and cannot be set lower with function key combo. Currently no one is working on fixing this.