Warnings
========

.. note ::

    This feature is now of historical interest only. Since kernel 4.15 and
    with TLP's current SATA ALPM defaults, the underlying problems no longer
    occur.

Which types of warnings does `tlp-stat -w` check for?
-----------------------------------------------------
Currently the command checks the kernel log for **SATA** disk related errors
possibly caused by the settings for :ref:`ALPM <set-disks-alpm>`.
Modern NVMe SSDs are **not affected**.

What should I do if warnings are shown?
---------------------------------------
TLP's historic (*until version 1.0*) default setting on battery `SATA_LINKPWR_ON_BAT=min_power`
is optimimized for maximum power saving, but it can cause kernel warnings for SATA disks.

Solution: disable your individual settings and use TLP's current default `med_power_with_dipm`
for :ref:`set-disks-alpm`.

What to do if the warnings do not disappear after changing the settings?
------------------------------------------------------------------------
* In case there are only 1 or 2 warnings immediately after system boot or a change
  of power source: users reported it is safe to just ignore them
* In case of a high warning count: try to use a newer kernel version
* Disable `SATA_LINKPWR_ON_PRF/BAL/SAV` completely

What is the cause for these errors?
-----------------------------------
Candidates are:

* Poor implementation of ALPM in the disk's firmware or SATA controller or
  System BIOS
* Issues between the controller and the disk
* Kernel driver issues
