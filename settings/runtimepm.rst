PCIe Autosuspend and ASPM
=========================
.. rubric:: Scope:

* Builtin or externally attached PCIe bus devices


RUNTIME_PM_ON_AC/BAT
--------------------
Controls autosuspend for PCIe devices. Possible values:

* auto – enabled (power down idle devices)
* on – disabled (devices powered on permanently)

*Version 1.11 and newer* ::

    RUNTIME_PM_ON_PRF=on
    RUNTIME_PM_ON_BAL=auto
    RUNTIME_PM_ON_SAV=auto

*Versions 1.10 through 1.9* ::

    RUNTIME_PM_ON_AC=on
    RUNTIME_PM_ON_BAT=auto

Default when unconfigured: on (PRF/AC), auto (BAL/BAT, SAV)


.. _set-runtimepm-denylist:
.. _RUNTIME_PM_BLACKLIST:

RUNTIME_PM_DENYLIST
--------------------
Exclude listed PCIe device addresses from autosuspend. Use
:command:`lspci` to lookup the addresses (first output column). ::

    RUNTIME_PM_DENYLIST="11:22.3 44:55.6"

Default when unconfigured: <none>

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
Exclude PCIe devices assigned to listed drivers from autosuspend.
Use :command:`tlp-stat -e` to lookup the drivers (in parentheses at the end of
each output line).

Separate multiple drivers with spaces. ::

    RUNTIME_PM_DRIVER_DENYLIST="mei_me nouveau radeon xhci_hcd"

Default when unconfigured:

    | "amdgpu mei_me nouveau nvidia xhci_hcd" – *Version 1.10 and newer*
    | "mei_me nouveau radeon xhci_hcd" – *Versions 1.9.1 through 1.7*

.. note::

    The default serves to prevent accidential power on of hybrid graphics' discrete
    part. Use an empty list ("") to disable the feature completely (not recommended).

.. important::

    A device initialized to `auto` by the kernel cannot be set to `on` by
    entering its driver into `RUNTIME_PM_DRIVER_DENYLIST`. Instead use
    `RUNTIME_PM_DISABLE` below.


RUNTIME_PM_ENABLE
-----------------
Permanently enable (`auto`) autosuspend for PCI(e) device addresses in the
list (independent of the power source). This has priority over all
preceding Runtime PM settings. Separate multiple addresses with spaces.
Use :command:`lspci` to get the adresses (1st column). ::

    RUNTIME_PM_ENABLE="11:22.3"

Default when unconfigured: <none>


RUNTIME_PM_DISABLE
------------------
Permanently disable (`on`) autosuspend for PCI(e) device addresses in the
list. The configuration works as for `RUNTIME_PM_ENABLE` above. ::

    RUNTIME_PM_DISABLE="44:55.6"

Default when unconfigured: <none>


PCIE_ASPM_ON_AC/BAT
-------------------
.. rubric:: Active State Power Management (ASPM)

Sets PCIe ASPM power saving mode. Possible values:

* default – recommended
* performance
* powersave
* powersupersave

*Version 1.11 and newer* ::

    PCIE_ASPM_ON_PRF=default
    PCIE_ASPM_ON_BAL=default
    PCIE_ASPM_ON_SAV=default

*Versions 1.10 through 1.9* ::

    PCIE_ASPM_ON_AC=default
    PCIE_ASPM_ON_BAT=default
    PCIE_ASPM_ON_SAV=default

*Version 1.8 and older* ::

    PCIE_ASPM_ON_AC=default
    PCIE_ASPM_ON_BAT=default

Default when unconfigured: default

.. note::

    Using `performance` can lead to increasing power consumption and higher
    temperatures because deeper sleep states of the CPU are no longer reached;
    `default` does not cause this.
    See `Issue #344 <https://github.com/linrunner/TLP/issues/344>`_.


.. seealso::

    * Settings: :doc:`/settings/introduction`
    * FAQ: :doc:`/faq/operation`
    * `Runtime power management <https://www.kernel.org/doc/Documentation/power/runtime_pm.txt>`_ – Kernel documentation
    * `Making sense of PCIe ASPM <http://smackerelofopinion.blogspot.de/2011/03/making-sense-of-pcie-aspm.html>`_ – PCI Express Active State Power Management
