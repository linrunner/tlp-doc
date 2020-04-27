Powertop
========

Does Powertop achieve better power saving than TLP?
---------------------------------------------------
Basically no. TLP's default configuration is based on the same policy as Powertop's
recommendations. Refer to the following sections for details and exceptions.

Does Powertop conflict with TLP?
--------------------------------
Powertop in interactive mode has no impact on TLP's function.

.. rubric:: Explanation

Powertop isn't a power management but merely an analysis tool.
You can use Powertop to view estimates about your power usage before and after
installation of TLP, but TLP determines the best defaults for your system
regardless of whether or not Powertop is installed.

.. note::

    On the other hand, attempting to run :command:`powertop --auto-tune` on each boot
    may overwrite TLP's settings depending on the actual boot order.

Why does Powertop suggest more power saving settings with TLP already running?
------------------------------------------------------------------------------
.. important::

    TLP applies maximum power savings on battery power only, so unplug AC power
    before checking with Powertop.

Powertop's recommendations are quite schematic, they do not take into account
frequently occurring problems with certain types of devices. For that reason TLP's
settings deviate from Powertop's recommendations for a few but important exceptions:

.. rubric:: Message 'VM writeback timeout'

Powertop insists on a value of 1500 centisecs, whereas TLP's defaults are 1500
on AC and 6000 on battery power. If you incline towards Powertop's opinion then
change the setting to: ::

    MAX_LOST_WORK_SECS_ON_BAT=15

.. rubric:: Message 'SATA ALPM link power'

Some laptops refuse ALPM for particular SATA links, so it is impossible to
change from `max_performance` to `min_power`. One cause may be an open link to
the docking station's drive bay when not docked.

Workaround for older X-Series ThinkPads: enter BIOS setup. Go to
`Security → IO Port Access` and change Ultrabay access to `disabled`.
Saves approx. 0.4 W.

.. rubric:: Message 'Wifi powersave'

For some Wi-Fi cards – for instance the Intel 3945abg – the Linux kernel does
not support Wi-Fi power saving.

.. rubric:: Message 'USB autosuspend'

TLP's default configuration intentionally exempts input devices like mice and
keyboards as well as scanners handled by `libsane`. To enforce autosuspend
nevertheless, add the device to :ref:`set-usb-whitelist`.

Powertop shows a very high power consumption for a device
---------------------------------------------------------
It is impossible to measure the power consumption of individual laptop components.
Therefore the readings in the 'Device stats' column 'Power est.' are rough estimations
and do not provide a reliable statement.
