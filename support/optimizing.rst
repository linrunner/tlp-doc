Optimizing Guide
================
This page is intended to help tune TLP individually to achieve additional
battery runtime or better performance on AC if possible.

Rationale: TLP's default settings should save as much energy on
battery power as possible (and of course not cost any performance on AC),
but in reality there are limits to what can be done:

* For simplicity, TLP's settings are limited to a single set for AC
  and battery respectively, the same applies to the defaults
* User requirements can vary greatly, there is no way to cover them all
  within defaults
* Defaults must not cause problems with common hardware

Therefore TLP's defaults can not achieve the maximum savings for any
hardware and use case. It may be necessary to make adjustments yourself
to get the optimum out of it.

.. note::

    * TLP's parameters must always be specified pairwise for AC and BAT
      respectively. Therefore both parameters are listed below, the one
      to be changed is highlighted.
    * Please activate and test the following suggestions individually if
      possible. This way, any problems that may occur will stand out
      immediately.
    * **If adjusting the settings does not yield the desired results,
      it may be worthwhile to experiment with a more recent kernel.**

Extend battery runtime
----------------------
1. Change :ref:`CPU energy/performance policy <set-cpu-energy-perf-policy>`
   to `power` (default is `balance_power`):

.. code-block::
    :emphasize-lines: 2

    CPU_ENERGY_PERF_POLICY_ON_AC=balance_performance
    CPU_ENERGY_PERF_POLICY_ON_BAT=power

2. Enable the :ref:`platform profile <set-platform-profile>` `low-power`:

.. code-block::
    :emphasize-lines: 2

    PLATFORM_PROFILE_ON_AC=balanced
    PLATFORM_PROFILE_ON_BAT=low-power

3. Disable :ref:`turbo boost <set-cpu-boost>`:

.. code-block::
    :emphasize-lines: 2, 5

    CPU_BOOST_ON_AC=1
    CPU_BOOST_ON_BAT=0

    CPU_HWP_DYN_BOOST_ON_AC=1
    CPU_HWP_DYN_BOOST_ON_BAT=0


Improve performance on AC power
-------------------------------
4. Change :ref:`CPU energy/performance policy <set-cpu-energy-perf-policy>`
   to `performance` (default is `balance_performace`):

.. code-block::
    :emphasize-lines: 1

    CPU_ENERGY_PERF_POLICY_ON_AC=performance
    CPU_ENERGY_PERF_POLICY_ON_BAT=balance_power

5. Enable the :ref:`platform profile <set-platform-profile>` `performance`:

.. code-block::
    :emphasize-lines: 1

    PLATFORM_PROFILE_ON_AC=performance
    PLATFORM_PROFILE_ON_BAT=balanced


.. _opt-reduce-power-on-ac:

Reduce power consumption / fan noise on AC power
------------------------------------------------
6. Enable :doc:`runtime power management </settings/runtimepm>`:

.. code-block::
    :emphasize-lines: 1

    RUNTIME_PM_ON_AC=auto
    RUNTIME_PM_ON_BAT=auto

7. Change :ref:`CPU energy/performance policy <set-cpu-energy-perf-policy>`
   to `balance_power` (default is `balance_performance`):

.. code-block::
    :emphasize-lines: 1

    CPU_ENERGY_PERF_POLICY_ON_AC=balance_power
    CPU_ENERGY_PERF_POLICY_ON_BAT=balance_power

8. Enable :ref:`Wi-Fi power save <set-wifi-pwr>` (default is `off`):

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
9. The `intel_pstate` scaling driver offers this :ref:`possibility <set-cpu-min-max-perf>`.
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


Improve performance on battery power
------------------------------------
10. In case Intel Core i 12th gen. refuses to activate turbo boost on battery
    you can change the :ref:`CPU energy/performance policy <set-cpu-energy-perf-policy>`:

.. code-block::
    :emphasize-lines: 2

    CPU_ENERGY_PERF_POLICY_ON_AC=balance_performance
    CPU_ENERGY_PERF_POLICY_ON_BAT=balance_performance

.. seealso::

    * :doc:`/faq/powercon` (FAQ) - More about the topics *fan noise* and *power consumption*
    * :doc:`/support/troubleshooting` - Provides help to isolate problems
      caused by TLP's power saving
