Operation
=========

TLP_ENABLE
----------
::

   TLP_ENABLE=1

Set to 0 to disable TLP (reboot needed).

Default when unconfigured: 1

TLP_DISABLE_DEFAULTS
--------------------
*Version 1.9 and newer*
::

    TLP_DISABLE_DEFAULTS=0

Set to 1 to deactivate all intrinsic defaults of TLP. This means that
TLP only applies settings that have been explicitly activated i.e.
parameters without a leading '#'.

Notes:

* Helpful if one wants to use only selected features of TLP
* After activation, use :command:`tlp-stat -c` to display your effective configuration

TLP_WARN_LEVEL
--------------
::

   TLP_WARN_LEVEL=3

Controls how warnings about invalid settings are issued:

* 0 - disabled
* 1 - background tasks (boot, resume, change of power source) report to the syslog/journal
* 2 - shell commands report to the terminal (stderr)
* 3 - combination of 1 and 2

Default when unconfigured: 3

TLP_MSG_COLORS
--------------
*Version 1.7 and newer*
::

    TLP_MSG_COLORS="91 93 1 92"

Colorize error, warning, notice and success messages. Colors are specified
with ANSI codes:

    1=bold black, 90=grey, 91=red, 92=green, 93=yellow, 94=blue, 95=magenta, 96=cyan, 97=white.

Notes:

    * Other colors are possible, refer to: `ANSI escape code <https://en.wikipedia.org/wiki/ANSI_escape_code#3-bit_and_4-bit>`_
    * Colors must be specified in the order "<error> <warning> <notice> <success>"
    * By default, errors are shown in red, warnings in yellow, notices in bold and success in green
    * Disable the feature completely by using :code:`TLP_MSG_COLORS=""`

Default when unconfigured: "91 93 1 92"


.. _set-auto-switch:

TLP_AUTO_SWITCH
---------------
*Version 1.9 and newer*
::

    TLP_AUTO_SWITCH=2

Control automatic switching of the power profile when connecting or removing
the charger [#]_, when booting the system or when executing :command:`tlp start`:

* 0 - disabled: never switch, use `TLP_DEFAULT_MODE` if configured
* | 1 - auto: always switch, select `performance` on AC and `balanced` on battery power [#]_
* 2 - smart: do not switch if the following profiles were active previously:

  * `power-saver` or `balanced` on AC
  * `power-saver` or `performance` on battery power

.. [#] The same logic applies if the charger was connected/removed during suspend
.. [#] Behavior identical to *version 1.8 or older*

Default when unconfigured: 2


.. _set-default-mode:

TLP_DEFAULT_MODE
----------------
::

   TLP_DEFAULT_MODE=PRF

*Version 1.8 and older*

Defines TLP's default operation mode (`AC` or `BAT`) when a power source cannot
be detected.

*Version 1.9 and newer*

Defines power profile to use when automatic switching is disabled (`TLP_AUTO_SWITCH=0`),
profile is locked (TLP_PERSISTENT_DEFAULT=1) or no power source is detected:

* `PRF` - performance
* `BAL` - balanced
* `SAV` - power-saver

Note: Legacy values `AC` and `BAT` continue to work. They are mapped to `PRF` and `BAL`,
respectively.

.. note::
    To disable automatic switching and default to the `balanced` profile, configure: ::

        TLP_AUTO_SWITCH=0
        TLP_DEFAULT_MODE=BAL


.. _set-persistent-default:

TLP_PERSISTENT_DEFAULT
----------------------
::

   TLP_PERSISTENT_DEFAULT=0

Lock power profile:

* 0 - disabled: profile depends on automatic switching (see `TLP_AUTO_SWITCH` above)
* 1 - enabled: profile is locked to `TLP_DEFAULT_MODE` (`TLP_AUTO_SWITCH` is ignored)

Default when unconfigured: 0

.. note::
    To always use BAT settings / balanced profile, configure: ::

        TLP_DEFAULT_MODE=BAT
        TLP_PERSISTENT_DEFAULT=1


TLP_PS_IGNORE
-------------
::

   TLP_PS_IGNORE="BAT"

Power supply class(es) to ignore when determining operation mode:

* AC
* BAT
* USB

Enter multiple classes separated with spaces.

.. note::

    Use as a workaround for laptops where operation mode AC or BAT is
    incorrectly detected.

.. seealso::

    * Settings: :doc:`/settings/introduction`
    * FAQ: :doc:`/faq/operation`
