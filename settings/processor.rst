Processor
=========

CPU_DRIVER_OPMODE_ON_AC/BAT
---------------------------
*Version 1.6 and newer* ::

    CPU_DRIVER_OPMODE_ON_AC=active
    CPU_DRIVER_OPMODE_ON_BAT=active

*Version 1.9 and newer* ::

    CPU_DRIVER_OPMODE_ON_SAV=active

Selects a CPU scaling driver operating mode. Configuration depends on the
active driver:

.. rubric:: amd-pstate

AMD Zen 2 or newer CPUs provide:

* active - requires kernel 6.3 or newer, changes driver name to `amd-pstate-epp`
* passive
* guided - requires kernel 6.4 or newer

Since AMD's kernel documentation is hard to penetrate, here is an attempt
to outline the individual modes (without guarantee):

* In **active** (autonomous) mode, the processor selects the operating
  frequencies within the hardware's min/max limits, based on the energy
  performance preference (see below).
* In **guided** (guided autonomous) mode, the processor chooses the operating
  frequencies within the hardware's limits, based on the current workload.
* In **passive** (non-autonomous) mode, the governor dictates the operating
  frequencies within the hardware's limits.

All modes support turbo boost and can be configured with min and max frequency
limits (see below).

.. note::

    * From kernel 6.13 onwards, `amd-pstate` sets the initial `scaling_min_freq` to
      `amd_pstate_lowest_nonlinear_freq` instead of `cpuinfo_min_freq`
      (see the :command:`tlp-stat -p -v` output) to achieve the `"most energy-efficient performance"
      <https://web.git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/drivers/cpufreq/amd-pstate.c?id=5d9a354cf839a652f8f6ca2b920696c6a9041801>`_.
      However, you may still configure a lower min frequency (see below).
    * For kernels older than 6.5 `amd-pstate` must be activated via a kernel
      boot option; see the driver documentation linked at the bottom.
    * It should be kept in mind that not all laptop BIOSes allow the activation
      of `amd-pstate` despite a suitable CPU. This situation is identifiable by
      the kernel message "_CPC object is not present in SBIOS or ACPI disabled".
      Please do *not* open TLP issues for this.
    * `passive` and `guided` mode do *not* support EPP (:ref:`set-cpu-energy-perf-policy`).
    * Some users have observed limited frequency or P-state ranges after
      switching modes. These are hardware issues or kernel issues, so please
      do *not* open a TLP issue for them.

.. rubric:: intel_pstate

Intel Core i 2nd gen. ("Sandy Bridge") or newer Intel CPUs provide these modes:

* active
* passive

Starting with kernel 5.7, the `intel_pstate` driver selects passive mode
aka `intel_cpufreq` for CPUs that do not support hardware-managed P-states
(HWP), i.e. Intel Core i 5th gen. or older.

.. rubric:: acpi-cpufreq

This driver for older AMD/Intel CPUs and other vendors does not support changing
the operating mode.


.. _set-cpu-scaling-governor:

CPU_SCALING_GOVERNOR_ON_AC/BAT
------------------------------
::

    CPU_SCALING_GOVERNOR_ON_AC=powersave
    CPU_SCALING_GOVERNOR_ON_BAT=powersave

*Version 1.9 and newer* ::

    CPU_SCALING_GOVERNOR_ON_SAV=powersave

Selects the CPU scaling governor for automatic frequency scaling. Configuration
depends on the active driver:

.. rubric:: amd-pstate | intel_pstate in active mode

* performance
* powersave – default

.. rubric:: amd-state in passive or guided mode
            | intel_pstate in passive mode
            | acpi-cpufreq

* conservative
* ondemand – default for most distributions
* userspace
* powersave
* performance
* schedutil – default for newer kernels and the `amd-pstate`/`intel_pstate`
  drivers in passive mode

.. note::

    * Refer to the output of :command:`tlp-stat -p` to determine the active
      scaling driver and available governors.
    * Keep in mind that modularized kernels may not reveal all available
      governors in the :command:`tlp-stat -p` output after boot.
      In this case, configure the desired governor anyway and, after
      :command:`tlp start`, check the output of :command:`tlp-stat -p` again
      (requires version 1.6).

.. important::

    Default governors listed above are power efficient for almost all workloads,
    therefore kernel devs and most distributions have chosen them as such.
    So *before* changing the scaling governor please do your research about the
    advantages and disadvantages of the others (refer to the links at the bottom
    of this page).

CPU_SCALING_MIN/MAX_FREQ_ON_AC/BAT
----------------------------------
::

    CPU_SCALING_MIN_FREQ_ON_AC=0
    CPU_SCALING_MAX_FREQ_ON_AC=0
    CPU_SCALING_MIN_FREQ_ON_BAT=0
    CPU_SCALING_MAX_FREQ_ON_BAT=0

*Version 1.9 and newer* ::

    CPU_SCALING_MIN_FREQ_ON_SAV=0
    CPU_SCALING_MAX_FREQ_ON_SAV=0

Set the min/max frequency available for the scaling governor. Possible values
depend on your CPU. For available frequencies consult the output of
:command:`tlp-stat -p`.

Hints:

* Do not use this setting with the `intel_pstate` driver in active mode,
  use :ref:`set-cpu-min-max-perf` instead
* Min/max frequencies must always be specified for both modes i.e. `AC` and `BAT`
* To enable processor defaults comment all four settings and reboot
* Lowering the max frequency on battery power may not conserve power;
  best results are to be expected from the above mentioned default governors
  without frequency limits


.. _set-cpu-energy-perf-policy:

CPU_ENERGY_PERF_POLICY_ON_AC/BAT
--------------------------------
::

    CPU_ENERGY_PERF_POLICY_ON_AC=balance_performance
    CPU_ENERGY_PERF_POLICY_ON_BAT=balance_power

*Version 1.9 and newer* ::

    CPU_ENERGY_PERF_POLICY_ON_SAV=power

Set CPU energy/performance policies (in order of
increasing power saving):

* performance
* balance_performance – kernel default
* default – Note: the kernel translates default into balance_performance,
  which is then returned when viewed with :command:`tlp-stat -p`
* balance_power
* power

Default when unconfigured: balance_performance (AC), balance_power (BAT), power(SAV)

Requirements:

.. rubric:: AMD

*Version 1.6 and newer*

* AMD Zen 2 or newer CPU with kernel 6.3 and `amd-pstate` driver in active mode

.. rubric:: Intel

* **HWP.EPP** (hardware-managed P-states): Intel Core i 6th gen. ("Skylake")
  or newer CPU with kernel 4.10 and `intel_pstate` driver in active mode
* **EPB**: Intel Core i 2nd gen. (“Sandy Bridge”) or newer CPU with kernel 5.2
  and `intel_pstate` driver
* Note that HWP.EPP and EPB are mutually exclusive. When EPP is available,
  Intel CPUs will not honor EPB. Only the matching feature will be applied
  by TLP and shown by :command:`tlp-stat -p`.


.. _set-cpu-min-max-perf:

CPU_MIN/MAX_PERF_ON_AC/BAT
--------------------------
::

    CPU_MIN_PERF_ON_AC=0
    CPU_MAX_PERF_ON_AC=100
    CPU_MIN_PERF_ON_BAT=0
    CPU_MAX_PERF_ON_BAT=80

*Version 1.9 and newer* ::

    CPU_MIN_PERF_ON_SAV=0
    CPU_MAX_PERF_ON_SAv=60

Define the min/max P-state for Intel CPUs. Values are stated as a
percentage (0..100%) of the total available processor performance.

Hints:

* Requires Intel Core i 2nd gen. ("Sandy Bridge") or newer CPU with
  `intel_pstate` or `intel_cpufreq` scaling driver
* The driver imposes a limit > 0 on the min P-state, see `min_perf_pct` in the
  output of :command:`tlp-stat -p`
* This setting is intended to limit the power dissipation of the CPU


.. _set-cpu-boost:

CPU_BOOST_ON_AC/BAT
-------------------
::

    CPU_BOOST_ON_AC=1
    CPU_BOOST_ON_BAT=1

*Version 1.9 and newer* ::

    CPU_BOOST_ON_SAV=0

Configure CPU "turbo boost" (Intel) or "core performance boost" aka "turbo core" (AMD):

* 0 - disable
* 1 - allow

.. note::

    * A value of 1 does not activate boosting, it just allows it
    * For use with the `amd-pstate` driver, at least kernel 6.11 is required


CPU_HWP_DYN_BOOST_ON_AC/BAT
---------------------------
::

    CPU_HWP_DYN_BOOST_ON_AC=1
    CPU_HWP_DYN_BOOST_ON_BAT=1

*Version 1.9 and newer* ::

    CPU_HWP_DYN_BOOST_ON_SAV=0

Configure the Intel CPU HWP dynamic boost feature:

* 0 - disable
* 1 - enable

Requires: Intel Core i 6th gen. ("Skylake") or newer CPU with `intel_pstate`
scaling driver in `active` mode


.. seealso::

    * Settings: :doc:`/settings/introduction`
    * FAQ: :doc:`../faq/processor`
    * `CPU Performance Scaling <https://docs.kernel.org/admin-guide/pm/cpufreq.html>`_
      – kernel documentation covering scaling governors et al.
    * `amd-pstate CPU Performance Scaling Driver <https://docs.kernel.org/admin-guide/pm/amd-pstate.html>`_
      – driver documentation
    * `intel_pstate CPU Performance Scaling Driver <https://docs.kernel.org/admin-guide/pm/intel_pstate.html>`_
      – driver documentation
    * `Intel Hardware P-State (HWP) / Intel Speed Shift <https://smackerelofopinion.blogspot.com/2021/07/intel-hardware-p-state-hwp-intel-speed.html>`_
      – a consideration of `HWP.EPP`
    * `Intel Performance and Energy Bias Hint <https://docs.kernel.org/admin-guide/pm/intel_epb.html>`_
      – `EPB` documentation
    * `Improvements in CPU frequency management <https://lwn.net/Articles/682391/>`_
      – LWN article covering the schedutil governor
    * `Why EPB is not set when HWP.EPP is available <https://bbs.archlinux.org/viewtopic.php?pid=2076591#p2076591>`_
      – Arch Linux Forums
