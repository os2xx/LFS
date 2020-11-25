---
---

[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](index.md)
[NEXT](LFS-02.md)

# Set a Blank VirtualBox Guest of LFS

<br>
* Create LFS-ORIG.ova

  * Base Memory Maximum, eg. 8096 MB

  * **Boot Order [Optical, Harddisk]**

  * Processors (Core) Maximum, eg. 6

  * Delete IDE Controller

  * Main SATA Disk: 16 GB.

  * LFS SATA Disk: 32 GB.

  * SSH: TCP Port Forwading 127.0.0.1:6024 (host) to 10.0.2.15:22 (guest)

  * Export as LFS-ORIG.ova

<br>
* Import a blank OVA file

  * Rename to LFS-ORIG

<img src="pictures/LFS-A01.jpg" width="960">
<br>

<br>
* Create LFS-ORIG

  * Settings

<img src="pictures/LFS-A02.jpg" width="960">
<br>

<br>
* Settings:General:Basic

  * Name: LFS-ORIG

  * Type: Linux

  * Version: Debian (64 bit)

<img src="pictures/LFS-A03.jpg" width="960">
<br>

<br>
* Settings:General:Advanced

<img src="pictures/LFS-A04.jpg" width="960">
<br>

<br>
* Settings:System:Motherboard

  * Base Memory (Maximum) eg. 8096 MB

  * **Boot Order [Optical, Harddisk]**

<img src="pictures/LFS-A05.jpg" width="960">
<br>

<br>
* Settings:System:Processor

  * Processors (Core Maximum), eg. 6

<img src="pictures/LFS-A06.jpg" width="960">
<br>

<br>
* Settings:Storage

  * Delete IDE Controller

<img src="pictures/LFS-A07.jpg" width="960">
<br>

<br>
* Settings:Storage:Devices:Add SATA Optical Drive (ISO)

  * Add latest Debian NetInst ISO (subject to change): debian-10.6.0-amd64-netinst.iso

<img src="pictures/LFS-A08.jpg" width="960">
<br>

<img src="pictures/LFS-A09.jpg" width="960">
<br>

<br>
* Settings:Storage:Devices

<img src="pictures/LFS-A10.jpg" width="960">
<br>

<img src="pictures/LFS-A11.jpg" width="960">
<br>

<br>
* Settings:Network:Adaptor 1

  * SSH: TCP Port Forwading 127.0.0.1:6024 (host) to 10.0.2.15:22 (guest)

<img src="pictures/LFS-A13.jpg" width="960">
<br>

<img src="pictures/LFS-A12.jpg" width="960">
<br>

* Storage

  * Optical Drive: debian-10.6.0-amd64-netinst.iso (349 MB)

  * Main SATA Disk: 16 GB.

  * LFS SATA Disk: 32 GB.

<img src="pictures/LFS-A14.jpg" width="960">
<br>

<br>
* Export as LFS-ORIG.ova

<br>
#### ENDOFPAGE
[HOME](index.md)
[ABOUT](README.md)
[WEB](https://lfs.vlsm.org/)
[GITHUB](https://github.com/OSP4DISS/lfs/)
[TOP](#)
[BOTTOM](#endofpage)
[PREV](index.md)
[NEXT](LFS-02.md)
<br>

