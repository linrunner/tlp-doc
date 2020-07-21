Introduction
************
TLP is a feature-rich command line utility for Linux, saving laptop battery power
without the need to delve deeper into technical details.

TLP's default settings are already optimized for battery life, so you may just
install and forget it. Nevertheless TLP is highly customizable to fulfil your
specific requirements.

.. important::

    TLP will take care of the majority of settings that :command:`powertop --autotune`
    would, and with less trial and error, see :doc:`/faq/powertop`.

.. note::

    TLP is a pure command line utility. It does not contain a GUI.

.. _intro-how-it-works:

How it works
============
What TLP basically does is tweaking `kernel settings` that affect power
consumption.

So what are `kernel settings`?
------------------------------
First of all `kernel settings` are of a volatile nature. Their state is held in
RAM during runtime and the kernel provides no persistence for them.
Upon boot the kernel creates a default state and changes have to be re-applied
on every boot by a `user space tool`. TLP is such a `user space tool`.

Most `kernel settings` TLP works on are exported to user space as
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
one for battery (BAT) and one for AC operation.
This means that TLP must apply the appropriate profile not only at boot, but
also each time the power source changes.

Event-driven architecture
-------------------------

To achieve all of the above, TLP's actions are event-driven. The following events
will cause settings to be applied:

Charger plugged in (AC powered)
    Applies the AC settings profile.

Charger unplugged (battery powered)
    Applies the BAT settings profile.

USB device plugged in
    Activates USB autosuspend mode for the device (if not automatically excluded
    or blacklisted).

System startup (boot)
    Applies the settings profile corresponding to the current power source
    AC/BAT, applies charge thresholds and switches bluetooth, Wi-Fi and WWAN
    devices depending on your individual settings (this is disabled in the
    default configuration).

System shutdown (power off)
    Applies the AC settings profile so that the laptop is powered off as fast
    as possible.

System reboot
    Applies the AC settings profile to reboot swiftly and after reboot proceeds
    as described above for system startup.

System suspend - ACPI Sleep States S3 (Suspend to RAM) or S4 (Suspend to disk)
    Applies the AC settings profile so that the suspend state is reached as
    soon as possible.

System resume
    Applies the settings profile corresponding to the current power source AC/BAT.

LAN, Wi-Fi, WWAN connected/disconnected or laptop docked/undocked (:doc:`/settings/rdw`)
    Enable or disable builtin bluetooth, Wi-Fi and WWAN devices depending on your
    individual settings (this is disabled in the default configuration).

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

Features
========
:doc:`/settings/index` are organized into two profiles, enabling you to adjust
between savings and performance independently for battery (BAT) and AC operation:

* Kernel laptop mode and dirty buffer timeouts
* Processor frequency scaling including 'turbo boost' and 'turbo core'
* Limit max/min P-state to control power dissipation of Intel CPUs
* Intel CPU energy/performance policies HWP.EPP and EPB
* Hard disk advanced power magement level (APM) and spin down timeout (per disk)
* AHCI link power management (ALPM) with device blacklist
* AHCI runtime power management for host controllers and disks
* PCIe active state power management (ASPM)
* Runtime power management for PCIe bus devices
* Intel GPU frequency limits
* AMD Radeon GPU power management
* Wi-Fi power save
* Enable/disable integrated bluetooth, Wi-Fi and WWAN devices
* Power off removable optical drives (in drive bays)
* Audio power save

Additional settings - independent of the power source - are:

* I/O scheduler (per disk)
* USB autosuspend with device blacklist/whitelist
* Enable or disable radio devices (bluetooth, Wi-Fi and WWAN) upon boot and shutdown
* Restore radio device state on boot (from previous shutdown)
* Radio device wizard: enable/disable radios upon network connect/disconnect and dock/undock
* Disable Wake-On-LAN
* Bluetooth and WWAN state is restored after suspend/hibernate
* Battery charge thresholds and recalibration – ThinkPads only

.
