Graphics
========

INTEL_GPU_POWER_PROFILE_ON_PRF/BAL/SAV
--------------------------------------
.. rubric:: Intel GPU

Select the power profile for Intel Arc and Xe GPUs. Supported by the `xe` driver only (kernel 6.18 and newer).
Possible values:

* base
* power_saving

*Version 1.11 and newer* ::

    INTEL_GPU_POWER_PROFILE_ON_PRF=base
    INTEL_GPU_POWER_PROFILE_ON_BAL=power_saving
    INTEL_GPU_POWER_PROFILE_ON_SAV=power_saving

*Version 1.10* ::

    INTEL_GPU_POWER_PROFILE_ON_AC=base
    INTEL_GPU_POWER_PROFILE_ON_BAT=power_saving
    INTEL_GPU_POWER_PROFILE_ON_SAV=power_saving

Default when unconfigured: base (PRF/AC), power_saving (BAL/BAT, SAV)


INTEL_GPU_MIN/MAX/BOOST_FREQ_ON_PRF/BAL/SAV
-------------------------------------------
.. rubric:: Intel GPU

Set the min/max/turbo frequency for Intel GPUs. Supported by the `xe` and `ì915` drivers.
Possible values depend on your hardware. See the output of :command:`tlp-stat -g` for available
frequencies.

*Version 1.11 and newer* ::

    INTEL_GPU_MIN_FREQ_ON_PRF=0
    INTEL_GPU_MIN_FREQ_ON_BAL=0
    INTEL_GPU_MIN_FREQ_ON_SAV=0
    INTEL_GPU_MAX_FREQ_ON_PRF=0
    INTEL_GPU_MAX_FREQ_ON_BAL=0
    INTEL_GPU_MAX_FREQ_ON_SAV=0
    INTEL_GPU_BOOST_FREQ_ON_PRF=0
    INTEL_GPU_BOOST_FREQ_ON_BAL=0
    INTEL_GPU_BOOST_FREQ_ON_SAV=0

*Version 1.10* ::

    INTEL_GPU_MIN_FREQ_ON_AC=0
    INTEL_GPU_MIN_FREQ_ON_BAT=0
    INTEL_GPU_MIN_FREQ_ON_SAV=0
    INTEL_GPU_MAX_FREQ_ON_AC=0
    INTEL_GPU_MAX_FREQ_ON_BAT=0
    INTEL_GPU_MAX_FREQ_ON_SAV=0
    INTEL_GPU_BOOST_FREQ_ON_AC=0
    INTEL_GPU_BOOST_FREQ_ON_BAT=0
    INTEL_GPU_BOOST_FREQ_ON_SAV=0

*Version 1.9 and older* ::

    INTEL_GPU_MIN_FREQ_ON_AC=0
    INTEL_GPU_MIN_FREQ_ON_BAT=0
    INTEL_GPU_MAX_FREQ_ON_AC=0
    INTEL_GPU_MAX_FREQ_ON_BAT=0
    INTEL_GPU_BOOST_FREQ_ON_AC=0
    INTEL_GPU_BOOST_FREQ_ON_BAT=0

Default when unconfigured: <none>


RADEON_DPM_PERF_LEVEL_ON_PRF/BAL/SAV
------------------------------------
.. rubric::  AMD GPU

Controls the Dynamic power management (DPM) performance level of AMD GPUs.
Supported by the `amdgpu` and `radeon` drivers. Possible values:

* auto – dynamically select the optimal power profile for current conditions (recommended)
* low – lock the clocks to the lowest state
* high – lock the clocks to the highest state (beware of overheating on hardware with limited cooling)

*Version 1.11 and newer* ::

    RADEON_DPM_PERF_LEVEL_ON_PRF=auto
    RADEON_DPM_PERF_LEVEL_ON_BAL=auto
    RADEON_DPM_PERF_LEVEL_ON_SAV=low

*Versions 1.10 and 1.9* ::

    RADEON_DPM_PERF_LEVEL_ON_AC=auto
    RADEON_DPM_PERF_LEVEL_ON_BAT=auto
    RADEON_DPM_PERF_LEVEL_ON_SAV=low

*Version 1.8 and older* ::

    RADEON_DPM_PERF_LEVEL_ON_AC=auto
    RADEON_DPM_PERF_LEVEL_ON_BAT=auto

Default when unconfigured: Default: auto (PRF/AC), auto (BAL/BAT), low (SAV)


RADEON_DPM_STATE_ON_AC/BAT/SAV
------------------------------
.. rubric::  AMD GPU

Controls the power management method of AMD GPUs. Possible values:

* battery
* balanced
* performance

*Removed with version 1.11*

*Versions 1.10 through 1.9.1* ::

    RADEON_DPM_STATE_ON_AC=performance
    RADEON_DPM_STATE_ON_BAT=balanced
    RADEON_DPM_STATE_ON_SAV=balanced

*Version 1.9 and older* ::

    RADEON_DPM_STATE_ON_AC=performance
    RADEON_DPM_STATE_ON_BAT=balanced


.. _set-amdgpu-abm:

AMDGPU_ABM_LEVEL_ON_PRF/BAL/SAV
-------------------------------
.. rubric::  AMD GPU

Enable display panel Adaptive Backlight Modulation (ABM) of AMD GPUs.
Possible levels are:

* 0 – off
* 1..4 – maximum brightness reduction allowed by the ABM
  algorithm, 1 represents the least and 4 the most power saving

*Version 1.11 and newer* ::

    AMDGPU_ABM_LEVEL_ON_PRF=0
    AMDGPU_ABM_LEVEL_ON_BAL=1
    AMDGPU_ABM_LEVEL_ON_SAV=3

*Versions 1.10 through 1.9* ::

    AMDGPU_ABM_LEVEL_ON_AC=0
    AMDGPU_ABM_LEVEL_ON_BAT=1
    AMDGPU_ABM_LEVEL_ON_SAV=3

*Versions 1.8 through 1.7* ::

    AMDGPU_ABM_LEVEL_ON_AC=0
    AMDGPU_ABM_LEVEL_ON_BAT=1

Default when unconfigured: 0 (PRF/AC), 1 (BAL/BAT), 3 (SAV)

Notes:

* Requires AMD Vega or newer GPU with the `amdgpu` driver as of kernel 6.9
* Savings are made at the expense of color balance and contrast


.. seealso::

    * Settings: :doc:`/settings/introduction`
    * FAQ: :doc:`/faq/graphics`
    * FAQ: :doc:`/faq/powercon`
    * `AMDgpu kernel driver <https://docs.kernel.org/gpu/amdgpu/thermal.html#power-dpm-force-performance-level>`_ – Sysfs node documentation, see **power_dpm_force_performance_level**
    * `AMDgpu DRM driver <https://dri.freedesktop.org/docs/drm/gpu/amdgpu.html>`_ – see **abmlevel**
    * `Testing the new experimental Intel Xe driver <https://wiki.archlinux.org/title/Intel_graphics#Testing_the_new_experimental_Xe_driver>`_ – Arch Linux Wiki
    * `Radeon KMS driver <https://wiki.x.org/wiki/RadeonFeature>`_ – see "KMS Power Management Options"
