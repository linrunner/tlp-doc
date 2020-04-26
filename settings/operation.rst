.. _set-operation:

Operation
=========

TLP_ENABLE
----------
::

   TLP_ENABLE=1

Set to 0 to disable TLP (reboot needed).

Default when unconfigured: 0

TLP_DEFAULT_MODE
----------------
::

   TLP_DEFAULT_MODE=AC

Defines TLP's default operation mode (`AC` or `BAT`) in case a power source cannot
be detected. Concerns some desktop and embedded hardware only.

TLP_PERSISTENT_DEFAULT
----------------------
::

   TLP_PERSISTENT_DEFAULT=0

Selects how the operation mode is determined:

* 0 – apply settings profile acccording to actual power source (default)
* 1 – always use settings for `TLP_DEFAULT_MODE`

Default when unconfigured: 0

Hint: `TLP_DEFAULT_MODE=BAT`, `TLP_PERSISTENT_DEFAULT=1` forces BAT settings
while AC powered.

TLP_PS_IGNORE
-------------
.. versionadded:: 1.3

::

   TLP_PS_IGNORE=BAT

Power supply class to ignore when determining operation mode:

* `AC`
* `BAT`

Hint: use as a workaround for laptops where operation mode AC or BAT is
incorrectly detected.
