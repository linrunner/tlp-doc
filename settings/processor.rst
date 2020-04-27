Processor
=========

CPU_SCALING_GOVERNOR_ON_AC/BAT
------------------------------
::

    CPU_SCALING_GOVERNOR_ON_AC=powersave
    CPU_SCALING_GOVERNOR_ON_BAT=powersave

Selects the CPU scaling governor for automatic frequency scaling. Configuration
depends on the active scaling driver:

.. rubric:: intel_pstate

For Intel Core i 2nd gen. ("Sandy Bridge") or newer Intel CPUs. Supported
governors are:

* powersave – recommended (kernel default)
* performance

.. rubric:: acpi-cpufreq

For all AMD and older Intel CPUs. Supported governors are:

* ondemand – recommended (default for most distributions)
* schedutil
* powersave
* performance
* conservative

Hint: refer to the output of :command:`tlp-stat -p` to determine the active
scaling driver and available governors.

.. important::

    `powersave` for `intel_pstate` and `ondemand` for `acpi-cpufreq` are power
    efficient for almost all workloads and therefore kernel and most distributions
    have chosen them as defaults; if you still want to change the scaling governor,
    you should know what you're doing!

CPU_SCALING_MIN/MAX_FREQ_ON_AC/BAT
----------------------------------
::

    CPU_SCALING_MIN_FREQ_ON_AC=0
    CPU_SCALING_MAX_FREQ_ON_AC=9999999
    CPU_SCALING_MIN_FREQ_ON_BAT=0
    CPU_SCALING_MAX_FREQ_ON_BAT=9999999

Set the min/max frequency available for the scaling governor. Possible values
depend on your CPU. For available frequencies consult the output of
:command:`tlp-stat -p`.

Hints:

* Do not use this setting with the `intel_pstate` scaling driver, use
  :ref:`set-cpu-mix-max-perf` instead
* Min/max frequencies have to be specified for battery and AC modes
* To enable processor defaults comment all four settings and reboot
* Lowering the max frequency on battery power does not conserve power;
  best results are achieved by the ondemand governor without frequency limits

.. _set-cpu-energy-perf-policy:

CPU_ENERGY_PERF_POLICY_ON_AC/BAT
--------------------------------
*Version 1.3 and higher*

::

    CPU_ENERGY_PERF_POLICY_ON_AC=balance_performance
    CPU_ENERGY_PERF_POLICY_ON_BAT=balance_power

Set Intel CPU energy/performance policies `HWP.EPP` and `EPB` (in order of
increasing power saving):

* performance
* balance_performance
* default
* balance_power
* power

Default when unconfigured: balance_performance (AC), balance_power (BAT)

Hints:

* Requires Intel Core i CPU and the `intel_pstate` scaling driver
* `HWP.EPP` requires kernel 4.10 and Intel Core i Gen. 5 (Skylake) or newer CPU
* `EPB` requires kernel 5.2 or module msr and x86_energy_perf_policy from linux-tools
* When `HWP.EPP` is available, `EPB` is not set

CPU_HWP_ON_AC/BAT
-----------------
*Version 1.2.2 and lower*

::

    CPU_HWP_ON_AC=balance_performance
    CPU_HWP_ON_BAT=balance_power

Set Intel CPU energy/performance policy HWP.EPP. Possible values are (in order
of increasing power saving):

* performance
* balance_performance
* default
* balance_power
* power

Hints:

* Requires Intel Core i Gen. 5 (Skylake) or newer CPU and the `intel_pstate` scaling driver
* Requires kernel 4.10
* For version 1.3 and higher this parameter is replaced by :ref:`set-cpu-energy-perf-policy`

.. _set-cpu-mix-max-perf:

CPU_MIN/MAX_PERF_ON_AC/BAT
--------------------------
::

    CPU_MIN_PERF_ON_AC=0
    CPU_MAX_PERF_ON_AC=100
    CPU_MIN_PERF_ON_BAT=0
    CPU_MAX_PERF_ON_BAT=30

Define the min/max P-state for Intel Core i processors. Values are stated as a
percentage (0..100%) of the total available processor performance.

Hints:

* Requires the `intel_pstate` scaling driver
* The driver imposes a limit > 0 on the min P-state, see `min_perf_pct` in the
  output of :command:`tlp-stat -p`
* This setting is intended to limit the power dissipation of the CPU

CPU_BOOST_ON_AC/BAT
-------------------
::

    CPU_BOOST_ON_AC=1
    CPU_BOOST_ON_BAT=0

Disable CPU "turbo boost" (Intel) or "turbo core" (AMD) feature (0 = disable /
1 = allow).

Hints:

* A value of 1 does not activate boosting, it just allows it
* This may conflict with your distribution's governor settings

SCHED_POWERSAVE_ON_AC/BAT
-------------------------
::

    SCHED_POWERSAVE_ON_AC=0
    SCHED_POWERSAVE_ON_BAT=1

Minimize number of used CPU cores/hyper-threads under light load conditions
(1 = enabled, 0 = disabled). Depends on kernel and processor model.

Default when unconfigured: 0 (AC), 1 (BAT)

ENERGY_PERF_POLICY_ON_AC/BAT
-----------------------------
*Version 1.2.2 and lower*

::

    ENERGY_PERF_POLICY_ON_AC=performance
    ENERGY_PERF_POLICY_ON_BAT=power

Set Intel CPU energy/performance policy `EPB`. Possible values are (in order of
increasing power saving):

* performance
* balance-performance
* default (deprecated: normal)
* balance-power
* power (deprecated: powersave)

Hints:

* Requires the `intel_pstate` scaling driver
* Requires the kernel module `msr` and the tool `x86_energy_perf_policy` matching
  your kernel version
* For version 1.3 and higher this parameter is replaced by :ref:`set-cpu-energy-perf-policy`

.. seealso::

    * `intel_pstate CPU Performance Scaling Driver <https://www.kernel.org/doc/html/latest/admin-guide/pm/intel_pstate.html>`_
      – driver documentation
    * `Intel Performance and Energy Bias Hint <https://www.kernel.org/doc/html/latest/admin-guide/pm/intel_epb.html>`_
      – `EPB` documentation
    * `Improvements in CPU frequency management <https://lwn.net/Articles/682391/>`_
      – LWN article covering the schedutil governor
