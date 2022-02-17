tlp
---
.. topic:: Purpose

    Apply TLP's settings and change mode of operation.

Start or restart TLP
^^^^^^^^^^^^^^^^^^^^
Apply all configured settings according to the actual power source: ::

    sudo tlp start

.. note:: Also use this command to apply changes after editing the configuration.

Battery Mode
^^^^^^^^^^^^
Apply the battery settings profile and enter manual mode: ::

    sudo tlp bat

Hint: manual mode means that changes to the power source will be ignored until
the next reboot or :command:`tlp start` is issued to resume automatic mode.

AC Mode
^^^^^^^
Apply the AC settings profile and enter manual mode: ::

    sudo tlp ac

Hint: manual mode means that changes to the power source will be ignored until
the next reboot or :command:`tlp start` is issued to resume automatic mode.

USB Autosuspend
^^^^^^^^^^^^^^^
Apply autosuspend mode for all attached USB devices except those excluded by
default or in the configuration: ::

    sudo tlp usb

Optical Drive
^^^^^^^^^^^^^
Power off optical drive in MediaBay or Ultrabay: ::

    sudo tlp bayoff

Hints:

* Re-power the drive by releasing and reinserting the drive slot/Ultrabay eject lever; on newer models push the media eject button
* Devices other than optical drives – in particular hard disk drives – are not affected by this command

.. _cmd-tlp-battery-features:

Battery Care
^^^^^^^^^^^^
.. include:: ../include/battery-care-scope.rst


Change battery charge thresholds temporarily
""""""""""""""""""""""""""""""""""""""""""""
::

    sudo tlp setcharge [<START_CHARGE_THRESH> <STOP_CHARGE_THRESH>] [BAT0|BAT1|BAT<x>|CMB0|CMB1]

Changes the charge thresholds for the battery to the given values.

.. include:: ../include/charge-threshold-values.rst

Configured thresholds will be restored at the next boot or by using
:command:`tlp setcharge` again but without the threshold arguments.

Example: ::

    sudo tlp setcharge 70 90 BAT0

Applies thresholds of 70/90% to battery `BAT0`.

.. note::

    :command:`tlp setcharge` changes the charge thresholds only temporarily.
    To make the change permanent, you must activate or change the related settings
    in the config file. Refer to :doc:`/settings/battery`.

Charge battery to full capacity
"""""""""""""""""""""""""""""""
::

    sudo tlp fullcharge [BAT0|BAT1|BAT<x>|CMB0|CMB1]

This is done by applying vendor presets to the charge thresholds temporarily.
Configured thresholds will be restored at the next boot or by using
:command:`tlp setcharge` without the threshold arguments.

Hint: after setting the thresholds the command terminates; it does not wait for
the charge to complete.

Example: ::

    sudo tlp fullcharge BAT1

Charges battery `BAT1` to full capacity.

Charge battery to the stop charge threshold once
""""""""""""""""""""""""""""""""""""""""""""""""
*ThinkPads only*

::

     sudo tlp chargeonce [BAT0|BAT1]

This is done by temporarily lifting the start charge threshold.
The configured start charge threshold will be restored at the next boot or by using
:command:`tlp setcharge` without the threshold arguments.

Hint: after setting he thresholds the command terminates; it does not wait for
the charge to complete.

Discharge battery on AC power
"""""""""""""""""""""""""""""
*ThinkPads only*

::

    sudo tlp discharge [BAT0|BAT1]

`BAT0` selects the main/internal battery, `BAT1` the auxiliary/Ultrabay battery for discharge.
The command continously shows remaining capacity and estimated discharge time.
Discharging may be stopped at any time with :kbd:`Control-C`.

Hints:

* The command terminates automatically when the battery is discharged completely
* The command needs the AC power supply plugged in
* Normal use of the ThinkPad is possible during the discharge process
* ThinkPads with two batteries: the battery controller can only handle one
  battery at a time; while discharging one battery with this command the other
  battery can neither be charged nor discharged
* When encountering problems, see the FAQ: :doc:`/faq/battery`

Recalibrate battery on AC power
"""""""""""""""""""""""""""""""
*ThinkPads only*

::

    sudo tlp recalibrate [BAT0|BAT1]

This command works as follows:

* Applies vendor presets to the charge thresholds
* Discharges the selected battery completely (see description of
  :command:`tlp discharge` above)
* When discharging is complete the command terminates; it does not wait for the
  charge to complete
* Important: to complete the recalibration process, let the battery charge to
  100 % subsequently (you may power off but not remove AC power)

Example: ::

    sudo tlp recalibrate BAT0

Recalibrates the main battery (BAT0).

Hints:

* Configured thresholds will be restored at the next boot or by using
  :command:`tlp setcharge` without the threshold arguments
* ThinkPads with two batteries: the battery controller can only handle one
  battery at a time; while discharging one battery with this command the other
  battery can neither be charged nor discharged
* Recalibration forces the battery pack to update the `energy_full` or
  `charge_full` information shown by :command:`tlp-stat -b`
* Recalibration does not repair defective or worn out batteries

Disk IDs
^^^^^^^^
::

    tlp diskid

Shows the IDs of all attached disk drives.
