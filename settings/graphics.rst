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


.. _set-amdgpu-abm:

AMDGPU_ABM_LEVEL_ON_AC/BAT
--------------------------
*Version 1.7 (unreleased)*

.. rubric::  AMD GPU

::

    AMDGPU_ABM_LEVEL_ON_AC=0
    AMDGPU_ABM_LEVEL_ON_BAT=3

Enable display panel Adaptive Backlight Modulation (ABM).
Possible levels are:

* 0 – off
* 1..4 – maximum brightness reduction allowed by the ABM
  algorithm, 1 represents the least and 4 the most power saving

Notes:

* Requires AMD Vega or newer GPU with `amdgpu` driver as of kernel 6.9
* Savings are made at the expense of color balance

Default when unconfigured: 0 (AC), 1 (BAT)

.. seealso::

    * `AMDgpu kernel driver <https://docs.kernel.org/gpu/amdgpu/thermal.html#power-dpm-force-performance-level>`_ – Sysfs node documentation, see **power_dpm_force_performance_level**
    * `AMDgpu DRM driver <https://dri.freedesktop.org/docs/drm/gpu/amdgpu.html>`_ – see **abmlevel**
    * `Radeon KMS driver <https://wiki.x.org/wiki/RadeonFeature>`_ – see "KMS Power Management Options"

