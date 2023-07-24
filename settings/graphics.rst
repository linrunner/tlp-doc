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


RADEON_DPM_PERF_LEVEL_ON_AC/BAT
-------------------------------
.. rubric::  AMD GPU

::

    RADEON_DPM_PERF_LEVEL_ON_AC=auto
    RADEON_DPM_PERF_LEVEL_ON_BAT=auto

Controls the Dynamic power management (DPM) performance level. Supported by
`amdgpu` *(TLP version 1.4 and newer only)* and `radeon` drivers. Possible values:

* auto – recommended
* low
* high

Default when unconfigured: auto


RADEON_DPM_STATE_ON_AC/BAT
--------------------------
.. rubric::  AMD GPU

::

    RADEON_DPM_STATE_ON_AC=performance
    RADEON_DPM_STATE_ON_BAT=battery

Controls the power management method of the GPU. Possible values:

* battery
* balanced
* performance


RADEON_POWER_PROFILE_ON_AC/BAT
------------------------------
.. rubric::  AMD GPU (ATI legacy hardware)

::

    RADEON_POWER_PROFILE_ON_AC=default
    RADEON_POWER_PROFILE_ON_BAT=default

Controls the clock of the GPU. Supported by the `radeon` driver on legacy
ATI hardware only (where DPM is not available). Possible values:

* low
* mid
* high
* auto – high on AC, mid on battery
* default – use hardware defaults

Default when unconfigured: default

.. note::

    This setting may cause the display to flicker briefly when changing the
    power source.


.. seealso::

    * `Radeon driver documentation <https://wiki.x.org/wiki/RadeonFeature>`_ – see "KMS Power Management Options"
