Graphics
========

.. seealso::

    Power saving for hybrid graphics is covered in
    :ref:`faq-powercon-hybrid-graphics`.

Screen flicker
---------------
.. rubric:: Nvidia dGPU

Affected hardware: a Dell Latitude 5480 user reported *"screen flickers at an
inconsistent rate, though not that sporadic"*.

Solution: disable ASPM on battery in the configuration ::

    PCIE_ASPM_ON_BAT=default

.. rubric:: AMD GPU

Symptom: screen flickers briefly when the power source changes.

Solution: disable the `radeon` driver's power saving: ::

    RADEON_DPM_STATE_ON_AC=performance
    RADEON_DPM_STATE_ON_BAT=performance

Refer to :doc:`/settings/graphics` for details.

.. note::

    Affects `radeon` driver only, not `amdgpu`.

Kernel message "*ERROR* radeon: ring 0 test failed' on AC power"
----------------------------------------------------------------
.. rubric:: AMD GPU

Affected hardware: a Lenovo Ideapad 300 (Intel / Radeon hybrid graphics) user
reported this issue.

Solution: disable DPM like this ::

    RADEON_POWER_PROFILE_ON_AC=""

.. note::

    Affects `radeon` driver only, not `amdgpu`.

