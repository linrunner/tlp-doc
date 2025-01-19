USB Devices
===========

USB device not working
----------------------
Symptom: some USB devices do not work reliable when TLP activates USB autosuspend
mode.

Solution: lookup the corresponding USB device ID with :command:`lsusb`, add it to
:ref:`set-usb-denylist` and reconnect the device.

.. include:: /include/usb-excluded-devices.rst


.. _faq-usb-not-working-on-battery:

USB devices not working on battery power
----------------------------------------
Symptoms:

* Newly inserted USB devices are not recognized on battery power
* Bluetooth, Webcam, and other USB bus devices stop working after
  switching to battery power

Affected hardware: ThinkPad E595 AMD, T495 AMD, T495s AMD.

Related issues: `#436 <https://github.com/linrunner/TLP/issues/436>`_,
`#587 <https://github.com/linrunner/TLP/issues/587>`_.

Workarounds (please try one by one):

* Disable runtime power management for USB3 controllers by
  adding `xhci_hcd` to the denylist *and reboot* ::

      RUNTIME_PM_DRIVER_DENYLIST="mei_me nouveau radeon xhci_hcd"

* Disable runtime power management for AHCI devices an execute :command:`tlp start` ::

      AHCI_RUNTIME_PM_ON_BAT=on

* Disable runtime power management for AHCI the disk controllers by
  adding `ahci` to the denylist *and reboot* ::

      RUNTIME_PM_DRIVER_DENYLIST="mei_me nouveau radeon ahci"

.. seealso::

   * :ref:`set-disks-ahci-runtime-pm`
   * :ref:`RUNTIME_PM_DRIVER_BLACKLIST`
   * :ref:`faq-resume-freeze`

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

Then reconnect the device.


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
