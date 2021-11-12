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
*Version 1.4 and higher*
::

   TLP_WARN_LEVEL=3

Controls how warnings about invalid settings are issued:

* 0 - disabled
* 1 - background tasks (boot, resume, change of power source) report to the syslog/journal
* 2 - shell commands report to the terminal (stderr)
* 3 - combination of 1 and 2

Default when unconfigured: 3

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
* USB - *Version 1.4 and higher only*

*Version 1.4 and higher only: enter multiple classes separated with spaces.*

.. note::

    Use as a workaround for laptops where operation mode AC or BAT is
    incorrectly detected.
