Battery Care
============
.. include:: ../include/battery-care-explain.rst
.. include:: ../include/battery-care-scope.rst


START/STOP_CHARGE_THRESH_BATx
-----------------------------
::

    START_CHARGE_THRESH_BAT0=75
    STOP_CHARGE_THRESH_BAT0=80

    START_CHARGE_THRESH_BAT1=75
    STOP_CHARGE_THRESH_BAT1=80

Set battery charge thresholds for main/internal battery `BAT0` and auxiliary/Ultrabay
battery `BAT1`:

.. include:: ../include/charge-threshold-effect.rst

* Thresholds always go with a lower usable battery capacity, therefore the settings
  are disabled by default and must be enabled explicitly by removing the leading `#`

.. important::

    You must always specify both charge thresholds - *start and stop* - for a battery,
    otherwise TLP will ignore the setting completely (*and up to version 1.3.1
    silently*). If you want to apply only one threshold (or your hardware
    does support only one), then use the value `0` to skip the other one.

RESTORE_THRESHOLDS_ON_BAT
-------------------------
::

    RESTORE_THRESHOLDS_ON_BAT=1

Restore configured charge thresholds when AC is unplugged:

* 0 – disable
* 1 – enable

Default when unconfigured: 0

Hint: after the commands :command:`tlp fullcharge/recalibrate` the charge thresholds
will stay at the vendor specific defaults until the next reboot. Use this
feature to restore them prematurely.

NATACPI/TPACPI/TPSMAPI_ENABLE
-----------------------------
::

    NATACPI_ENABLE=1
    TPACPI_ENABLE=1
    TPSMAPI_ENABLE=1

Control battery care drivers:

* 0 – disable
* 1 – enable

Default when unconfigured: 1 (all)

Scope:

    * `NATACPI`: all supported laptops
    * `TPACPI_ENABLE`, `TPSMAPI_ENABLE`: ThinkPad specific
