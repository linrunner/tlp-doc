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

awk, grep, sed - *mandatory*
    TLP is tested with the GNU version of these essential utilities.
    Your mileage with other implementations may vary.

.. include:: /include/busybox-not-supported.rst

ethtool - *optional*
    Used to disable Wake-on-LAN.

hdparm - *mandatory*
    Needed for hard disk advanced power management (APM) and to show information
    in :command:`tlp-stat -d`.

iw - *mandatory*
    Needed for Wi-Fi power save, replaces deprecated `iwconfig`
    (see wireless-tools below).

laptop-mode-tools - *conflicts*
    A power management tool that essentially works on the same tunables as TLP.

pciutils - *mandatory*
    Provides `lspci` used to show PCI(e) devices in :command:`tlp-stat -e`.


.. _dev-depend-ppd:

power-profiles-daemon - *conflicts*
    `power-profiles-daemon` manages the following subset of TLP's tunables:

     * AMDGPU_ABM_LEVEL_ON_AC/BAT (activated in default.conf)
     * CPU_ENERGY_PERF_POLICY_ON_AC/BAT (activated in default.conf)
     * CPU_BOOST_ON_AC/BAT
     * PLATFORM_PROFILE_ON_AC/BAT

    Much more serious is that
    `power-profiles-daemon.service <https://gitlab.freedesktop.org/hadess/power-profiles-daemon/-/blob/main/data/power-profiles-daemon.service.in>`_
    breaks the activation of TLP's settings at boot time by preventing the start
    of `tlp.service`: ::

        Conflicts= ... tlp.service ...

    Masking or disabling `power-profiles-daemon.service` is not a viable
    alternative, as it breaks `powerprofilesctl` and desktop components
    using it.

    Conclusion: this can only be reliably prevented by not installing `power-profiles-daemon`
    and `tlp` at the same time.

    .. important::

        Make sure that adding the conflict to a `tlp` distribution package does not
        unintentionally uninstall essential desktop packages. If necessary,
        their dependencies on `power-profiles-daemon` must be changed to optional.

    .. seealso::

       * `Issue #564 <https://github.com/linrunner/TLP/issues/564#issuecomment-943292082>`_
       * `Bug 2028701 <https://bugzilla.redhat.com/show_bug.cgi?id=2028701>`_


rfkill - *mandatory*
    Needed for switching radio devices on and off.

smartmontools - *optional*
    Provides `smartctl` used to show hard disk drive SMART data in
    :command:`tlp-stat -d`.

tuned - *conflicts*
    A power management tool that essentially works on the same tunables as TLP.

tp-smapi - *optional*
    External kernel modules providing battery charge thresholds and recalibration
    for ThinkPads before model year 2011 as well as specific :command:`tlp-stat -b`
    output until model year 2011. See :ref:`bc-vendor-thinkpad-legacy`.

.. include:: /include/no-package-dep.rst

udev *- mandatory*
    Needed for event handling (see :doc:`architecture`) and providing `udevadm`.

usbutils - *mandatory*
    Provides `lsusb` used to show USB devices in :command:`tlp-stat -u`.

util-linux - *mandatory*
    Provides `flock` and `dmesg` (for :command:`tlp-stat -w`).

.. include:: /include/busybox-not-supported.rst

x86_energy_perf_policy - *obsolete* [starting with 1.7]
    Tool for setting the energy performance bias (EPB). Became obsolete
    with kernel 5.2.


Package tlp-rdw
---------------

tlp - *mandatory*
    Provides libraries **tlp-func-base** and **func.d/***.

network-manager - *mandatory*
    Used to hook ifup/ifdown events and to determine the corresponding
    interface type LAN/Wi-Fi/WWAN.

Package tlp-pd
--------------

tlp - *mandatory*
    tlp-pd invokes tlp to apply power profiles.

dbus-python - *mandatory*
    Required by tlp-pd to implement the D-Bus daemon.

power-profiles-daemon - *conflicts*
    tlp-pd offers similiar functionality and replaces power-profiles-daemon.

PyGObjects - *mandatory*
    Required by tlp-pd and tlpctl for D-Bus functionality.

python3 - *mandatory*
    tlp-pd and tlpctl are written in Python.

tuned-ppd - *conflicts*
    tlp-pd offers similiar functionality and replaces tuned-ppd.
