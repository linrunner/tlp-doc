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

Default when unconfigured: on (AC), auto (BAT)


.. _set-runtimepm-denylist:
.. _RUNTIME_PM_BLACKLIST:

RUNTIME_PM_DENYLIST
--------------------
*This parameter was renamed with version 1.4. In 1.3.1 and below it is called
RUNTIME_PM_BLACKLIST. 1.4 and higher also recognize the old name.*

::

    RUNTIME_PM_DENYLIST="11:22.3 44:55.6"

Exclude listed PCIe device addresses from runtime power management. Use
:command:`lspci` to lookup the addresses (first output column).

.. important::

    * Exclusion of a device means that the value `on` or `auto` as initialized by
      the kernel default at system startup is used and TLP won't touch it at all

    * A device initialized to `auto` by the kernel cannot be set to `on` by
      entering it into `RUNTIME_PM_DENYLIST`. Instead use `RUNTIME_PM_DISABLE`
      below.


.. _set-runtimepm-driver-denylist:
.. _RUNTIME_PM_DRIVER_BLACKLIST:

RUNTIME_PM_DRIVER_DENYLIST
---------------------------
*This parameter was renamed with version 1.4. In 1.3.1 and below it is called
RUNTIME_PM_DRIVER_BLACKLIST. 1.4 and higher also recognize the old name.*

::

    RUNTIME_PM_DRIVER_DENYLIST="mei_me nouveau radeon"

Exclude PCIe devices assigned to listed drivers from runtime power management.
Use :command:`tlp-stat -e` to lookup the drivers (in parentheses at the end of
each output line).

Separate multiple drivers with spaces.

Default when unconfigured:

    | "mei_me nouveau radeon" - *Version 1.4 and higher*
    | "amdgpu mei_me nouveau nvidia pcieport radeon" - *Version 1.3.1 and lower*

.. note::

    The default serves to prevent accidential power on of hybrid graphics' discrete
    part. Use an empty list ("") to disable the feature completely (not recommended).

.. important::

    A device initialized to `auto` by the kernel cannot be set to `on` by
    entering its driver into `RUNTIME_PM_DRIVER_DENYLIST`. Instead use
    `RUNTIME_PM_DISABLE` below.


RUNTIME_PM_ENABLE
-----------------
*Version 1.4 and higher*

::

    RUNTIME_PM_ENABLE="11:22.3"

Permanently enable (`auto`) Runtime PM for PCI(e) device addresses in the
list (independent of the power source). This has priority over all
preceding Runtime PM settings. Separate multiple addresses with spaces.
Use :command:`lspci` to get the adresses (1st column).


RUNTIME_PM_DISABLE
------------------
*Version 1.4 and higher*

::

    RUNTIME_PM_DISABLE="44:55.6"

Permanently disable (`on`) Runtime PM for PCI(e) device addresses in the
list. The configuration works as for `RUNTIME_PM_ENABLE` above.


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

Default when unconfigured: default

.. note::

    Using `performance` can lead to increasing power consumption and higher
    temperatures because deeper sleep states of the CPU are no longer reached;
    `default` does not cause this.
    See `Issue #344 <https://github.com/linrunner/TLP/issues/344>`_.

.. seealso::

    * `Runtime power management <https://www.kernel.org/doc/Documentation/power/runtime_pm.txt>`_ – Kernel documentation
    * `Making sense of PCIe ASPM <http://smackerelofopinion.blogspot.de/2011/03/making-sense-of-pcie-aspm.html>`_ – PCI Express Active State Power Management
