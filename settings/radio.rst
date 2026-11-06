Radio Device Switching
======================
This section contains settings to switch builtin Bluetooth, Wi-Fi and WWAN
devices on system boot or shutdown and when changing profiles.

For more event based switching refer to :doc:`/settings/rdw`.

RESTORE_DEVICE_STATE_ON_STARTUP
-------------------------------
*This parameter has been removed in version 1.11, do not use anymore* ::

    RESTORE_DEVICE_STATE_ON_STARTUP=0

Restores radio device state (builtin Bluetooth, Wi-Fi, WWAN) from previous
shutdown on boot:

* 0 – disable
* 1 – enable

Default when unconfigured: 0

.. important::

    * The parameters `DEVICES_TO_DISABLE_ON_STARTUP` below are ignored when
      this feature is enabled
    * Debian and Ubuntu packages mask `systemd-rfkill.service` because it implements
      identical functionality; to mimic the systemd default behavior,
      set `RESTORE_DEVICE_STATE_ON_STARTUP=1`


.. _set-radio-disable-on:

DEVICES_TO_DISABLE_ON_STARTUP
-----------------------------
Disables builtin radio devices on boot:

* bluetooth
* nfc – Near Field Communication (NFC)
* wifi – Wireless LAN (Wi-Fi)
* wwan – Wireless Wide Area Network (4G/LTE, 5G)

Multiple devices are separated with blanks. ::

    DEVICES_TO_DISABLE_ON_STARTUP="bluetooth nfc wifi wwan"

Default when unconfigured: <none>


DEVICES_TO_ENABLE_ON_STARTUP
----------------------------
Linux enables all builtin radio devices by default. In case of exception you
can use this setting to enable the missing devices on boot.
Possible values are as above. ::

    DEVICES_TO_ENABLE_ON_STARTUP="wifi"

Default when unconfigured: <none>

.. note::

    The following settings apply only at the moment where the TLP profile
    (or power source) actually changes.

DEVICES_TO_ENABLE/DISABLE_ON_PRF
--------------------------------
Enables or disables builtin radio devices when changing to the *performance* profile.
Possible values are as above.

*Version 1.11 and newer* ::

    DEVICES_TO_ENABLE_ON_PRF="bluetooth nfc wifi wwan"
    DEVICES_TO_DISABLE_ON_PRF="bluetooth nfc wifi wwan"

When not connected only: ::

    DEVICES_TO_DISABLE_ON_PRF_NOT_IN_USE="bluetooth nfc wifi wwan"

*Versions 1.10 through 1.9* ::

    DEVICES_TO_ENABLE_ON_AC="bluetooth nfc wifi wwan"

*Version 1.8 and older* when AC power is plugged in ::

    DEVICES_TO_ENABLE_ON_AC="bluetooth nfc wifi wwan"

Default when unconfigured: <none>


DEVICES_TO_ENABLE/DISABLE_ON_BAL
--------------------------------
Enables or disables builtin radio devices when changing to the *balanced* profile.
Possible values are as above.

*Version 1.11 and newer* ::

    DEVICES_TO_ENABLE_ON_BAL="bluetooth nfc wifi wwan"
    DEVICES_TO_DISABLE_ON_BAL="bluetooth nfc wifi wwan"
    DEVICES_TO_DISABLE_ON_BAL_NOT_IN_USE="bluetooth nfc wifi wwan" # when not connected only

*Versions 1.10 through 1.9* ::

    DEVICES_TO_DISABLE_ON_BAT="bluetooth nfc wifi wwan"
    DEVICES_TO_DISABLE_ON_BAT_NOT_IN_USE="bluetooth nfc wifi wwan" # when not connected only

*Version 1.8 and older* when changing to battery power ::

    DEVICES_TO_DISABLE_ON_BAT="bluetooth nfc wifi wwan"
    DEVICES_TO_DISABLE_ON_BAT_NOT_IN_USE="bluetooth nfc wifi wwan" # when not connected only

Default when unconfigured: <none>

DEVICES_TO_ENABLE/DISABLE_ON_SAV
--------------------------------
Enables or disables builtin radio devices when changing to the *power-saver* profile.
Possible values are as above.

*Version 1.11 and newer* ::

    DEVICES_TO_ENABLE_ON_BAL="bluetooth nfc wifi wwan"
    DEVICES_TO_DISABLE_ON_BAL="bluetooth nfc wifi wwan"
    DEVICES_TO_DISABLE_ON_BAL_NOT_IN_USE="bluetooth nfc wifi wwan" # when not connected only

*Versions 1.10 through 1.9* ::

    DEVICES_TO_DISABLE_ON_BAT="bluetooth nfc wifi wwan"

Default when unconfigured: <none>

.. note::

    Do not use both `DEVICES_TO_DISABLE_ON_*` and `DEVICES_TO_DISABLE_ON_BAL_NOT_IN_*`
    for the same radio device because `DEVICES_TO_DISABLE_ON_*` always takes precendence.


.. seealso::

    * Settings: :doc:`/settings/introduction`
    * FAQ: :doc:`/faq/radio`
    * `rfkill <https://www.kernel.org/doc/Documentation/rfkill.txt>`__
      – Kernel framework for switching radio devices
