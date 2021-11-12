.. important::

    All input devices (driver `usbhid`), `libsane`-supported scanners and
    *(as of version 1.4)* audio devices get excluded by default.
    It's therefore unnecessary to put them on the :ref:`set-usb-denylist`.
    To circumvent the default for certain devices enter the IDs into
    :ref:`set-usb-allowlist`.
