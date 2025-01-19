Disks and Controllers
=====================

DISK_DEVICES
------------
::

    DISK_DEVICES="nvme0n1 sda"

Defines the disk devices on which the following parameters act. Multiple
devices are separated with blanks.

Default when unconfigured: "nvme0n1 sda"

When using a 2nd disk e.g. in a :ref:`set-drive-bay`, the assignment of device names
by the kernel (`sda`, `sdb`) can change possibly. In this case it is advisable to do
the device assignment using IDs: ::

    DISK_DEVICES="ata-INTEL_SSDSA2M160G2GC_XZY123456890 ata-HITACHI_HTS541612J9SA00_XZY123456890"

The command :command:`tlp diskid` shows the IDs of all attached disks.

.. _set-disks-apm:

DISK_APM_LEVEL_ON_AC/BAT
------------------------
.. rubric: Advanced Power Management (APM)

::

    DISK_APM_LEVEL_ON_AC="254 254"
    DISK_APM_LEVEL_ON_BAT="128 128"

Set the "Advanced Power Management Level". Possible values range between 1 and 255.

Some selected values are:

* 1 – max power saving / minimum performance – Important: this setting may lead
  to increased disk drive wear and tear because of excessive read-write head
  unloading (recognizable from the clicking noises)
* 128 – compromise between power saving and wear (TLP standard setting on battery)
* 192 – prevents excessive head unloading of some HDDs
* 254 – minimum power saving / max performance (TLP standard setting on AC)
* 255 – disable APM (not supported by some disk models)
* keep – special value to skip this setting for the particular disk (synonym: _)

Default when unconfigured: "254 254" (AC), "128 128" (BAT)

Values for multiple disks are separated with blanks.


DISK_APM_CLASS_DENYLIST
-----------------------
::

    DISK_APM_CLASS_DENYLIST="usb ieee1394"

Exclude disk classes from advanced power management (APM). Possible values:

* sata
* ata
* usb
* ieee1394

Default when unconfigured: "usb ieee1394"

Separate multiple classes with spaces.

.. caution::

    USB and IEEE1394 disks may fail to mount or data may get corrupted
    with APM enabled. Be careful and make sure you have backups of all affected
    media before removing `usb` or `ieee1394` from the denylist!


.. _set-disks-spindown:

DISK_SPINDOWN_TIMEOUT_ON_AC/BAT
-------------------------------
::

    DISK_SPINDOWN_TIMEOUT_ON_AC="0 0"
    DISK_SPINDOWN_TIMEOUT_ON_BAT="0 0"

Timeout value until the spindle motor stops when the disk is idle. Valid settings:

* 0 – disabled
* 1..240 – timeouts from 5 seconds to 20 minutes (in increments of 5 seconds)
* 241..251 – timeouts from 30 minutes to 5.5 hours (in increments of 30 minutes)
* keep – special value to skip this setting for the particular disk (synonym: _)

Values for multiple disks are separated with blanks.

.. note::

    * SSDs don't have moving parts, therefore this setting is "don't care" for
      them and can remain disabled.
    * Stopping the system disk for extended periods of time is unlikely to work,
      because applications and system daemons wake up the disk frequently. However
      for a 2nd disk in a swappable drive slot or the Ultrabay that is not accessed
      permanently, this setting may be quite useful.

DISK_IOSCHED
------------
::

    DISK_IOSCHED="mq-deadline mq-deadline"

Sets the I/O scheduler per disk. Possible values below.

.. rubric:: Multi queue (blk-mq) schedulers:

* mq-deadline – recommended
* none
* kyber
* bfq
* keep – special value to use the kernel's default scheduler for the particular disk (synonym: _)

.. rubric:: Single queue schedulers:

* deadline – recommended
* cfq
* bfq
* noop
* keep – special value to use the kernel's default scheduler for the particular disk (synonym: _)

Default when unconfigured: keep

.. note::

    * Single queue schedulers are legacy now and were removed together with the
      old block layer for kernels ≥ 5.0.
    * Multi queue (blk-mq) may need kernel boot options `scsi_mod.use_blk_mq=1`
      and `dm_mod.use_blk_mq=y` as well as :command:`modprobe mq-deadline-iosched|kyber|bfq`
      on kernels < 5.0.

.. _set-disks-alpm:

SATA_LINKPWR_ON_AC/BAT
----------------------
.. rubric:: AHCI Link Power Management (ALPM)

::

    SATA_LINKPWR_ON_AC="med_power_with_dipm"
    SATA_LINKPWR_ON_BAT="med_power_with_dipm"

Sets the power management mode for the SATA links connecting disks and optical
drives. Possible values (in order of increasing power saving):

* max_performance – minimum power saving / max performance
* medium_power – medium power saving and performance
* med_power_with_dipm – best balance between power saving and performance
  (recommended, kernel ≥ 4.15 required)
* min_power – max power saving / minimum performance

Default when unconfigured: med_power_with_dipm (AC and BAT)

Use empty strings (“”) to disable the feature completely.
Multiple values separated with spaces are tried sequentially until success.

.. note::

    Until version 1.5 a second value was provided in the default configuration
    as a fallback for older kernels. TLP determines automatically if
    `med_power_with_dipm` is available and otherwise uses the second value.

.. _SATA_LINKPWR_BLACKLIST:

SATA_LINKPWR_DENYLIST
----------------------
::

    SATA_LINKPWR_DENYLIST="host1"

Exclude SATA disks from AHCI link power management (ALPM). This is intended as
a workaround for SATA disks not bearing link power management.

Disks are specified by their host. Refer to the output of :command:`tlp-stat -d`
to determine the host; the format is `hostX`. Separate multiple hosts with spaces.


.. _set-disks-ahci-runtime-pm:

AHCI_RUNTIME_PM_ON_AC/BAT
-------------------------
::

    AHCI_RUNTIME_PM_ON_AC=on
    AHCI_RUNTIME_PM_ON_BAT=auto


Controls runtime power management for NVMe, SATA, ATA and USB disks
as well as SATA ports. Possible values:

* auto – enabled (power down idle devices)
* on – disabled (devices powered on permanently)

.. note::

    * Works only on disks defined in `DISK_DEVICES`
    * SATA *controllers* are PCIe bus devices and handled by the corresponding
      :doc:`RUNTIME_PM settings</settings/runtimepm>`

Default when unconfigured: on (AC), auto (BAT)


AHCI_RUNTIME_PM_TIMEOUT
-----------------------
::

    AHCI_RUNTIME_PM_TIMEOUT=15

Seconds of inactivity before a disk or a port is suspended. Effective only when
`AHCI_RUNTIME_PM_ON_AC/BAT` is activated.

Default when unconfigured: 15


.. seealso::

    * Settings: :doc:`/settings/introduction`
    * FAQ: :doc:`/faq/disks`
    * `Linux I/O schedulers <https://wiki.ubuntu.com/Kernel/Reference/IOSchedulers>`_ – Ubuntu Wiki article
    * `med_power_with_dipm <https://hansdegoede.livejournal.com/18412.html>`_ – Explanation from kernel developer Hans de Goede
