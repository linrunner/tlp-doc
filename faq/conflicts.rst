Conflicts
=========

Does TLP conflict with other power management tools?
----------------------------------------------------
**Generally yes. It is not recommended to use two (or more) power
management tools at the same time.**

Basically, all power management tools control a similar
set of :ref:`kernel tunables <intro-how-it-works>`.
"Conflict" refers to the situation where tunables controlled by TLP are
overwritten by the other tool (and vice versa), resulting in
unpredictable power savings.
Additionally, identifying the tool causing the issue may be challenging
if power saving leads to problems during operation.

The remainder of this section discusses dedicated power management tools,
while the following section deals with desktop environments.

auto-cpufreq
^^^^^^^^^^^^
auto-cpufreq controls a subset of TLP's kernel tunables, **do not use
together with TLP**.

Note: auto-cpufreq is not a pure power saving tool. On the contrary, when the
CPU is under heavy load on battery power, performance is boosted at the cost of
higher power consumption.

Intel Low Power Mode Daemon (intel-lpmd)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
`intel-lpmd <https://github.com/intel/intel-lpmd>`_
is designed to optimize active idle power by activating the power-efficient cores and
disabling the rest. Conflicts with TLP are not known.

power-profiles-daemon
^^^^^^^^^^^^^^^^^^^^^
Please refer to the dedicated page: :doc:`/faq/ppd`.

Powertop
^^^^^^^^
Please refer to the dedicated page: :doc:`powertop`.

Slimbook Battery
^^^^^^^^^^^^^^^^
Slimbook Battery uses TLP as a backend to control
:ref:`kernel tunables <intro-how-it-works>`.
For this purpose, it continuously overwrites your TLP configuration.
If you wish to configure TLP individually, you must **first uninstall
Slimbook Battery**.

system76-power (Pop!_OS)
^^^^^^^^^^^^^^^^^^^^^^^^
system76-power is part of the standard installation of SYSTEM76's Pop!_OS.
It works on the same set of :ref:`kernel tunables <intro-how-it-works>`
as TLP. **Do not use together with TLP**.

Thermal Daemon (thermald)
^^^^^^^^^^^^^^^^^^^^^^^^^
`thermald <https://github.com/intel/thermal_daemon>`_ limits power dissipation
to prevent the laptop from overheating. It does not provide power saving
functionality for other situations and therefore does not conflict with TLP.

throttled
^^^^^^^^^
Only throttled's dynamic `HWP_Mode` setting interferes with TLP's actions.
If you want to use it, disable the feature in TLP by configuring
`CPU_ENERGY_PERF_POLICY_ON_AC=""`.


Does TLP conflict with my desktop environment's power savings?
--------------------------------------------------
It depends on which desktop. The following applies to all:

* TLP does not touch display or keyboard backlight brightness ⇒ no conflict.
* TLP does not change suspend / hibernate settings ⇒ no conflict.

The following applies specifically:

GNOME Desktop
^^^^^^^^^^^^^
By default, many distributions install power-profiles-daemon
as an optional component of the GNOME desktop environment.
power-profiles-daemon competes and conflicts with TLP,
please refer to the dedicated page: :doc:`/faq/ppd`.

In addition, the GNOME desktop can apply battery charge thresholds if configured.
To disable the setting, go to `Settings → Battery Charging` and check
`Maximize Charge`, then use the :command:`tlp start` command
to re-apply the thresholds already configured in TLP.

KDE Plasma Desktop
^^^^^^^^^^^^^^^^^^
By default, many distributions install power-profiles-daemon
as an optional component of the KDE desktop environment.
power-profiles-daemon competes and conflicts with TLP,
please refer to the dedicated page: :doc:`/faq/ppd`.

In addition, the KDE desktop can apply charge thresholds if configured.
To disable the setting, go to
`System Settings → Power Management → Advanced Power Management`
and make sure that no `Charge Limits` are entered there,
then use the :command:`tlp start` command
to re-apply the thresholds already configured in TLP.

Cinnamon Desktop (Linux Mint)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
By default, Linux Mint 22 (and later) installs power-profiles-daemon
as an optional component of the Cinnamon desktop environment.
power-profiles-daemon competes and conflicts with TLP,
please refer to the dedicated page: :doc:`/faq/ppd`.
