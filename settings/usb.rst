.. _set-usb:

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

Default when unconfigured: 0

.. note::

    Input devices like mice and keyboards as well as scanners handled by
    `libsane` are exluded by default.

.. _set-usb-blacklist:

USB_BLACKLIST
-------------
::

    USB_BLACKLIST="1111:2222 3333:4444"

Exclude USB device IDs from autosuspend mode. Useful for devices having difficulties
in waking up from autosuspend. Use :command:`tlp-stat -u` to determine IDs.
Multiple IDs are separated with blanks.

.. note::

    All input devices (driver `usbhid`) and [as of version 1.2] `libsane`-supported
    scanners get excluded by default. It's therefore unnecessary to put them on
    the :ref:`set-usb-blacklist`. To circumvent the default for certain devices
    enter the IDs into :ref:`set-usb-whitelist`.

.. _set-usb-blacklist-btusb:

USB_BLACKLIST_BTUSB
-------------------
::

    USB_BLACKLIST_BTUSB=1

Exclude bluetooth devices from autosuspend mode:

* 0 – do not exclude
* 1 – exclude

Default when unconfigured: 0

.. note::

    This feature is intended to solve stability issues with bluetooth connections.

USB_BLACKLIST_PHONE
-------------------
::

    USB_BLACKLIST_PHONE=1

Exclude smartphones from autosuspend mode to enable charging:

* 0 – do not exclude
* 1 – exclude

Default when unconfigured: 0

USB_BLACKLIST_PRINTER
---------------------
::

    USB_BLACKLIST_PRINTER=1

Exclude printers from autosuspend mode:

* 0 – do not exclude
* 1 – exclude

Default when unconfigured: 1

USB_BLACKLIST_WWAN
------------------
::

    USB_BLACKLIST_WWAN=0

Exclude builtin WWAN devices from autosuspend mode:

* 0 – do not exclude
* 1 – exclude

Default when unconfigured: 0

.. note::

    This feature is implemented by an internal blacklist currently matching
    cards from Qualcomm, Ericsson and Sierra. Enter WWAN devices not recognized
    into :ref:`set-usb-blacklist`.

.. _set-usb-whitelist:

USB_WHITELIST
-------------

USB_WHITELIST="5555:6666 7777:8888"

Re-enable autosuspend mode for USB device IDs already excluded by any of the
settings above (whitelist always wins). Use :command:`tlp-stat -u` to determine
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
