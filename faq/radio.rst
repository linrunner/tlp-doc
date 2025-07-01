Radio Devices: Bluetooth, Wi-Fi, WWAN
=====================================

Slow or unstable Wi-Fi on battery power
---------------------------------------
Cause: kernel driver does not implement power saving properly.

Solution: disable power saving on battery in configuration ::

    WIFI_PWR_ON_BAT=off

Apply with :command:`tlp start`.

Wi-Fi power saving is activated despite being disabled in TLP's configuration
-----------------------------------------------------------------------------
Cause: conflict with `NetworkManager`.

Solution: remove the file **/etc/NetworkManager/conf.d/default-wifi-powersave-on.conf**.


.. _faq-bluetooth-unstable:

Bluetooth devices hang, disconnect or do not pair
-------------------------------------------------
Cause: most internal laptop bluetooth devices and all external bluetooth
dongles are USB devices. Some do not implement autosuspend mode properly,
giving trouble to connected devices or preventing discovery or pairing.

Solution: configure ::

    USB_EXCLUDE_BTUSB=1

Apply with :command:`tlp usb`.


Bluetooth stops working after change to battery power
-----------------------------------------------------
See :ref:`faq-usb-not-working-on-battery`.


USB_EXCLUDE_BTUSB=1 does not disable autosuspend
--------------------------------------------------
Refer to :ref:`faq-usb-bt-exclude`.


Bluetooth is not disabled upon system startup
---------------------------------------------
Probable cause: your desktop environment's bluetooth applet – or some other
installed software – re-enables the bluetooth device after TLP disabled it.

Solution: disable the relevant applet's setting or disable/remove the causing
applet. For XFCE/blueman see
`Disable Bluetooth Auto Power-on in Blueman <https://winaero.com/blog/disable-bluetooth-auto-power-blueman/>`_.


Radio states are not as expected after boot or a configured event
-----------------------------------------------------------------
Cause 1: conflict with other settings, for instance :ref:`set-radio-disable-on` ff.

Solution: don't use `RESTORE_DEVICE_STATE_ON_STARTUP=1` and
:ref:`set-radio-disable-on` ff. simultaneously.

----

Cause 2: other conflicting settings from :doc:`/settings/radio` and/or :doc:`/settings/rdw`.

Solution: check if you configured contradictory instructions, for example

::

    DEVICES_TO_DISABLE_ON_BAT="wifi"
    DEVICES_TO_ENABLE_ON_LAN_DISCONNECT="wifi"

may not turn on Wi-Fi when undocking and changing to battery power.

----

Cause 3: systemd implements its radio state restore scheme.

Symptoms: :command:`tlp-stat -s` shows ::

    Warning: systemd-rfkill.service is not masked, radio device switching may not work as configured.
    >>> Invoke 'systemctl enable systemd-rfkill.service to correct this!

and/or ::

    Warning: systemd-rfkill.socket is not masked, radio device switching may not work as configured.
    >>> Invoke 'systemctl enable systemd-rfkill.socket to correct this!

`systemd-rfkill.service/.socket` are part of systemd. Their purpose is
to restore the state of the radio devices from the last shutdown at system startup.
In case you enabled settings from :doc:`/settings/radio` or :doc:`/settings/rdw`
this may lead to a conflict that produces unpredictable results.

Solution: use either `RESTORE_DEVICE_STATE_ON_STARTUP=1` and mask systemd-rfkill.service
and systemd-rfkill.socket or use systemd's approach but not both.

----

Cause 4: the :doc:`/settings/rdw` is not installed.

Solution: install the package **tlp-rdw**, see :doc:`/installation/index`.

