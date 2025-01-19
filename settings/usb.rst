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
.. include:: /include/usb-excluded-devices.rst

::

    USB_DENYLIST="1111:2222 3333:4444"

Exclude USB device IDs from autosuspend mode. Useful for devices having difficulties
in waking up from autosuspend. Use :command:`tlp-stat -u` to determine IDs.
Multiple IDs are separated with blanks.


USB_EXCLUDE_AUDIO
-----------------
::

    USB_EXCLUDE_AUDIO=1

Exclude audio devices from autosuspend mode:

* 0 – do not exclude
* 1 – exclude

Default when unconfigured: 1


.. _set-usb-exclude-btusb:
.. _USB_BLACKLIST_BTUSB:

USB_EXCLUDE_BTUSB
-----------------
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
::

    USB_EXCLUDE_PHONE=1

Exclude smartphones from autosuspend mode to enable charging:

* 0 – do not exclude
* 1 – exclude

Default when unconfigured: 0


.. _USB_BLACKLIST_PRINTER:

USB_EXCLUDE_PRINTER
-------------------
::

    USB_EXCLUDE_PRINTER=1

Exclude printers from autosuspend mode:

* 0 – do not exclude
* 1 – exclude

Default when unconfigured: 1


.. _USB_BLACKLIST_WWAN:

USB_EXCLUDE_WWAN
----------------
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
::

    USB_ALLOWLIST="5555:6666 7777:8888"

Re-enable autosuspend mode for USB device IDs already excluded by any of the
settings above (allowlist always wins). Use :command:`tlp-stat -u` to determine
IDs. Multiple IDs are separated with blanks.


USB_AUTOSUSPEND_DISABLE_ON_SHUTDOWN
-----------------------------------
*This parameter was removed in version 1.7*

::

    USB_AUTOSUSPEND_DISABLE_ON_SHUTDOWN=1

Disables USB autosuspend mode upon system shutdown. This is intended as a
workaround if suspended USB devices disturb the shutdown process.

Default when unconfigured: 0


.. seealso::

    * Settings: :doc:`/settings/introduction`
    * FAQ: :doc:`/faq/usb`
    * `USB autosuspend <https://www.kernel.org/doc/Documentation/usb/power-management.txt>`_ – Kernel documentation
