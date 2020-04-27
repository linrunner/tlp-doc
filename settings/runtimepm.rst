Runtime Power Management and ASPM
=================================
.. rubric:: Scope:

* Attached PCIe bus devices

RUNTIME_PM_ON_AC/BAT
--------------------
::

    RUNTIME_PM_ON_AC=on
    RUNTIME_PM_ON_BAT=auto

Controls runtime power management for PCIe devices. Possible values:

* auto – enabled (power down idle devices)
* on – disabled (devices powered on permanently)

Hint: to disable this setting completely, insert a `#` in the first column.

.. _set-runtimepm-blacklist:

RUNTIME_PM_BLACKLIST
--------------------
::

    RUNTIME_PM_BLACKLIST="00:12.3 00:45.6"

Exclude listed PCIe device addresses from runtime power management. Use
:command:`lspci` to lookup the addresses (first output column).

.. _set-runtimepm-driver-blacklist:

RUNTIME_PM_DRIVER_BLACKLIST
---------------------------
::

    RUNTIME_PM_DRIVER_BLACKLIST="amdgpu mei_me nouveau nvidia pcieport radeon"

Exclude PCIe devices assigned to listed drivers from runtime power management.
Use :command:`tlp-stat -e` to lookup the drivers (in parentheses at the end of
each output line).

Separate multiple drivers with spaces.

Default when unconfigured: "amdgpu mei_me nouveau nvidia pcieport radeon"

.. note::

    The default serves to prevent accidential power on of hybrid graphics' discrete
    part. Use an empty list ("") to disable the feature completely (not recommended).


PCIE_ASPM_ON_AC/BAT
-------------------
.. rubric:: Active State Power Management (ASPM)

::

    PCIE_ASPM_ON_AC=default
    PCIE_ASPM_ON_BAT=default

Sets PCIe ASPM power saving mode. Possible values:

* default – recommended
* performance
* powersave
* powersupersave

.. note::

    Using `performance` can lead to increasing power consumption and higher
    temperatures because deeper sleep states of the CPU are no longer reached;
    `default` does not cause this.
    See `Issue #344 <https://github.com/linrunner/TLP/issues/344>`_.

.. seealso::

    * `Runtime power management <https://www.kernel.org/doc/Documentation/power/runtime_pm.txt>`_ – Kernel documentation
    * `Making sense of PCIe ASPM <http://smackerelofopinion.blogspot.de/2011/03/making-sense-of-pcie-aspm.html>`_ – PCI Express Active State Power Management
