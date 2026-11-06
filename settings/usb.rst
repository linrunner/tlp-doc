USB Autosuspend
===============

.. rubric:: Scope:

* Attached and subsequently plugged USB devices

.. _set-usb-autosuspend:

USB_AUTOSUSPEND
---------------
Controls autosuspend for USB devices on boot and when plugged.
Possible values:

* 1 – enable
* 0 – disable

::

    USB_AUTOSUSPEND=1

Default when unconfigured: 1


.. _set-usb-denylist:
.. _USB_BLACKLIST:

USB_DENYLIST
-------------
.. include:: /include/usb-excluded-devices.rst

Exclude USB device IDs from autosuspend. Useful for devices having difficulties
in waking up from autosuspend. Use :command:`tlp-stat -u` to the determine IDs.
Multiple IDs are separated with blanks.

::

    USB_DENYLIST="1111:2222 3333:4444"

Default when unconfigured: <none>

USB_EXCLUDE_AUDIO
-----------------
Exclude audio devices from autosuspend:

* 0 – do not exclude
* 1 – exclude

::

    USB_EXCLUDE_AUDIO=1

Default when unconfigured: 1


.. _set-usb-exclude-btusb:
.. _USB_BLACKLIST_BTUSB:

USB_EXCLUDE_BTUSB
-----------------
Exclude bluetooth devices from autosuspend:

* 0 – do not exclude
* 1 – exclude

::

    USB_EXCLUDE_BTUSB=1

Default when unconfigured: 0

.. note::

    This feature is intended to solve stability issues with bluetooth connections.


.. seealso::

    :ref:`faq-usb-bt-exclude`


.. _USB_BLACKLIST_PHONE:

USB_EXCLUDE_PHONE
-----------------
Exclude smartphones from autosuspend to enable charging:

* 0 – do not exclude
* 1 – exclude

::

    USB_EXCLUDE_PHONE=1

Default when unconfigured: 0


.. _USB_BLACKLIST_PRINTER:

USB_EXCLUDE_PRINTER
-------------------
Exclude printers from autosuspend:

* 0 – do not exclude
* 1 – exclude

::

    USB_EXCLUDE_PRINTER=1

Default when unconfigured: 1


.. _USB_BLACKLIST_WWAN:

USB_EXCLUDE_WWAN
----------------
Exclude builtin WWAN devices from autosuspend:

* 0 – do not exclude
* 1 – exclude

::

    USB_EXCLUDE_WWAN=0

Default when unconfigured: 0

.. note::

    This feature is implemented by an internal denylist currently matching
    cards from Qualcomm, Ericsson and Sierra. Enter WWAN devices not recognized
    into :ref:`set-usb-denylist`.


.. _set-usb-allowlist:
.. _USB_WHITELIST:

USB_ALLOWLIST
-------------
Re-enable autosuspend for USB device IDs already excluded by any of the
settings above (allowlist always wins). Use :command:`tlp-stat -u` to determine
the IDs. Multiple IDs are separated with blanks.

::

    USB_ALLOWLIST="5555:6666 7777:8888"

Default when unconfigured: <none>

.. seealso::

    * Settings: :doc:`/settings/introduction`
    * FAQ: :doc:`/faq/usb`
    * `USB autosuspend <https://www.kernel.org/doc/Documentation/usb/power-management.txt>`_ – Kernel documentation
