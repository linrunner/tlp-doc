Dependencies
============
TLP depends on (or conflicts with) the tools described below. They should be
implemented as package dependencies.

Package tlp
-----------
acpi-call - *obsolete* [starting with 1.7]
   External kernel module that became obsolete with kernel 5.17.

auto-cpufreq - *conflicts*
    A power management tool that essentially works on the same tunables as TLP.

awk, grep, sed - *depends*
    TLP is tested with the GNU version of these essential utilities.
    Your mileage with other implementations may vary.

.. include:: /include/busybox-not-supported.rst

ethtool - *depends optional*
    Used to disable Wake-on-LAN.

hdparm - *depends*
    Needed for hard disk advanced power management (APM) and to show information
    in :command:`tlp-stat -d`.

iw - *depends*
    Needed for Wi-Fi power save, replaces deprecated `iwconfig`
    (see wireless-tools below).

laptop-mode-tools - *conflicts*
    A power management tool that essentially works on the same tunables as TLP.

pciutils - *depends*
    Provides `lspci` used to show PCI(e) devices in :command:`tlp-stat -e`.


rfkill - *depends*
    Needed for switching radio devices on and off.

smartmontools - *depends optional*
    Provides `smartctl` used to show hard disk drive SMART data in
    :command:`tlp-stat -d`.

tuned - *conflicts*
    A power management tool that essentially works on the same tunables as TLP.

tp-smapi - *depends optional*
    External kernel modules providing battery charge thresholds and recalibration
    for ThinkPads before model year 2011 as well as specific :command:`tlp-stat -b`
    output until model year 2011. See :ref:`bc-vendor-thinkpad-legacy`.

.. include:: /include/no-package-dep.rst

udev *- depends*
    Needed for event handling (see :doc:`architecture`) and providing `udevadm`.

usbutils - *depends*
    Provides `lsusb` used to show USB devices in :command:`tlp-stat -u`.

util-linux - *depends*
    Provides `flock` and `dmesg` (for :command:`tlp-stat -w`).

.. include:: /include/busybox-not-supported.rst

x86_energy_perf_policy - *obsolete* [starting with 1.7]
    Tool for setting the energy performance bias (EPB). Became obsolete
    with kernel 5.2.


Package tlp-pd
--------------

tlp - *depends*
    tlp-pd invokes tlp to apply power profiles.

dbus-python - *depends*
    Required by tlp-pd to implement the D-Bus daemon.

polkit - *depends*
    Required by tlp-pd to implement D-Bus API authorization for tlpctl's actions.

.. _dev-depend-ppd:

power-profiles-daemon - *provides (and/or conflicts)*
    tlp-pd offers similiar functionality and replaces power-profiles-daemon.

PyGObjects - *depends*
    Required by tlp-pd and tlpctl for D-Bus functionality.

python3 - *depends*
    tlp-pd and tlpctl are written in Python.

tuned-ppd - *conflicts*
    tlp-pd offers similiar functionality and replaces tuned-ppd.


Package tlp-rdw
---------------

tlp - *depends*
    Provides libraries **tlp-func-base** and **func.d/***.

network-manager - *depends*
    Used to hook ifup/ifdown events and to determine the corresponding
    interface type LAN/Wi-Fi/WWAN.
