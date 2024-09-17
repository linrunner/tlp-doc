Introduction
************
TLP is a feature-rich command line utility for Linux, saving laptop battery power
without the need to delve deeper into technical details.

TLP's default settings are already optimized for battery life, so you may
install it and just sit back and relax.
Nevertheless, TLP is completely customizable to get even more power savings
or meet your exact requirements.

.. important::

    TLP will take care of the majority of settings that :command:`powertop --autotune`
    would, and with less trial and error, see :doc:`/faq/powertop`.

.. note::

    TLP is a pure command line utility. It does not contain a GUI.

.. _intro-how-it-works:

How it works
============
What TLP basically does is tweak `kernel tunables` that have an impact on
power consumption.

So what are `kernel tunables`?
------------------------------
First, `kernel tunables` are volatile in nature. Their state is held in RAM
during runtime, and the kernel provides no persistence for them. At boot time,
the kernel creates a default state, and changes must be reapplied by a userspace
tool at each boot. TLP is one such `userspace tool`.

Most `kernel tunables` TLP works on are exported to user space as
`sysfs nodes <https://en.wikipedia.org/wiki/Sysfs>`_
i.e. files below **/sys/**. The output of :command:`tlp-stat` will show the
paths.

.. important::

    Not all `sysfs nodes` shown by :command:`tlp-stat` are actually touched
    by TLP when applying settings, some are displayed for information or
    diagnostic purposes only.

Profiles
--------
TLP provides two independent sets of :doc:`/settings/index` called profiles,
one for battery operation (BAT) and one for AC operation.
This means that TLP must apply the appropriate profile not only at boot time,
but also each time the power source is changed.

Event-driven architecture
-------------------------

To accomplish all of the above, TLP's actions are event-driven. The following events
cause settings to be applied:

Charger plugged in (AC powered)
    Applies the AC settings profile.

Charger unplugged (battery powered)
    Applies the BAT settings profile.

USB device plugged in
    Activates USB autosuspend mode for the device (if not excluded or denylisted).

System startup (boot)
    Applies the settings profile corresponding to the current power source
    AC/BAT. Applies charge thresholds and switches bluetooth, Wi-Fi and WWAN
    devices depending on your individual settings (disabled in the default
    configuration).

System shutdown (power off)
    Saves or switches bluetooth, Wi-Fi and WWAN device state and disables USB
    autosuspend depending on your individual settings (disabled in the default
    configuration).

System reboot
    Same as shutdown, then continues with startup.

System suspend to ACPI Sleep States S0ix (Idle standby), S3 (Suspend to RAM) or S4 (Suspend to disk)
    Saves bluetooth, Wi-Fi and WWAN device states and powers off removable optical
    drives depending on your individual settings (disabled in the default
    configuration).

System resume from ACPI Sleep States S0ix (Idle standby), S3 (Suspend to RAM) or S4 (Suspend to disk)
    Applies the settings profile corresponding to the current power source AC/BAT.
    Restores charge thresholds and bluetooth, Wi-Fi and WWAN device states
    depending on your individual settings (disabled in the default configuration).

LAN, Wi-Fi, WWAN connected/disconnected or laptop docked/undocked (:doc:`/settings/rdw`)
    Enables or disables builtin bluetooth, Wi-Fi and WWAN devices depending on
    your individual settings (disabled in the default configuration).

.. note::

    * TLP will not make dynamic or adaptive changes to the settings beyond the
      events described above
    * In particular, TLP will never adjust the settings due to CPU load, battery
      charge level or else

.. important::

    TLP does not monitor the above events itself but relies on a range of
    system daemons, namely `systemd`, `udevd` and `NetworkManager`.
    Therefore TLP does not include a daemon and there is no permanent `tlp`
    background process showing up in the output of :command:`ps`. Refer to
    :doc:`/developers/architecture` for technical details.


.. _intro-features:

Features
========
:doc:`Power saving settings </settings/index>` are organized into two profiles, enabling you to adjust
between savings and performance independently for battery (BAT) and AC operation:

* Kernel laptop mode and dirty buffer timeouts
* AMD/Intel CPU scaling driver operation mode (active/guided/passive)
* Processor frequency scaling and turbo boost
* Intel CPU max/min P-state limits to control power dissipation
* AMD/Intel CPU energy/performance policies (EPP) and dynamic boost
* Platform profile to control power/performance levels, thermal and fan speed
* Hard disk advanced power magement level (APM) and spin down timeout (per disk)
* AHCI link power management (ALPM) with device denylist
* AHCI runtime power management for NVMe/SATA/USB disks and SATA ports
* PCIe active state power management (ASPM)
* Runtime power management for PCIe bus devices
* Intel GPU frequency limits
* AMD GPU power management
* Wi-Fi power save
* Enable/disable integrated bluetooth, Wi-Fi and WWAN devices
* Power off removable optical drives (in drive bays)
* Audio power save

Additional power saving settings - independent of the power source - are:

* I/O scheduler (per disk)
* USB autosuspend with device denylist/allowlist
* Enable or disable radio devices (bluetooth, Wi-Fi and WWAN) upon boot and shutdown
* Restore radio device state on boot (from previous shutdown)
* Radio device wizard: enable/disable radios upon network connect/disconnect and dock/undock
* Disable Wake-On-LAN
* Bluetooth and WWAN state is restored after suspend/hibernate

Battery care settings are:

* Charge thresholds and recalibration
