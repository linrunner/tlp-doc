.. _set-graphics:

Graphics
========

INTEL_GPU_*
-----------
.. rubric:: Intel GPU

::

    INTEL_GPU_MIN_FREQ_ON_AC=0
    INTEL_GPU_MIN_FREQ_ON_BAT=0
    INTEL_GPU_MAX_FREQ_ON_AC=0
    INTEL_GPU_MAX_FREQ_ON_BAT=0
    INTEL_GPU_BOOST_FREQ_ON_AC=0
    INTEL_GPU_BOOST_FREQ_ON_BAT=0

Set the min/max/turbo frequency for the Intel GPU. Possible values depend on
your hardware. See the output of :command:`tlp-stat -g` for available
frequencies.


RADEON_POWER_PROFILE_ON_AC/BAT
------------------------------
.. rubric::  AMD Radeon GPU (old)

::

    RADEON_POWER_PROFILE_ON_AC=default
    RADEON_POWER_PROFILE_ON_BAT=default

Controls the clock for the AMD GPU. Supported by the radeon driver only,
not fglrx. Possible values:

* low
* mid
* high
* auto – high on AC, mid on battery
* default – use hardware defaults

Default when unconfigured: default

RADEON_DPM_STATE_ON_AC/BAT
--------------------------
.. rubric::  AMD Radeon GPU (new)

Since kernel 3.11 the new radeon dynamic power management (DPM) is available.
Supported by the radeon driver only, not fglrx.

::

    RADEON_DPM_STATE_ON_AC=performance
    RADEON_DPM_STATE_ON_BAT=battery

Controls the power management method of the AMD GPU. Possible values:

* battery – default on battery power
* performance – default on AC power

RADEON_DPM_PERF_LEVEL_ON_AC/BAT
-------------------------------
.. rubric::  AMD Radeon GPU (new)

::

    RADEON_DPM_PERF_LEVEL_ON_AC=auto
    RADEON_DPM_PERF_LEVEL_ON_BAT=auto

Controls the performance level. Possible values:

* auto – recommended
* low
* high

Default when unconfigured: auto

.. note::

    This setting makes the display flicker briefly when the power source changes.


.. seealso::

    * `Radeon driver documentation <https://wiki.x.org/wiki/RadeonFeature>`_ – see "KMS Power Management Options"

