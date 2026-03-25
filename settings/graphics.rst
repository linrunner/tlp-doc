Graphics
========

INTEL_GPU_POWER_PROFILE_ON_AC/BAT/SAV
-------------------------------------
.. rubric:: Intel GPU

*Version 1.10 and newer* ::

    INTEL_GPU_POWER_PROFILE_ON_AC=base
    INTEL_GPU_POWER_PROFILE_ON_BAT=base
    INTEL_GPU_POWER_PROFILE_ON_SAV=power_saving

Select power profile for Intel Arc and Xe GPUs. Supported by the `xe` driver only (kernel 6.18 and newer).
Possible values:

* base
* power_saving

Default when unconfigured: base (AC), base (BAT), power_saving (SAV)


INTEL_GPU_MIN/MAX/BOOST_FREQ_ON_AC/BAT
--------------------------------------
.. rubric:: Intel GPU

::

    INTEL_GPU_MIN_FREQ_ON_AC=0
    INTEL_GPU_MIN_FREQ_ON_BAT=0
    INTEL_GPU_MAX_FREQ_ON_AC=0
    INTEL_GPU_MAX_FREQ_ON_BAT=0
    INTEL_GPU_BOOST_FREQ_ON_AC=0
    INTEL_GPU_BOOST_FREQ_ON_BAT=0

Set the min/max/turbo frequency for Intel GPUs. Supported by the `xe` and `ì915` drivers.
Possible values depend on your hardware. See the output of :command:`tlp-stat -g` for available
frequencies.


RADEON_DPM_PERF_LEVEL_ON_AC/BAT/SAV
-----------------------------------
.. rubric::  AMD GPU

::

    RADEON_DPM_PERF_LEVEL_ON_AC=auto
    RADEON_DPM_PERF_LEVEL_ON_BAT=auto

*Version 1.9 and newer* ::

    RADEON_DPM_PERF_LEVEL_ON_BAT=low

Controls the Dynamic power management (DPM) performance level of AMD GPUs.
Supported by the `amdgpu` and `radeon` drivers. Possible values:

* auto – dynamically select the optimal power profile for current conditions (recommended)
* low – lock the clocks to the lowest state
* high – lock the clocks to the highest state (beware of overheating on hardware with limited cooling)

Default when unconfigured: Default: auto (AC), auto (BAT), low (SAV)


RADEON_DPM_STATE_ON_AC/BAT
--------------------------
.. rubric::  AMD GPU

*This parameter was deprecated in version 1.10, do not use anymore* ::

    RADEON_DPM_STATE_ON_AC=performance
    RADEON_DPM_STATE_ON_BAT=balanced

Controls the power management method of AMD GPUs. Possible values:

* battery
* balanced
* performance


.. _set-amdgpu-abm:

AMDGPU_ABM_LEVEL_ON_AC/BAT/SAV
------------------------------
.. rubric::  AMD GPU

*Version 1.7 and newer* ::

    AMDGPU_ABM_LEVEL_ON_AC=0
    AMDGPU_ABM_LEVEL_ON_BAT=1

*Version 1.9 and newer* ::

    AMDGPU_ABM_LEVEL_ON_SAV=3

Enable display panel Adaptive Backlight Modulation (ABM) of AMD GPUs.
Possible levels are:

* 0 – off
* 1..4 – maximum brightness reduction allowed by the ABM
  algorithm, 1 represents the least and 4 the most power saving

Notes:

* Requires AMD Vega or newer GPU with the `amdgpu` driver as of kernel 6.9
* Savings are made at the expense of color balance

Default when unconfigured: 0 (AC), 1 (BAT), 3 (SAV)

.. seealso::

    * Settings: :doc:`/settings/introduction`
    * FAQ: :doc:`/faq/graphics`
    * FAQ: :doc:`/faq/powercon`
    * `AMDgpu kernel driver <https://docs.kernel.org/gpu/amdgpu/thermal.html#power-dpm-force-performance-level>`_ – Sysfs node documentation, see **power_dpm_force_performance_level**
    * `AMDgpu DRM driver <https://dri.freedesktop.org/docs/drm/gpu/amdgpu.html>`_ – see **abmlevel**
    * `Testing the new experimental Intel Xe driver <https://wiki.archlinux.org/title/Intel_graphics#Testing_the_new_experimental_Xe_driver>`_ - Arch Linux Wiki
    * `Radeon KMS driver <https://wiki.x.org/wiki/RadeonFeature>`_ – see "KMS Power Management Options"
