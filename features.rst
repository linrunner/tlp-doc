.. _features:

Features
********
TLP is a feature-rich command line utility for Linux, saving laptop battery power
without the need to delve deeper into technical details.

TLP's default settings are already optimized for battery life, so you may just
install and forget it. Nevertheless TLP is highly customizable to fulfil your
specific requirements.

:doc:`/settings/index` are organized into two profiles, enabling you to adjust
between savings and performance independently for battery (BAT) and AC operation.

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

Additional settings - independent of ther power source - are:

* I/O scheduler (per disk)
* USB autosuspend with device blacklist/whitelist
* Enable or disable radio devices (bluetooth, Wi-Fi and WWAN) upon boot and shutdown
* Restore radio device state on boot (from previous shutdown)
* Radio device wizard: enable/disable radios upon network connect/disconnect and dock/undock
* Disable Wake-On-LAN
* Bluetooth and WWAN state is restored after suspend/hibernate
* Battery charge thresholds and recalibration – ThinkPads only

.. note::

    * TLP is a pure command line utility with automated background tasks. It
      does not contain a GUI.
    * TLP implements :doc:`/faq/powertop`'s recommendations out of the box, so
      there is no need to also run :command:`powertop --autotune` on boot.
