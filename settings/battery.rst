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

Set battery charge thresholds for main and secondary battery:

.. include:: ../include/charge-threshold-effect.rst

Keep in mind that the names of the batteries shown by :command:`tlp-stat -b`
don't have to match the `_BAT0` or `_BAT1` parameter qualifiers.
Please refer to :doc:`bc-vendors` to see which qualifier applies to which battery.

Thresholds always go with a lower usable battery capacity, therefore the settings
are disabled by default and must be enabled explicitly by removing the leading `#`.

.. important::

    You must always specify both charge thresholds - *start and stop* - for a battery,
    otherwise TLP will reject both thresholds. If you want to apply only one threshold
    (or your hardware does support only one), then use the dummy value
    `0` to skip the other one.

.. seealso::

    * :ref:`faq-how-to-set-thresholds`
    * :ref:`faq-how-to-disable-thresholds`
    * :ref:`cmd-tlp-fullcharge`


RESTORE_THRESHOLDS_ON_BAT
-------------------------
::

    RESTORE_THRESHOLDS_ON_BAT=1

Restore configured charge thresholds when AC is unplugged:

* 0 – disable
* 1 – enable

Default when unconfigured:

    | 1 – *Version 1.10 and newer*
    | 0 – *Version 1.9.1 and older*

Hint: after the commands :command:`tlp fullcharge/recalibrate` the charge thresholds
will stay at the vendor specific defaults until the next reboot. Use this
feature to restore them prematurely.


.. seealso::

    * Settings: :doc:`/settings/introduction`
    * Settings: :doc:`/settings/bc-vendors`
    * FAQ: :doc:`/faq/battery`
