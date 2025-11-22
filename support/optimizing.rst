Optimizing Guide
================
This page is intended to help tune TLP individually to achieve additional
battery runtime or better performance on AC if possible.

Rationale: TLP's default settings should save as much energy on
battery power as possible (and of course not cost any performance on AC),
but in reality there are limits to what can be done:

* For simplicity, TLP's settings are limited to three sets/profiles
  (*Version 1.9 and newer*) or one set each for AC and battery operation
  (*Version 1.8 and older*). The same applies to the defaults.
* User requirements can vary greatly, there is no way to cover them all
  within defaults
* Defaults must not cause problems with common hardware

Therefore TLP's defaults can not achieve the maximum savings for any
hardware and use case. It may be necessary to make adjustments yourself
to get the best out of it.

.. note::

    * TLP's parameters must always be specified for all profiles, i.e. `_AC`,
      `_BAT` and `_SAV` where applicable, otherwise, parameters from the
      configured profiles may unintentionally spill over to the unconfigured
      ones. Therefore all parameters are listed below, the one
      to be changed is highlighted.
    * Please activate and test the following suggestions individually if
      possible. This shows the specific effect, and problems are immediately
      apparent.
    * Feel free to try out a suggested change in one of the other scenarios.
    * If adjusting the settings does not yield the desired results,
      it may be worthwhile to experiment with a more recent kernel or
      BIOS version.
    * The inconsistent use of terms such as `performance`, `balance(d)` and
      `(low-)power` in settings and for profiles is confusing,
      but this is specified by the kernel and the power profiles API and
      therefore cannot be avoided. The following examples will hopefully help.

Extend battery runtime
----------------------
To extend battery life, you can manually switch to the `power-saver` profile.

If you don't want to do this every time, you can adjust the `balanced` profile,
which is automatically selected when changing to battery power.
The following describes the adjustment of the `balanced` profile.

1. Change :ref:`CPU energy/performance policy <set-cpu-energy-perf-policy>`
   to `power` (default is `balance_power`):

.. code-block::
    :emphasize-lines: 2

    CPU_ENERGY_PERF_POLICY_ON_AC=balance_performance
    CPU_ENERGY_PERF_POLICY_ON_BAT=power
    CPU_ENERGY_PERF_POLICY_ON_SAV=power

2. Change the :ref:`platform profile <set-platform-profile>` to `low-power`
   (default is `balanced`):

.. code-block::
    :emphasize-lines: 2

    PLATFORM_PROFILE_ON_AC=performance
    PLATFORM_PROFILE_ON_BAT=low-power
    PLATFORM_PROFILE_ON_SAV=low-power

3. Disable :ref:`turbo boost <set-cpu-boost>`:

.. code-block::
    :emphasize-lines: 2, 3, 6, 7

    CPU_BOOST_ON_AC=1
    CPU_BOOST_ON_BAT=0
    CPU_BOOST_ON_SAV=0

    CPU_HWP_DYN_BOOST_ON_AC=1
    CPU_HWP_DYN_BOOST_ON_BAT=0
    CPU_HWP_DYN_BOOST_ON_SAV=0

Note: the `power-saver` profile does not disable turbo boost by default.
However, you can configure this additionally as shown.

4. Increase :ref:`ABM level <set-amdgpu-abm>` to `3` (default is `1`):

.. code-block::
    :emphasize-lines: 2

    AMDGPU_ABM_LEVEL_ON_AC=0
    AMDGPU_ABM_LEVEL_ON_BAT=3
    AMDGPU_ABM_LEVEL_ON_SAV=3

Improve performance on battery power
------------------------------------
5. Change :ref:`CPU energy/performance policy <set-cpu-energy-perf-policy>`
   to `balance_performance` (default is `balance_power`):

.. code-block::
    :emphasize-lines: 2

    CPU_ENERGY_PERF_POLICY_ON_AC=balance_performance
    CPU_ENERGY_PERF_POLICY_ON_BAT=balance_performance
    CPU_ENERGY_PERF_POLICY_ON_SAV=power

Note: this may be necessary in particular if the Intel Core i 12th gen. refuses to activate
turbo boost on battery power.

6. Change the :ref:`platform profile <set-platform-profile>` to `performance`
   (default is `balanced`):

.. code-block::
    :emphasize-lines: 2

    PLATFORM_PROFILE_ON_AC=performance
    PLATFORM_PROFILE_ON_BAT=performance
    PLATFORM_PROFILE_ON_SAV=power-saver

Improve performance on AC power
-------------------------------
7. Change :ref:`CPU energy/performance policy <set-cpu-energy-perf-policy>`
   to `performance` (default is `balance_performance`):

.. code-block::
    :emphasize-lines: 1

    CPU_ENERGY_PERF_POLICY_ON_AC=performance
    CPU_ENERGY_PERF_POLICY_ON_BAT=balance_power
    CPU_ENERGY_PERF_POLICY_ON_SAV=power

8. Enable the :ref:`platform profile <set-platform-profile>` `performance`:

*Version 1.8 and older only*

.. code-block::
    :emphasize-lines: 1

    PLATFORM_PROFILE_ON_AC=performance
    PLATFORM_PROFILE_ON_BAT=balanced


.. _opt-reduce-power-on-ac:

Reduce power consumption / fan noise on AC power
------------------------------------------------
9. Enable :doc:`runtime power management </settings/runtimepm>`:

.. code-block::
    :emphasize-lines: 1

    RUNTIME_PM_ON_AC=auto
    RUNTIME_PM_ON_BAT=auto

10. Change :ref:`CPU energy/performance policy <set-cpu-energy-perf-policy>`
    to `balance_power` (default is `balance_performance`):

.. code-block::
    :emphasize-lines: 1

    CPU_ENERGY_PERF_POLICY_ON_AC=balance_power
    CPU_ENERGY_PERF_POLICY_ON_BAT=balance_power

11. Change the :ref:`platform profile <set-platform-profile>` to `balanced`
    (default is `performance`):

.. code-block::
    :emphasize-lines: 1

    PLATFORM_PROFILE_ON_AC=balanced
    PLATFORM_PROFILE_ON_BAT=balanced
    PLATFORM_PROFILE_ON_SAV=low-power

12. Enable :ref:`Wi-Fi power save <set-wifi-pwr>` (default is `off`):

.. code-block::
    :emphasize-lines: 1

    WIFI_PWR_ON_AC=on
    WIFI_PWR_ON_BAT=on

.. seealso::

    Missing *hardware video acceleration* and *hybrid graphics* are other
    common causes of high fan speed and elevated power dissipation.
    There is more about these topics in the FAQ: :doc:`/faq/powercon`.

.. _faq-powercon-high-cpu-load:

Limit power consumption under high CPU load
-------------------------------------------
13. The `intel_pstate` scaling driver offers this :ref:`possibility <set-cpu-min-max-perf>`.
Employ the settings

.. code-block::
    :emphasize-lines: 1, 2

    CPU_MAX_PERF_ON_AC=nn
    CPU_MAX_PERF_ON_BAT=nn

with `nn` < 100 to achieve it.

.. note::

    * Check the output of :command:`tlp-stat -p` to determine the active
      scaling driver
    * This will not limit the power consumption of the GPU (neither
      for integrated nor for discrete graphics)


.. seealso::

    * :doc:`/faq/powercon` (FAQ) - More about the topics *fan noise* and *power consumption*
    * :doc:`/support/troubleshooting` - Provides help to isolate problems
      caused by TLP's power saving
