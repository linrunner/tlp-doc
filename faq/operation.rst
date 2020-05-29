Operation
================

.. _faq-disable-tlp:

How do I stop or disable TLP completely?
----------------------------------------
**The right way:**

* Change configuration: `TLP_ENABLE=0`
* Optional: reboot to restore kernel defaults

.. note::

    Uninstalling the packages will also disable TLP completely.

*Wrong ways:*

* :command:`systemctl stop tlp.service` – *this will not prevent TLP from applying
  power saving settings when the power source AC/battery changes; depending on
  your configuration this may also immediately power off your Wi-Fi and/or
  bluetooth/WWAN, which might be undesirable*
* :command:`systemctl disable/mask tlp.service` – *this will only prevent TLP
  from applying charge thresholds and switching radios on system startup/shutdown;
  power saving settings are applied nevertheless (see above)*

.. _faq-start-tlp:

How do I start/restart TLP – and apply power saving?
----------------------------------------------------
**The right way:** ::

    tlp start

.. note::

    Following the installation, TLP will start automatically on boot. To avoid
    having to restart the system, the first time you can start it manually
    as explained above.

Make shure, as a prerequisite, that:

* `TLP_ENABLE=1` is configured
* systemd service units are enabled, see below; the output of
  :command:`tlp-stat -s` will tell you  when this is not the case

*Wrong way:*

* :command:`systemctl start/restart tlp.service` – depending on your configuration
  this may immediately power off your W-Fi and/or bluetooth/WWAN and drive bay,
  which might be undesirable; `tlp start` applies only power saving

.. _faq-service-units:

systemd: must I enable TLP's service units?
-------------------------------------------
Yes. The service units are *indispensable* for correct operation:

* **tlp.service**: applies power saving settings and switches radio devices on
  system boot and shutdown
* **tlp-sleep.service**: applies powers saving upon system suspend and resume
  (not applicable for version 1.3 and higher)

.. note::

    Debian, Fedora and Ubuntu enable the service by default as part of the
    package :doc:`/installation/index`, others such as Arch Linux don't.
    If unshure check the output of :command:`tlp-stat -s` for corresponding
    notes.

How to temporarily use battery settings on AC (and vice versa)?
---------------------------------------------------------------
Invoke `manual mode` with the following commands: ::

    tlp bat
    tlp ac

Manual mode will remain until the next reboot or until terminated with the
command ::

    tlp start

.. seealso::

    * :ref:`set-persistent-default` for a permanent change
    * :doc:`/usage/tlp` command


AC or BAT is not detected
-------------------------
This also concerns changes from AC to BAT and vice versa.

Possible causes: BIOS bug (DSDT may need fixing) or kernel ACPI bug.

Workaround: to ignore the problematic power source, try to configure ::

    PS_IGNORE=AC

or ::

    PS_IGNORE=BAT


.. faq-ac-quirk:

AC is not detected when plugged in
----------------------------------
Affected hardware: Dell XPS 15 9550/9560 (happens after booting on battery only)

Symptoms: :command:`tlp-stat -s` shows ::

    Power source = battery

:command:`tlp-stat --psus` shows only the battery

.. code-block:: none

    /sys/class/power_supply/BAT0/type:Battery
    /sys/class/power_supply/BAT0/present:1
    /sys/class/power_supply/BAT0/device/path:_SB_.BAT0

Charger is present when booted on AC only:

.. code-block:: none

    /sys/class/power_supply/AC/type:Mains
    /sys/class/power_supply/AC/online:1
    /sys/class/power_supply/AC/device/path:_SB_.AC__

Cause: BIOS bug, DSDT needs fixing (see last comment in
`kernel bug #156171 <https://bugzilla.kernel.org/show_bug.cgi?id=156171>`_).

References: Issues `#223 <https://github.com/linrunner/TLP/issues/223>`_,
`#343 <https://github.com/linrunner/TLP/issues/343>`_,
`#362 <https://github.com/linrunner/TLP/issues/362>`_.

Solutions:

* Update to version 1.2 or higher – recommended
* Reboot with AC connected
* Ask the laptop vendor for a corrected BIOS
* Fix the DSDT yourself

Shutdown freezes before poweroff
--------------------------------
Solution: add the `mei_me` module to :ref:`set-runtimepm-driver-blacklist`.

Shutdown reboots instead of poweroff
------------------------------------
Affected hardware: HP laptops (based on user feedback)

Solution: deactivate Wake-on-LAN in the BIOS.

Spontaneous shutdown on battery
-------------------------------
Symptom: laptop shuts down spontaneously when changing to battery power.

Affected hardware: an Acer Aspire V5-591G user with kernel 4.4 reported this issue.

Solution: disable :doc:`/settings/audio` power saving.

Ethernet not working after resume
---------------------------------
Affected hardware: a Dell XPS user with Kernel 4.4 reported this issue.

Solution: enable Wake-on-LAN ::

    WOL_DISABLE=N

Exit Code 4
-----------
Symptom: journal shows the message

.. code-block:: none

    ERROR: systemd-udevd[1234]: Process '/usr/bin/tlp auto' failed with exit code 4

Solution: update to version 1.2 or higher.

