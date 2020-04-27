Warnings
========

Which types of warnings does `tlp-stat -w` check for?
-----------------------------------------------------
Currently the command checks the kernel log for errors possibly caused by the
settings for :ref:`ALPM <set-disks-alpm>`.

What should I do if warnings are shown?
---------------------------------------
TLP's former [until 1.0] default setting on battery `SATA_LINKPWR_ON_BAT=min_power`
is optimimized for maximum power saving. Try less aggressive values: with
kernel ≥ 4.15 use ::

    SATA_LINKPWR_ON_BAT=med_power_with_dipm

for kernel < 4.15 use ::

    SATA_LINKPWR_ON_BAT=medium_power

or use ::

    SATA_LINKPWR_ON_BAT=max_performance

or disable the setting completely – see :ref:`set-disks-alpm`.

Don't forget to reboot before checking again.


.. note::

    * The same applies to `SATA_LINKPWR_ON_AC`.
    * The difference in power consumption between `min_power` and `max_performance`
      may amount to more than 1 W depending on your hardware.

What to do if the warnings do not disappear after changing the settings?
------------------------------------------------------------------------
* In case there are only 1 or 2 warnings immediately after system boot or a change
  of power source: users reported it is safe to just ignore them.
* In case of a high warning count: try to use a newer kernel version.

What is the cause for these errors?
-----------------------------------
Candidates are:

* Poor implementation of ALPM in the disk's firmware or SATA controller or
  System BIOS
* Issues between the controller and the disk
* Kernel driver issues
