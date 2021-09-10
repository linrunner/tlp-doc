Allowed threshold values are version and vendor specific:

* *Version 1.4 and higher:*

    * Consult the output of :command:`tlp-stat -b`
    * :command:`tlp setcharge` validates parameters and configuration and reports errors
    * A value of 0 is translated to the vendor specific default (or the `disabled` state)
    * If the laptop supports only a stop charge threshold,
      use `START_CHARGE_THRESH=0`
    * In case it supports both thresholds (currently Huawei and ThinkPads)
      and you want to use only one, then use `START_CHARGE_THRESH=0` or
      `STOP_CHARGE_THRESH=100` to disable the one you don't want to use
    * For some vendors the `BAT0` parameters apply to all batteries, regardless
      of their actual name

* *Version 1.3.1 and lower (only ThinkPads):*

    * `START_CHARGE_THRESH`: 1 to 96
    * `STOP_CHARGE_THRESH`: 5 to 100
    * `START_CHARGE_THRESH` must be <= `STOP_CHARGE_THRESH` - 4.
    * A value of 0 is translated to the vendor default 96/100%
