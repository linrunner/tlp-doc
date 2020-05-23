Troubleshooting Guide
=====================
TLP's default settings are carefully chosen to avoid power saving to
interfere with normal laptop operation.

However kernel drivers do not implement power saving perfectly for all existing
hardware – possibly leading to:

* malfunctioning devices,
* system freezes,
* suspend/resume problems,
* system startup/shutdown/reboot problems

and the like.

TLP is not able to make kernel issues disappear but there is (almost) always a
workaround by tweaking TLP's :doc:`/settings/index`.

This document teaches how to isolate the causing device or power saving feature
and disable it.

Step 1: Update your system
--------------------------
Invoke your system's package manager and update all packages including the
Linux kernel. Reboot.

.. note::

    Linux Mint: make sure kernel and X updates are enabled in the update manager.

Step 2: Check the FAQ
---------------------

:doc:`/faq/index` contains many ready-to-use solutions for common problems with TLP.
Carefully check it for your symptoms.

Step 3: Disable TLP temporarily
-------------------------------
To determine if TLP is the source of your problem, disable TLP via
:doc:`Operation settings </settings/operation>`: ::

    TLP_ENABLE=0

Then reboot the system and check if your symptom disappeared.

.. important::

    In case of sporadic symptoms be sure to allow for enough testing
    time to reliably reproduce the problem!

If your problem is gone now, re-enable TLP ::

    TLP_ENABLE=1

and proceed to :ref:`tsg-power-source`.

Else your problem is not caused by TLP and you should seek help in an appropriate
internet forum or file a bug report against your Linux distribution.

.. _tsg-power-source:

Step 4: Determine affected power sources
----------------------------------------
Many of TLP's power saving features have different settings depending on the
actual power source, e.g.:

* AC – setting named `FEATURE_ON_AC`
* Battery – setting named `FEATURE_ON_BAT`

Examine your system if the symptom occurs on AC or battery or both. Use the
result when disabling a power saving feature below. Proceed with the next step.

Step 5: Is the causing device already known?
--------------------------------------------
When a symptom is specific enough to blame a particular device, start with
disabling associated feature(s) for the power source(s) identified in
:ref:`tsg-power-source`.

For functional device categories, e.g.

* :doc:`/settings/audio`
* :doc:`/settings/disks`
* :doc:`/settings/graphics`
* :doc:`/settings/network`

refer to :doc:`/settings/index` and :doc:`/faq/index` how to achieve this.

Bus oriented devices, e.g.

* :doc:`PCIe (Runtime Power Management) </settings/runtimepm>`
* :doc:`/settings/usb`

can be blacklisted, see :doc:`/settings/index` and :doc:`/faq/index` for the corresponding
sections.

When unsure if a device is PCIe or USB examine the output of
:command:`tlp-stat -e -u`. If you don't know the causing device or your problem
isn't solved by now, proceed to the next step.

Step 6: Isolate the causing device
----------------------------------
This step applies the following strategy to isolate a bus oriented device:

* Disable the feature completely
* Blacklist devices one by one
* Blacklist devices by driver

.. important::

    Make shure to reboot the computer after *every* configuration
    change for this step!

6.1 PCIe devices
^^^^^^^^^^^^^^^^
.. rubric:: Disable Runtime Power Management completely

Change both related config lines: ::

    RUNTIME_PM_ON_AC=
    RUNTIME_PM_ON_BAT=

When the problem disappears, uncomment above lines and continue with the
next section. Otherwise the cause is not a PCIe device – proceed to
:ref:`tsg-usb`.

.. rubric:: Blacklist single devices

Enter every PCIe device address into :ref:set-runtimepm-blacklist` – but only
one device at a time!

As soon as the problem disappears, you have identified the causing PCIe device
and are finished. Otherwise the cause is not a PCIe device – proceed to
:ref:`tsg-usb`.

.. rubric:: Blacklist drivers

As an alternative to device blacklisting, you may blacklist all devices attached
to a particular driver by means of :ref:`set-runtimepm-driver-blacklist`.

If the problem isn't solved by now, proceed to :ref:`tsg-usb`.

.. _tsg-usb:

6.2 USB devices
^^^^^^^^^^^^^^^
.. rubric:: Disable USB autosuspend completely

Disable the feature as follows (refer to :ref:`set-usb-autosuspend`): ::

    USB_AUTOSUSPEND=0

When the problem disappears, reenable the feature and continue with the
next section. Otherwise the causing device cannot be identified – proceed to
:ref:`tsg-kernel`.

.. rubric:: Blacklist single devices

Enter every USB device ID into :ref:set-usb-blacklist` – but only one device at
a time!

As soon as the problem disappears, you have identified the causing USB device
and are finished. Otherwise an causing device cannot be found – proceed to the
next section.

.. _tsg-kernel:

Step 7: Upgrade kernel or firmware
----------------------------------
When all else fails, try to:

* Upgrade the Linux kernel to the latest version,
  e.g. use mainline kernel (Ubuntu) or backports (Debian) or the equivalent for
  your distribution or compile Linus' kernel yourself
* Update BIOS/UEFI for your laptop
* Update firmware for the causing device (if possible)

.. note::

    Consult adequate forums to learn how to do this.
