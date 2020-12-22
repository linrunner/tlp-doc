Processor
=========

.. seealso::

    :ref:`faq-powercon-cpu-dp`

Sluggish performance with kernel 5.7 (and higher)
-------------------------------------------------
Symptoms: you have configured the `powersave` scaling governor for `intel_pstate`: ::

    CPU_SCALING_GOVERNOR_ON_AC=powersave
    CPU_SCALING_GOVERNOR_ON_BAT=powersave

and :command:`tlp-stat -p` displays ::

    /sys/devices/system/cpu/cpu0/cpufreq/scaling_driver    = intel_cpufreq
    /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor  = powersave
    /sys/devices/system/cpu/cpu0/cpufreq/scaling_available_governors = conservative ondemand userspace powersave performance schedutil

Cause: starting with kernel 5.7, the `intel_pstate` scaling driver selects
"passive mode" aka `intel_cpufreq` for CPUs that do not support hardware-managed
P-states (HWP), i.e. Intel Core i 6th Gen. ("Skylake") or newer. The governor
`powersave` provided by `intel_cpufreq` works differently than the one provided
by `intel_pstate` and produces the sluggish behaviour.

Solution: use the default governor – should be `schedutil` - by commenting
above configuration lines or configure it explictly: ::

    CPU_SCALING_GOVERNOR_ON_AC=schedutil
    CPU_SCALING_GOVERNOR_ON_BAT=schedutil

For details refer to :ref:`set-cpu-scaling-governor`.

Frequency scaling settings do not get applied
---------------------------------------------
Symptom: :command:`tlp-stat -p` shows values that do not reflect configuration.

There are several possible causes:

.. rubric:: Invalid frequency settings

Solution: :command:`tlp-stat -p` tells the possible frequencies for your CPU.
Example: ::

    /sys/devices/system/cpu/cpu0/cpufreq/scaling_available_frequencies = 2400000 1600000 800000 [kHz]


.. rubric:: No `ondemand` governor with `intel_pstate`

Symptom: :command:`tlp-stat -p` displays ::

    /sys/devices/system/cpu/cpu0/cpufreq/scaling_driver = intel_pstate
    /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor = powersave

Works as designed: since kernel 3.9 the new scaling driver `intel_pstate` is
available and enabled by default on Intel Sandy Bridge (or newer) hardware.
`intel_pstate` supports the governors `powersave` (recommended default) and
`performance` only, `ondemand` is not available.

`tlp-stat -p` shows 'x86_energy_perf_policy: program for your kernel not installed.'
------------------------------------------------------------------------------------
Depending on the distribution your mileage may vary:

* **Ubuntu**: install the metapackage `linux-tools-generic` or `linux-tools-generic-lts-*`
  for HWE stack kernels, no package is available for mainline kernels.
* **Debian**: install the package `linux-cpupower`.
* **Arch**: install the package `x86_energy_perf_policy`.
* **Fedora**: install the package `kernel-tools`.
