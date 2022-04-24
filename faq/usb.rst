USB Devices
===========

USB device does not work
------------------------
Symptom: some USB devices do not work reliable when TLP activates USB autosuspend
mode.

Solution: lookup the corresponding USB device ID with :command:`lsusb`, add it to
:ref:`set-usb-denylist` and reconnect the device.

.. include:: /include/usb-excluded-devices.rst

Nvidia Jetson devices not detected in recovery mode
-------------------------------------------------------
Symptom: the device cannot be flashed with SDK Manager, the error message reads:

.. code-block:: none

    Could not detect target hardware. Please make sure the target hardware is
    connected and is in recovery mode, click OK and RETRY.

Solution: add the USB device ID to :ref:`set-usb-denylist` ::

    USB_DENYLIST="0955:7c18"

and reconnect the device. Several Jetson devices with different IDs exist.
Lookup the corresponding ID with :command:`lsusb | grep '0955'` or check the
list referenced below.

References:

* `PR #628 <https://github.com/linrunner/TLP/issues/628>`_
* `List of Nvida Jetson USB device IDs (incomplete) <https://docs.nvidia.com/jetson/archives/l4t-archived/l4t-325/index.html#page/Tegra%20Linux%20Driver%20Package%20Development%20Guide/quick_start.html#wwpID0EMHA>`_

Newly inserted USB devices not recognized in battery mode
---------------------------------------------------------
Affected hardware: ThinkPad T495 AMD, T495s AMD
(see `Issue #436 <https://github.com/linrunner/TLP/issues/436>`_).

Workaround: denylist the USB controllers with ::

    RUNTIME_PM_DENYLIST="06:00.3 06:00.4" # T495 AMD
    RUNTIME_PM_DENYLIST="05:00.3 05:00.4" # T495s AMD

*Version 1.3.1 and lower: use RUNTIME_PM_BLACKLIST instead*

.. note::

    * Other hardware may require different values: check the output of
      :command:`tlp-stat -e` for lines ending in 'USB controller, xhci_hcd'
      to identify corresponding devices
    * Another workaround may also help: :ref:`faq-resume-freeze`

.. _faq-usb-bt-exclude:

USB_EXCLUDE_BTUSB=1 does not disable autosuspend
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
Solution: exclude the smartphone from autosuspend by configuring ::

    USB_EXLUDE_PHONE=1

*Version 1.3.1 and lower: use USB_BLACKLIST_PHONE instead*

Then reconnect the device.
