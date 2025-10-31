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

* :command:`systemctl stop tlp.service` – This will not prevent TLP from applying
  power saving settings when the power source AC/battery changes. Depending on
  your configuration, it may also immediately power off your Wi-Fi and/or
  bluetooth/WWAN, which might be undesirable.
* :command:`systemctl disable/mask tlp.service` – This will only prevent TLP
  from applying charge thresholds and switching radios on system startup/shutdown.
  Power saving settings are applied nevertheless (see above).

.. seealso::

    Background: the service is intended for system startup and shutdown only.
    :ref:`intro-how-it-works` (user perspective) and
    :doc:`/developers/architecture` (technical) explain TLP's event-driven
    actions.


.. _faq-start-tlp:

How do I start/restart TLP – and apply power saving?
----------------------------------------------------
**The right way:** ::

    tlp start

.. note::

    Following the installation, TLP will start automatically on boot. To avoid
    having to restart the system, the first time you can start it manually
    as explained above.

Make sure, as a prerequisite, that:

* `TLP_ENABLE=1` is configured
* systemd service units are enabled, see below. The output of
  :command:`tlp-stat -s` will tell you  when this is not the case.

*Wrong way:*

* :command:`systemctl start/restart tlp.service` –
  The service is intended for system startup and shutdown only. Depending on
  your configuration, it may immediately power off your Wi-Fi and/or
  bluetooth/WWAN and drive bay, which might be undesirable.
  `tlp start` applies only power saving.


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

:command:`tlp-stat --psup` shows only the battery

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

* Update to the newest version – highly recommended
* Reboot with AC connected
* Ask the laptop vendor for a corrected BIOS
* Fix the DSDT yourself

.. _faq-resume-freeze:

System freezes on wakeup from suspend on battery
------------------------------------------------
Symptom: on battery power, trying to wake up, the laptop freezes (either showing
a black screen or a static lock screen) and becomes unresponsive to input.

Solutions:

* Update to version 1.6
* Workaround for older versions: change your configuration to disable
  :ref:`disk runtime power management <set-disks-ahci-runtime-pm>`
  on battery power: ::

    AHCI_RUNTIME_PM_ON_BAT=on

Related issues: `#593 <https://github.com/linrunner/TLP/issues/593>`_,
`#606 <https://github.com/linrunner/TLP/issues/606>`_,
`#698 <https://github.com/linrunner/TLP/issues/698>`_.

Shutdown freezes before poweroff
--------------------------------
Solution: add the `mei_me` module to :ref:`set-runtimepm-driver-denylist`.

Shutdown reboots instead of poweroff
------------------------------------
Affected hardware: HP laptops (based on user feedback)

Solution: deactivate Wake-on-LAN in the BIOS.

Affected hardware: ThinkPad T14 Gen 6 Intel (and possibly other vendors and models)

Solution: deactivate Runtime PM: ::

    RUNTIME_PM_ON_AC=
    RUNTIME_PM_ON_BAT=

Related issue: `#830 <https://github.com/linrunner/TLP/issues/830>`_.

Spontaneous shutdown (on battery)
---------------------------------
Symptoms: laptop shuts down spontaneously when:

* Changing to battery power
* Headphone jack is plugged in

Affected hardware: Acer Aspire V5-591G (kernel 4.4),
Acer Extensa 2520G (Ubuntu 22.04)

Solution: disable :doc:`/settings/audio` power saving.

Ethernet not working after resume
---------------------------------
Affected hardware: a Dell XPS user with Kernel 4.4 reported this issue.

Solution: enable Wake-on-LAN ::

    WOL_DISABLE=N
