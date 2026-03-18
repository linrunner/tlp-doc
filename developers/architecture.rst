Architecture
============
TLP's actions are event-driven.

There is no TLP daemon to monitor events related to power sources, hot-plugging,
network interfaces or system startup/shutdown/suspend/resume. Instead TLP depends on
standard system daemons:

* systemd (or an alternative init system)
* udevd
* elogind
* NetworkManager (when **tlp-rdw** is installed)

However, if the user switches profiles by mouse click on the desktop or using :command:`tlpctl`,
the `TLP profiles daemon` **tlp-pd** steps in.

The following events will cause settings to be applied:

Charger plugged/unplugged - udevd
---------------------------------
A rule in **tlp.rules** calls :command:`tlp auto` which applies the appropriate TLP profile.

USB device plugged in - udevd
-----------------------------
A rule in **tlp.rules** calls :command:`tlp-usb-udev` to activate USB
autosuspend.

System startup/shutdown/reboot - systemd et al.
-----------------------------------------------
Depends on the init system:

Systemd
    The sources provide the unit :command:`tlp.service`, which calls
    :command:`tlp init start/stop` upon system startup/shutdown. Use the
    `TLP_WITH_SYSTEMD` :doc:`makefile` switch to install it.

Sysvinit
    The package install process must link the provided sysvinit script **tlp.init** i.e.
    **/etc/init.d/tlp** to the necessary runlevels by means of the target
    distribution.

Other init system
    In case the target distribution does support neither `systemd` nor `sysvinit`,
    you have to provide a script that calls :command:`tlp` accordingly: ::

        tlp init start|stop|restart|force-reload|status

    The parameter after `init` is checked by :command:`tlp`.

System suspend/resume - systemd et al.
--------------------------------------
*ACPI Sleep States S0ix (Idle standby), S3 (Suspend to RAM) or S4 (Suspend to disk)*

Depends on the target distribution:

Systemd
    The sources provide the script **tlp-sleep**
    which calls :command:`tlp suspend` or :command:`tlp resume`.
    Use the `TLP_WITH_SYSTEMD` :doc:`makefile` switch to install it to
    **/lib/systemd/system-sleep/tlp**.

Non-systemd
    * **elogind**: the sources provide the script **tlp-sleep.elogind**
      which calls :command:`tlp suspend` or :command:`tlp resume`.
      Use the `TLP_WITH_ELOGIND` :doc:`makefile` switch to install it to
      **/lib/elogind/system-sleep/49-tlp-sleep**.
    * Else: you must provide a way to run :command:`tlp suspend`
      or :command:`tlp resume` (your mileage may vary).

User changes profile by mouse click or tlpctl - tlp-pd
-------------------------------------------------------
**tlp-pd** receives the D-Bus message and calls :command:`tlp <profile>`.

LAN, Wi-Fi, WWAN connected/disconnected - NetworkManager
--------------------------------------------------------
The sources provide the script
**tlp-rdw-nm** - installed to **/usr/lib/NetworkManager/dispatcher.d/99tlp-rdw-nm**
by :command:`make install-rdw` - switching the configured radio devices.

Laptop docked/undocked - udevd
------------------------------
A rule in **tlp-rdw.rules** calls :command:`tlp-rdw-udev` switching the configured
radio devices.

.. seealso::

    :doc:`/introduction` has more details on the event-related settings.
