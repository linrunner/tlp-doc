USB
===

.. rubric:: Scope:

* Attached and subsequently plugged USB devices

.. _set-usb-autosuspend:

USB_AUTOSUSPEND
---------------
::

    USB_AUTOSUSPEND=1

Set autosuspend mode for USB devices on boot and when plugged.
Possible values:

* 1 – enable
* 0 – disable

Default when unconfigured: 1


.. _set-usb-denylist:
.. _USB_BLACKLIST:

USB_DENYLIST
-------------
*This parameter was renamed with version 1.4. In 1.3.1 and below it is called
USB_BLACKLIST. 1.4 and higher also recognize the old name.*

.. include:: /include/usb-excluded-devices.rst

::

    USB_DENYLIST="1111:2222 3333:4444"

Exclude USB device IDs from autosuspend mode. Useful for devices having difficulties
in waking up from autosuspend. Use :command:`tlp-stat -u` to determine IDs.
Multiple IDs are separated with blanks.


.. _set-usb-exclude-btusb:
.. _USB_BLACKLIST_BTUSB:

USB_EXCLUDE_BTUSB
-----------------
*This parameter was renamed with version 1.4. In 1.3.1 and below it is called
USB_BLACKLIST_BTUSB. 1.4 and higher also recognize the old name.*

::

    USB_EXCLUDE_BTUSB=1

Exclude bluetooth devices from autosuspend mode:

* 0 – do not exclude
* 1 – exclude

Default when unconfigured: 0

.. note::

    This feature is intended to solve stability issues with bluetooth connections.


.. seealso::

    :ref:`faq-usb-bt-exclude`


.. _USB_BLACKLIST_PHONE:

USB_EXCLUDE_PHONE
-----------------
*This parameter was renamed with version 1.4. In 1.3.1 and below it is called
USB_BLACKLIST_PHONE. 1.4 and higher also recognize the old name.*

::

    USB_EXCLUDE_PHONE=1

Exclude smartphones from autosuspend mode to enable charging:

* 0 – do not exclude
* 1 – exclude

Default when unconfigured: 0


.. _USB_BLACKLIST_PRINTER:

USB_EXCLUDE_PRINTER
-------------------
*This parameter was renamed with version 1.4. In 1.3.1 and below it is called
USB_BLACKLIST_PRINTER. 1.4 and higher also recognize the old name.*

::

    USB_EXCLUDE_PRINTER=1

Exclude printers from autosuspend mode:

* 0 – do not exclude
* 1 – exclude

Default when unconfigured: 1


.. _USB_BLACKLIST_WWAN:

USB_EXCLUDE_WWAN
----------------
*This parameter was renamed with version 1.4. In 1.3.1 and below it is called
USB_BLACKLIST_WWAN. 1.4 and higher also recognize the old name.*

::

    USB_EXCLUDE_WWAN=0

Exclude builtin WWAN devices from autosuspend mode:

* 0 – do not exclude
* 1 – exclude

Default when unconfigured: 0

.. note::

    This feature is implemented by an internal denylist currently matching
    cards from Qualcomm, Ericsson and Sierra. Enter WWAN devices not recognized
    into :ref:`set-usb-denylist`.


.. _set-usb-allowlist:
.. _USB_WHITELIST:

USB_ALLOWLIST
-------------
*This parameter was renamed with version 1.4. In 1.3.1 and below it is called
USB_WHITELIST. 1.4 and higher also recognize the old name.*

::

    USB_ALLOWLIST="5555:6666 7777:8888"

Re-enable autosuspend mode for USB device IDs already excluded by any of the
settings above (allowlist always wins). Use :command:`tlp-stat -u` to determine
IDs. Multiple IDs are separated with blanks.


USB_AUTOSUSPEND_DISABLE_ON_SHUTDOWN
-----------------------------------
::

    USB_AUTOSUSPEND_DISABLE_ON_SHUTDOWN=1

Disables USB autosuspend mode upon system shutdown. This is intended as a
workaround if suspended USB devices disturb the shutdown process.

Default when unconfigured: 0

.. seealso::

    * `USB autosuspend <https://www.kernel.org/doc/Documentation/usb/power-management.txt>`_ – Kernel documentation
