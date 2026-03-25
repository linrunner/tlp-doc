Radio Device Switching
======================
This section contains settings to switch builtin bluetooth, Wi-Fi and WWAN
devices on system boot or shutdown and depending on the power source.

For more event based switching refer to :doc:`/settings/rdw`.

RESTORE_DEVICE_STATE_ON_STARTUP
-------------------------------
::

    RESTORE_DEVICE_STATE_ON_STARTUP=0

Restores radio device state (builtin Bluetooth, Wi-Fi, WWAN) from previous
shutdown on boot:

* 0 – disable
* 1 – enable

Default when unconfigured: 0

.. important::

    * The parameters `DEVICES_TO_DISABLE_ON_STARTUP/SHUTDOWN` below are ignored when
      this feature is enabled
    * Debian and Ubuntu packages mask `systemd-rfkill.service` because it implements
      identical functionality; to mimic the systemd default behavior,
      set `RESTORE_DEVICE_STATE_ON_STARTUP=1`

.. _set-radio-disable-on:

DEVICES_TO_DISABLE_ON_STARTUP
-----------------------------
::

    DEVICES_TO_DISABLE_ON_STARTUP="bluetooth wifi wwan"

Disables builtin radio devices on boot:

* bluetooth
* wifi – Wireless LAN (Wi-Fi)
* wwan – Wireless Wide Area Network (3G/UMTS, 4G/LTE, 5G)

Multiple devices are separated with blanks.

DEVICES_TO_ENABLE_ON_STARTUP
----------------------------
::

    DEVICES_TO_ENABLE_ON_STARTUP="bluetooth wifi wwan"

Linux enables all builtin radio devices by default. In case of exception you
can use this setting to enable the missing devices on boot.
Possible values are as above.

.. note::

    The following settings apply only at the moment where the TLP profile
    (or power source) actually changes.

DEVICES_TO_ENABLE_ON_AC
-----------------------
::

    DEVICES_TO_ENABLE_ON_AC="bluetooth nfc wifi wwan"

*Version 1.9 and newer*

Enables builtin radio devices when changing to the *performance* profile.

*Version 1.8 and older*

Enables builtin radio devices when AC power is plugged in.

Possible values are as above.


DEVICES_TO_DISABLE_ON_BAT
-------------------------
::

    DEVICES_TO_DISABLE_ON_BAT="bluetooth nfc wifi wwan"

*Version 1.9 and newer*

Disables builtin radio devices regardless of their connection state
when changing to the *balanced* or *power-saver* profile.

*Version 1.8 and older*

Disables builtin radio devices regardless of their connection state
when changing to battery power.

Possible values are as above.

DEVICES_TO_DISABLE_ON_BAT_NOT_IN_USE
------------------------------------
::

    DEVICES_TO_DISABLE_ON_BAT_NOT_IN_USE="bluetooth nfc wifi wwan"

*Version 1.9 and newer*

Disables builtin radio devices that are not connected when changing to the
*balanced* or *power-saver* profile.

*Version 1.8 and older*

Disables builtin radio devices that are not connected when changing to battery
power.

Possible values are as above.

.. note::

    Do not use both `DEVICES_TO_DISABLE_ON_BAT` and `DEVICES_TO_DISABLE_ON_BAT_NOT_IN_USE`
    for the same radio device because `DEVICES_TO_DISABLE_ON_BAT` always has precendence.


.. seealso::

    * Settings: :doc:`/settings/introduction`
    * FAQ: :doc:`/faq/radio`
    * `rfkill <https://www.kernel.org/doc/Documentation/rfkill.txt>`__
      – Kernel framework for switching radio devices
