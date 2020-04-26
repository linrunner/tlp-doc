Graphics
========

Why does my display flicker when the power source changes?
-----------------------------------------------------------
.. rubric:: Radeon


This is caused by the radeon driver's power management. To disable the setting
either use ::

    RADEON_POWER_PROFILE_ON_AC=default
    RADEON_POWER_PROFILE_ON_BAT=default

or ::

    RADEON_DPM_STATE_ON_AC=performance
    RADEON_DPM_STATE_ON_BAT=performance

depending on your card/kernel. Refer to :ref:`set-graphics` for details.

Kernel message '*ERROR* radeon: ring 0 test failed' on AC power
---------------------------------------------------------------
.. rubric:: Radeon

Affected hardware: a Lenovo Ideapad 300 (Intel / Radeon hybrid graphics) user
reported this issue.

Solution: disable DPM like this ::

    RADEON_POWER_PROFILE_ON_AC=""

Flickering screen
-----------------
.. rubric:: Nvidia Optimus

Affected hardware: a Dell Latitude 5480 user reported 'screen flickers at an
inconsistent rate, though not that sporadic'.

Solution: disable ASPM on battery in the configuration ::

    PCIE_ASPM_ON_BAT=default

Why does my laptop consume so much battery power?
-------------------------------------------------
.. rubric:: Nvidia Optimus

Laptops with Optimus hybrid graphics contain two graphics units, one from Intel
on the processor die ('integrated', iGPU) and one from Nvidia ('discrete', dGPU).
Linux uses the Intel unit by default, but at the same time the unused Nvidia
unit might be enabled and using a lot of battery power because no driver is loaded.

TLP can't do anything about this – Possible solutions are:

* Disable one of the units in the BIOS setup
* Use a tool to disable the dGPU (see list below)

Significant loss of energy saving after upgrade to 1.0
------------------------------------------------------
Version 1.0 and higher exempt (blacklists) all dGPU's from runtime power
management. For certain hardware/kernel combinations this means that
the dGPU is no longer automatically deactivated at system startup.

Solution: remove dGPU's from the driver blacklist in the configuration ::

    RUNTIME_PM_DRIVER_BLACKLIST="mei_me pcieport"

or use a tool (see list below) to disable the dGPU upon boot.

I use PRIME to disable the dGPU, why is it always re-enabled after boot?
------------------------------------------------------------------------
.. rubric:: Nvidia Optimus

Exclude the dGPU from runtime power management.

Version 1.0 and higher: blacklist the driver(s) ::

    RUNTIME_PM_DRIVER_BLACKLIST="nouveau nvidia"

Version 0.9 and before: blacklist the device ::

    RUNTIME_PM_BLACKLIST="01:00.0"

Hint: to check whether '01:00.0' matches your Nvidia dGPU, use

.. code-block :: sh

    lspci -v | perl -ne '/VGA/../^$/ and /VGA|Kern/ and print'

Refer to :ref:`set-runtimepm` too.

Bumblebee fails to re-enable the dGPU
-------------------------------------
Exclude the dGPU from runtime power management, see the previous section.

.. seealso::

   Incomplete list of tools to handle hybrid graphics GPU switching:

    * `Switcheroo and Optimus/Prime <http://nouveau.freedesktop.org/wiki/Optimus/>`_
    * `Optimus-Manager <https://github.com/Askannz/optimus-manager>`_
    * `Bumblebee <http://bumblebee-project.org/>`_ (very deprecated)

   Also look for solutions already integrated into your Linux distribution or
   desktop.
