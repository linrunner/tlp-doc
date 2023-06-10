Processor
=========

.. _set-cpu-scaling-governor:

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

* performance
* powersave – default

.. rubric:: intel_cpufreq

Starting with kernel 5.7, the `intel_pstate` scaling driver selects "passive mode"
aka `intel_cpufreq` for CPUs that do not support hardware-managed P-states (HWP),
i.e. Intel Core i 5th gen. or older.

Supported governors are identical to the `acpi-cpufreq` driver below, the default
scheduler is `schedutil`.

.. rubric:: acpi-cpufreq

For AMD, older Intel CPUs and other vendors. Supported governors are:

* conservative
* ondemand – default for most distributions
* userspace
* powersave
* performance
* schedutil – default for newer kernels and the `intel_cpufreq` driver above

.. note::

    Refer to the output of :command:`tlp-stat -p` to determine the active
    scaling driver and available governors.

.. important::

    Default governors listed above are power efficient for almost all workloads,
    therefore kernel devs and most distributions have chosen them as such.
    So *before* changing the scaling governor please do your research about the
    advantages and disadvantages of the others (refer to the links on the bottom
    of this page).

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
  :ref:`set-cpu-min-max-perf` instead
* Min/max frequencies must always be specified for both modes i.e. `AC` and `BAT`
* To enable processor defaults comment all four settings and reboot
* Lowering the max frequency on battery power may not conserve power;
  best results are to be expected from the ondemand governor without
  frequency limits


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

* `HWP.EPP` (hardware-managed P-states): requires kernel 4.10, `intel_pstate`
  scaling driver and Intel Core i  6th gen. ("Skylake") or newer CPU
* `EPB`: requires kernel 5.2 (or module msr and x86_energy_perf_policy from linux-tools),
  `intel_pstate` or `intel_cpufreq` scaling driver and Intel Core i 2nd gen.
  (“Sandy Bridge”) or newer CPU
* HWP.EPP and EPB are mutually exclusive: when EPP is available, Intel CPUs
  will not honor EPB. Only the matching feature will be applied by TLP
  and shown by by :command:`tlp-stat -p`.


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

* Requires Intel Core i 6th gen. ("Skylake") or newer CPU with `intel_pstate`
  scaling driver
* For version 1.3 and higher use :ref:`set-cpu-energy-perf-policy` instead


.. _set-cpu-min-max-perf:

CPU_MIN/MAX_PERF_ON_AC/BAT
--------------------------
::

    CPU_MIN_PERF_ON_AC=0
    CPU_MAX_PERF_ON_AC=100
    CPU_MIN_PERF_ON_BAT=0
    CPU_MAX_PERF_ON_BAT=30

Define the min/max P-state for Intel CPUs. Values are stated as a
percentage (0..100%) of the total available processor performance.

Hints:

* Requires Intel Core i 2nd gen. ("Sandy Bridge") or newer CPU with
  `intel_pstate` or `intel_cpufreq` scaling driver
* The driver imposes a limit > 0 on the min P-state, see `min_perf_pct` in the
  output of :command:`tlp-stat -p`
* This setting is intended to limit the power dissipation of the CPU


CPU_BOOST_ON_AC/BAT
-------------------
::

    CPU_BOOST_ON_AC=1
    CPU_BOOST_ON_BAT=0

Configure CPU "turbo boost" (Intel) or "turbo core" (AMD) feature (0 = disable /
1 = allow).

.. note::

    A value of 1 does not activate boosting, it just allows it.


CPU_HWP_DYN_BOOST_ON_AC/BAT
---------------------------
*Version 1.4 and higher*

::

    CPU_HWP_DYN_BOOST_ON_AC=1
    CPU_HWP_DYN_BOOST_ON_BAT=0


Configure the Intel CPU HWP dynamic boost feature:

* 0 - disable
* 1 - enable

Requires: Intel Core i 6th gen. ("Skylake") or newer CPU with `intel_pstate`
scaling driver in `active` mode

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

* Requires `intel_pstate` or `intel_cpufreq` scaling driver and Intel Core i 2nd gen.
  ("Sandy Bridge") or newer CPU
* Requires the kernel module `msr` and the tool `x86_energy_perf_policy` matching
  your kernel version
* For version 1.3 and higher use :ref:`set-cpu-energy-perf-policy` instead

.. seealso::

    * `CPU Performance Scaling <https://www.kernel.org/doc/html/latest/admin-guide/pm/cpufreq.html>`_
      – kernel documentation covering scaling governors et al.
    * `intel_pstate CPU Performance Scaling Driver <https://www.kernel.org/doc/html/latest/admin-guide/pm/intel_pstate.html>`_
      – driver documentation
    * `Intel Hardware P-State (HWP) / Intel Speed Shift <https://smackerelofopinion.blogspot.com/2021/07/intel-hardware-p-state-hwp-intel-speed.html>`_
      – a consideration of `HWP.EPP`
    * `Intel Performance and Energy Bias Hint <https://www.kernel.org/doc/html/latest/admin-guide/pm/intel_epb.html>`_
      – `EPB` documentation
    * `Improvements in CPU frequency management <https://lwn.net/Articles/682391/>`_
      – LWN article covering the schedutil governor
