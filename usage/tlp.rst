tlp
---
.. topic:: Purpose

    Apply TLP's settings and change mode of operation.

Start or restart TLP
^^^^^^^^^^^^^^^^^^^^
Start TLP and apply profile for the current power source: ::

    sudo tlp start

.. note:: Also use this command to apply changes after editing the configuration.


.. _usage-tlp-profiles:

TLP Profiles
^^^^^^^^^^^^
*Version 1.9 and newer*

Apply the *performance* profile): ::

    sudo tlp performance

Apply the *balanced* profile: ::

        sudo tlp balanced

Apply the *power-saver* profile: ::

    sudo tlp power-saver


.. _usage-tlp-manual:

Manual Mode
^^^^^^^^^^^^
Manual mode means that changes to the power source will be ignored until
the next reboot. Instead, :command:`tlp start` can also be used to return
to automatic mode.

Apply the TLP profile for AC power and enter manual mode: ::

    sudo tlp ac

Apply the TLP profile for battery power and enter manual mode: ::

    sudo tlp bat


USB Autosuspend
^^^^^^^^^^^^^^^
Apply autosuspend to all attached USB devices except those excluded by
default or via configuration: ::

    sudo tlp usb

Optical Drive
^^^^^^^^^^^^^
Power off optical drive in MediaBay/Ultrabay: ::

    sudo tlp bayoff

Hints:

* Re-power the drive by releasing and reinserting the drive slot/Ultrabay eject lever; on newer models push the media eject button
* Devices other than optical drives – in particular hard disk drives – are not affected by this command

.. _cmd-tlp-battery-features:

Battery Care
^^^^^^^^^^^^
.. include:: ../include/battery-care-scope.rst

.. note::

    For laptops with two batteries, the secondary battery must be specified as a command
    parameter in order to select it. In many cases  the  main  battery will be `BAT0`,
    the secondary battery `BAT1`. When in doubt, check the output of :command:`tlp-stat -b`,
    which lists all batteries.

Change battery charge thresholds
""""""""""""""""""""""""""""""""
::

    sudo tlp setcharge [<start threshold> <stop threshold>] [<battery>]

Change the charge thresholds for the battery to the given values.
Configured thresholds will be restored at the next boot or by using
:command:`tlp setcharge` again but without the threshold arguments.

Example: ::

    sudo tlp setcharge 70 90 BAT0

Applies thresholds of 70/90% to battery `BAT0`.

.. note::

    :command:`tlp setcharge` changes the charge thresholds only temporarily.
    To make the change permanent, you must activate or change the related settings
    in the config file. Refer to :doc:`/settings/battery`.


.. _cmd-tlp-fullcharge:

Charge battery to full capacity
"""""""""""""""""""""""""""""""
::

    sudo tlp fullcharge [<battery>]

Remove the charge thresholds by reverting them to the vendor specific defaults.
Configured thresholds will be restored at the next boot or by using
:command:`tlp setcharge` without the threshold arguments.

Hint: after setting the thresholds the command terminates. It does not wait for
the charge to complete.

Example: ::

    sudo tlp fullcharge BAT0

Initiates a full-capacity charge of battery `BAT0`.

Charge battery to the stop charge threshold once
""""""""""""""""""""""""""""""""""""""""""""""""
::

     sudo tlp chargeonce [<battery>]

Lifts the start charge threshold temporarily.
The configured start charge threshold will be restored at the next boot or by using
:command:`tlp setcharge` without the threshold arguments.

Hint: after setting he thresholds the command terminates. It does not wait for
the charge to complete.

Example: ::

    sudo tlp chargeonce BAT0

Initiates a charge of battery `BAT0` up to the stop threshold.

Force a complete/partial discharge of the battery while on AC power
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
*Version 1.8 and newer*

::

    sudo tlp discharge [<battery>] [<level>]

*Version 1.7 and older*

::

    sudo tlp discharge [<battery>]

Discharge the selected battery to the desired percentage. If no percentage
is specified, the battery will be fully discharged. The command continuously
shows remaining the capacity and the estimated discharge time. You can stop
the process at any time by pressing :kbd:`Control-C`.

* The command requires the charger to be plugged in.
* If the target percentage is below the start threshold, the battery will begin
  recharging once the command ends.
* ThinkPads with two batteries: the battery controller can only handle one
  battery at a time. While discharging one battery with this command the other
  battery cannnot be charged or discharged.
* If you encounter any problems, see the :ref:`FAQ <faq-discharge-shutdown>`.

.. include:: ../include/discharge-shutdown.rst

Example: ::

    sudo tlp discharge BAT0 50

Discharges battery `BAT0` down to 50%.

Perform a battery recalibration while on AC power
"""""""""""""""""""""""""""""""""""""""""""""""""
::

    sudo tlp recalibrate [<battery>]

First, remove the charge thresholds. Then, fully discharge the selected
battery. The command continuously shows remaining the capacity and the
estimated discharge time. You can stop the process at any time by
pressing :kbd:`Control-C`.

* The command requires the charger to be plugged in.
* As soon as the battery is empty, the command ends and the recharge process
  begins. It does not wait for the recharge to complete.
* The configured thresholds will be restored at the next boot or by using
  :command:`tlp setcharge` without the threshold arguments.
* Important: To complete the recalibration process, fully charge the battery;
  you may power off, but do not remove the charger.
* ThinkPads with two batteries: the battery controller can only handle one
  battery at a time. While discharging one battery with this command the other
  battery cannnot be charged or discharged.
* If you encounter any problems, see the :ref:`FAQ <faq-discharge-shutdown>`.

.. include:: ../include/discharge-shutdown.rst

Example: ::

    sudo tlp recalibrate BAT0

Recalibrates battery `BAT0`.

Hints:

* Recalibration forces the battery pack to update the `energy_full` or
  `charge_full` information shown by :command:`tlp-stat -b`
* Recalibration does not repair defective or worn out batteries

Disk IDs
^^^^^^^^
::

    tlp diskid

Shows disk ids for configured drives.

Version
^^^^^^^
*Version 1.7 and newer*
::

    tlp --version

Shows the installed version.
