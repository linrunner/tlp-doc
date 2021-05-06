USB Devices
===========

USB device does not work
------------------------
Symptom: some USB devices do not work reliable when TLP activates USB autosuspend
mode.

Solution: lookup the corresponding USB device ID with :command:`lsusb`, add it to
:ref:`set-usb-blacklist` and reconnect the device.

.. include:: /include/usb-excluded-devices.rst


Newly inserted USB devices not recognized in battery mode
---------------------------------------------------------
Affected hardware: ThinkPad T495 AMD, T495s AMD
(see `Issue #436 <https://github.com/linrunner/TLP/issues/436>`_).

Workaround: blacklist the USB controllers with ::

    RUNTIME_PM_BLACKLIST="06:00.3 06:00.4" # T495 AMD
    RUNTIME_PM_BLACKLIST="05:00.3 05:00.4" # T495s AMD


.. _faq-usb-bt-exclude:

USB_BLACKLIST_BTUSB=1 does not disable autosuspend
--------------------------------------------------
Symptom: the USB bluetooth device has autosuspend enabled after boot or
after suspend/resume. :command:`tlp-stat -u` shows: ::

    Bus 001 Device 004 ID 8087:0a2b control = auto, autosuspend_delay_ms = 2000 -- Intel Corp.  (btusb)

Cause: the `btusb` kernel driver itself enforces autosuspend (Kconfig
`CONFIG_BT_HCIBTUSB_AUTOSUSPEND=y`).

Solution: add the boot option `btusb.enable_autosuspend=0` to your GRUB
configuration.


Smartphone does not charge when connected
-----------------------------------------
Solution: exclude the smartphone from autosuspend and reconnect the device.
Configure ::

    USB_BLACKLIST_PHONE=1
