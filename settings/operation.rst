Operation
=========

TLP_ENABLE
----------
::

   TLP_ENABLE=1

Set to 0 to disable TLP (reboot needed).

Default when unconfigured: 1

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
*Version 1.7*
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


TLP_DEFAULT_MODE
----------------
::

   TLP_DEFAULT_MODE=AC

Defines TLP's default operation mode (`AC` or `BAT`) in case a power source cannot
be detected. Concerns some desktop and embedded hardware only.

.. _set-persistent-default:

TLP_PERSISTENT_DEFAULT
----------------------
::

   TLP_PERSISTENT_DEFAULT=0

Selects how the operation mode is determined:

* 0 – apply settings profile acccording to actual power source (default)
* 1 – always use settings for `TLP_DEFAULT_MODE`

Default when unconfigured: 0

.. note::
    To force BAT settings while AC powered, use: ::

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
