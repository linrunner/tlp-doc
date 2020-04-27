Battery Charge Thresholds
=========================

.. include:: ../include/disc-battery-features.rst

START/STOP_CHARGE_THRESH_BATx
-----------------------------
::

    START_CHARGE_THRESH_BAT0=75
    STOP_CHARGE_THRESH_BAT0=80

    START_CHARGE_THRESH_BAT1=75
    STOP_CHARGE_THRESH_BAT1=80

Set battery charge thresholds for main battery (BAT0) and auxiliary/Ultrabay
battery (BAT1). Values are given as a percentage of the full capacity. A value of
0 is translated to the hardware defaults 96/100%.

.. note::

    * Charging starts upon connecting AC power, but only if the charge level is
      below the start threshold (`START_CHARGE_TRESH_BATx`)
    * Charging stops when reaching the stop threshold (`STOP_CHARGE_TRESH_BATx`)
    * If, however you connect the AC adapter when the charge level is above the
      start threshold, then it will not charge

Hints:

* Thresholds always go with a lower usable battery capacity, therefore the settings
  are disabled by default and must be enabled explicitly by removing the leading `#`
* ThinkPad T420(s)/T520/W520/X220 (and all newer models):
  check :ref:`Erratic Battery Behaviour <faq-erratic-battery-behavior>`
* For further questions concerning charge thresholds please visit the FAQ:
  :doc:`/faq/battery`

RESTORE_THRESHOLDS_ON_BAT
-------------------------
::

    RESTORE_THRESHOLDS_ON_BAT=1

Restore configured charge thresholds when AC is unplugged:

* 0 – disable
* 1 – enable

Default when unconfigured: 0

Hint: after the commands :command:`tlp fullcharge/recalibrate` the charge thresholds
will stay at the hardware defaults 96/100% until the next reboot. Use this
feature to restore them prematurely.

NATACPI/TPACPI/TPSMAPI_ENABLE
-----------------------------
::

    NATACPI_ENABLE=1
    TPACPI_ENABLE=1
    TPSMAPI_ENABLE=1

Control battery feature drivers:

* 0 – disable
* 1 – enable

Default when unconfigured: 1 (all)

.. seealso::

    * `tp-smapi <https://www.thinkwiki.org/wiki/Tp_smapi>`_
      – tp-smapi documentation at thinkwiki.org
    * `tpacpi-bat <https://github.com/teleshoes/tpacpi-bat>`_
      – tool to provide battery charge thresholds and recalibration for
      newer ThinkPads (X220/T420 and later)
