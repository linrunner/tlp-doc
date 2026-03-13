Power Consumption
=================

High fan speed
---------------
Symptom: high fan speed on AC power.

Solution: refer to :ref:`opt-reduce-power-on-ac`.

----

Symptom: high fan speed during video playback.

Cause: missing hardware video acceleration.

Solution: check the web how to enable hardware video acceleration for
your Linux distribution and/or web browser.

----

Symptom: fan speed never goes down.

Solution: uninstall or disable **thermald**.


.. _faq-powercon-hybrid-graphics:

Hybrid graphics
---------------
When your laptop shows a much higher power consumption under Linux than with the
factory installed operating system, the most common cause is `hybrid graphics`.

Laptops with hybrid graphics contain two graphics processing units (GPUs),
the primary one on the Intel or AMD processor die ('integrated', iGPU) and
a secondary one by Nvidia or AMD ('discrete', dGPU).

Linux will use the iGPU by default, but the unoccupied dGPU may still draw lots
of power for nothing. Because of the large number of combinations of kernels,
drivers and hardware, TLP's default configuration may not always produce optimal
results.

Solution: try the following steps.

.. rubric:: Step 1: Use the dGPU drivers' defaults for Runtime Power Management

Add all dGPU drivers to the denylist (older versions have fewer defaults): ::

    RUNTIME_PM_DRIVER_DENYLIST="amdgpu mei_me nouveau nvidia xhci_hcd"

Reboot to apply the changed settings.

Anticipated result: the GPU will be suspended (`D3cold`) automatically when in
idle state.

.. note::

    * `nouveau`: the open source driver for Nvidia GPUs enables runtime power
      management by default; keep it in `RUNTIME_PM_DRIVER_DENYLIST`
      to enable power saving and power down (autosuspend) on AC too
    * `nvidia`: to enable power down (autosuspend) on AC with the proprietary
      Nvidia GPU driver, other related devices that are not easily identifiable
      (e.g., one of the PCI bridges) must be sent to autosuspend. Therefore,
      enable runtime power management generally with `RUNTIME_PM_ON_AC=auto`
      and enable audio power saving too (see below); also refer to:

      * `Nvidia's RTD3 guide <https://download.nvidia.com/XFree86/Linux-x86_64/450.57/README/dynamicpowermanagement.html>`_
      * `Issue #859 <https://github.com/linrunner/TLP/issues/859>`_

    * Remember to uncomment the config line by removing the leading `#`

.. rubric:: Step 2a: Switch to the iGPU
    - *Nvidia Optimus with proprietary driver and settings tool*

Start :command:`nvidia-settings`, goto `PRIME Profiles`, select `Intel (Power
Saving Mode)`. Logon again (or reboot).

Result: rendering the screen content is shifted to the iGPU, sending the dGPU to
idle state, thus permitting runtime power management to power down the dGPU
(see Step 1).

.. rubric:: Step 2b: Switch to the iGPU - *Other GPU hardware, drivers or tools*

* **Nvidia:** if you do not want to use :command:`nvidia-settings` or prefer to
  employ the open source driver `nouveau`, look into
  `Switcheroo and Optimus/Prime <http://nouveau.freedesktop.org/wiki/Optimus/>`_
  or `Optimus-Manager <https://github.com/Askannz/optimus-manager>`_
  (list does not claim to be exhaustive).
* **AMD:** your mileage will vary depending on the driver, use
  `amdgpu with Switcheroo (Gentoo Wiki) <https://wiki.gentoo.org/wiki/AMDGPU#AMDGPU.2FRadeonSI_drivers_do_not_work>`_
  as a starting point.

Also look for tools already integrated into your specific Linux distribution or
desktop. Last but not least, your laptop may permit to disable the dGPU in the
BIOS setup.

.. _faq-powercon-optimus-audio:

Nvidia Optimus and Audio power saving
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
*Concerns nouveau and nvidia drivers.*

Disabling audio power saving on AC prevents automatic power down of the dGPU.
Make sure you either use TLP's defaults or change your different configuration
to: ::

    SOUND_POWER_SAVE_ON_AC=1
    SOUND_POWER_SAVE_ON_BAT=1

.. seealso ::

    * Settings: :doc:`/settings/runtimepm`
    * Settings: :doc:`/settings/audio`
    * :doc:`/support/optimizing` - Improvements ordered by objectives
    * :doc:`/support/troubleshooting` - Provides help to isolate problems
      caused by TLP's power saving
