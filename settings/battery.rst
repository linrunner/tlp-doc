Battery Charge Thresholds
=========================
The purpose of battery charge thresholds is to reduce loss of capacity caused
by ongoing use of the battery. Firstly, this can be achieved by limiting the
maximum charge level to below 100% (stop threshold). Secondly, you can set
that after a short discharge, charging does not immediately start again
when the power supply is plugged in (start threshold).

.. rubric:: How does it work?

Battery charging is a process controlled by the embedded controller (EC)
of your laptop. This makes the process work even when the laptop is switched off
or no operating system is running. The process works as follows:

* Charging starts upon connecting AC power, but only if the battery charge
  level is below the start threshold (`START_CHARGE_TRESH_BATx`); when the
  charge level is above the start threshold, then it will `not` charge
* Charging stops when reaching the stop threshold (`STOP_CHARGE_TRESH_BATx`)

You cannot change the basic behavior described above, because it is hard-coded
into the EC firmware by the manufacturer. However, you can control it by setting
thresholds using TLP.

.. rubric::  What charge thresholds do `not`

* They do not exercise any control over the discharge of the battery,
  this depends solely on whether AC is connected and if the laptop is switched on
* In particular, with AC connected, a battery with a charge level higher than
  the stop threshold will not be discharged to the stop threshold, nor will
  there be a (cyclic) discharge down to the start threshold

.. seealso::

    For further questions concerning charge thresholds please visit the FAQ:
    :doc:`/faq/battery`.

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

.. important::

    * You must always specify both thresholds - *start and stop* - for a battery,
      otherwise TLP will ignore the setting completely (and silently).
    * To use only the start threshold set the stop threshold to 100%: ::

        START_CHARGE_THRESH_BAT0=75
        STOP_CHARGE_THRESH_BAT0=100

Hints:

* Thresholds always go with a lower usable battery capacity, therefore the settings
  are disabled by default and must be enabled explicitly by removing the leading `#`
* ThinkPad T420(s)/T520/W520/X220 (and all newer models):
  check :ref:`Erratic Battery Behaviour <faq-erratic-battery-behavior>`


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
