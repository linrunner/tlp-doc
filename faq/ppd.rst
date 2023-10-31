power-profiles-daemon
=====================
Most distributions install power-profiles-daemon by default with their
GNOME and KDE desktop environments. This article compares TLP and
power-profiles-daemon and explains how to replace the latter with TLP.

Does power-profiles-daemon achieve better power savings than TLP?
-----------------------------------------------------------------
power-profiles-daemon covers only a subset of TLP's settings:

* `PLATFORM_PROFILE_ON_AC/BAT`
* `CPU_ENERGY_PERF_POLICY_ON_AC/BAT`
* `CPU_BOOST_ON_AC/BAT`

Due to the choice of settings, power-profiles-daemon represents a CPU
throttle which mainly takes effect when the CPU is under load by applications.
power-profiles-daemon does *not* include any settings to reduce power
consumption when the CPU is idle like TLP provides them.

So it depends on your workload:

* If the laptop often runs under medium or high load (e.g. video playback,
  compiling, etc.), then power-profiles-daemon using its `power-saver`
  profile may achieve savings similar to TLP
* When the laptop is mostly idle or running with low load (e.g. text editing,
  browsing, etc.), then TLP offers advantages over power-profiles-daemon

Finally, note that the TLP's default settings do not enable `PLATFORM_PROFILE_ON_AC/BAT`
and `CPU_BOOST_ON_AC/BAT`. To achieve truly comparable results, they must
be explicitly configured in TLP.
One also needs to know that many (especially older) laptops do not
support `PLATFORM_PROFILE_ON_AC/BAT`.

**Conclusion:** to find out which tool ultimately delivers better results,
do comparative measurements on your target hardware with your usage pattern.

Does power-profiles-daemon conflict with TLP?
---------------------------------------------
Yes. power-profiles-daemon uses kernel settings that TLP also
controls (see above). Using both tools simultaneously means that TLP’s
settings get overwritten by power-profiles-daemon (and vice versa)
resulting in unpredictable outcomes for the settings in question.

For this reason many distributions prevent the simultaneous installation
of their packages for TLP and power-profiles-daemon. In all other cases,
*uninstalling* power-profiles-daemon *is recommended*.


How can I achieve the effect of power-profiles-daemon with TLP?
---------------------------------------------------------------
First of all, it is important to understand the fundamental differences
between the two tools:

* TLP implements an "automatic transmission". Depending on the power source
  the corresponding one of the two settings profiles `AC` or `BAT` is applied
  *automatically*.
* power-profiles-daemon implements a "manual transmission", meaning there are
  three profiles ("gears") `power-saver`, `balanced` and `performance`,
  which you have to select *manually* via the panel applet. Nothing happens
  automatically. The preset profile is `balanced`.
* power-profiles-daemon's profiles' settings are hardcoded, while TLP's
  profiles can be customized as desired.

From what has been said above, it is clear that the behavior of power-profiles-daemon
cannot be completely reproduced with TLP. It is possible to map two of
power-profiles-daemon's profiles to the `AC` and the `BAT` profile of TLP
as shown in the following examples:

.. rubric:: Example 1: map `balanced` to `AC` and `power-saver` to `BAT`

.. code-block:: none

    PLATFORM_PROFILE_ON_AC=balanced
    PLATFORM_PROFILE_ON_BAT=low-power

    CPU_ENERGY_PERF_POLICY_ON_AC=balance_performance
    CPU_ENERGY_PERF_POLICY_ON_BAT=power

    CPU_BOOST_ON_AC=1
    CPU_BOOST_ON_BAT=0

.. rubric:: Example 2: map `performance` to AC and `balanced` to BAT

.. code-block:: none

    PLATFORM_PROFILE_ON_AC=performance
    PLATFORM_PROFILE_ON_BAT=balanced

    CPU_ENERGY_PERF_POLICY_ON_AC=performance
    CPU_ENERGY_PERF_POLICY_ON_BAT=balance_performance

    CPU_BOOST_ON_AC=1
    CPU_BOOST_ON_BAT=0

Last but not least you may select TLP's profile manually with a
:doc:`terminal command </usage/tlp>`:

.. code-block:: sh

    sudo tlp ac
    sudo tlp bat


.. seealso::

    * Settings: :doc:`/settings/platform`
    * Settings: :doc:`/settings/processor`
    * :doc:`/support/optimizing`
    * `TLP Issue #564 <https://github.com/linrunner/TLP/issues/564>`_
    * `power-profiles-daemon <https://gitlab.freedesktop.org/hadess/power-profiles-daemon>`_
      - Project homepage
