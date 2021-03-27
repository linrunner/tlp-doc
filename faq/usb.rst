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
Affected hardware: ThinkPad T495
(see `Issue #436 <https://github.com/linrunner/TLP/issues/436>`_).

Workaround: blacklist the USB controllers with ::

    RUNTIME_PM_BLACKLIST="06:00.3 06:00.4"


Smartphone does not charge when connected
-----------------------------------------
Solution: exclude the smartphone from autosuspend and reconnect the device.

Version 1.0 and higher: configure ::

    USB_BLACKLIST_PHONE=1

Version 0.9 and lower: lookup the smartphone's USB device ID with :command:`lsusb`
and add it to :ref:`set-usb-blacklist`.
