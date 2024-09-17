power-profiles-daemon
=====================
Most distributions install power-profiles-daemon by default with their
GNOME and KDE desktop environments. This article compares TLP and
power-profiles-daemon and explains how to replace the latter with TLP.

Does power-profiles-daemon save power over TLP?
-----------------------------------------------
Simply put, power-profiles-daemon is a CPU throttle that is most
effective when high or medium load applications are in use.
Unlike TLP, it has no settings to reduce power consumption when the CPU
is idle, such as when there is no user input.

power-profiles-daemon only covers a subset of TLP's settings:

1. `PLATFORM_PROFILE_ON_AC/BAT`
2. `CPU_ENERGY_PERF_POLICY_ON_AC/BAT`
3. `CPU_BOOST_ON_AC/BAT`

The second item is only activated if the first item is not supported
by the hardware. When the second item is active, turbo boost (third item)
is also disabled at high CPU temperatures.

Which of these tools will save you more power depends on your workload:

* If the laptop frequently runs under medium or high load, such as during
  video playback or compiling, using the `power-saver` profile with
  power-profiles-daemon can provide similar energy savings as TLP.
* However, TLP offers advantages over power-profiles daemon when the laptop
  is idle, such as during periods of no user input or low load operations
  like text editing or browsing.

Note that TLP's default settings do not enable `PLATFORM_PROFILE_ON_AC/BAT`
and `CPU_BOOST_ON_AC/BAT`. To achieve truly comparable results, these settings
must be explicitly configured in TLP.
It is also important to note that some laptops, especially older ones, may not
support `PLATFORM_PROFILE_ON_AC/BAT`.

**Conclusion:** To determine which tool provides superior results, conduct
comparative measurements on your target hardware using your specific usage
pattern.


.. _faq-ppd-conflict:

Does power-profiles-daemon conflict with TLP?
---------------------------------------------
Yes, it does. Using both tools at the same time can lead to unpredictable
results as they partly change the same kernel tunables (see above)
and overwrite each other's tuning.

To prevent conflicts, many Linux distributions do not permit the
installation of both TLP and power-profiles-daemon packages simultaneously.
It is advisable to uninstall power-profiles-daemon in all other scenarios.

If your distribution allows for parallel installation, the behaviour
depends on the version of TLP:

*Version 1.6* and later will automatically *not* apply the settings listed
above if a running power-profiles-daemon is detected. In addition the
:command:`tlp start` issues a warning about the conflict:

.. code-block:: none

    Warning: PLATFORM_PROFILE_ON_AC/BAT is not set because power-profiles-daemon is running.

*Version 1.5 and 1.4* merely detect the situation and the commands
:command:`tlp start` and :command:`tlp-stat -s` issue a message about the conflict:

.. code-block:: none

    Error: conflicting power-profiles-daemon.service is enabled, power saving will not apply on boot.
    >>> Invoke 'systemctl mask power-profiles-daemon.service' to correct this!

If you do not want to use power-profiles-daemon in parallel, you can either
uninstall the package (preferred) or, if that would uninstall essential packages,
stop and disable the service with ::

    sudo systemctl stop power-profiles-daemon.service
    sudo systemctl mask power-profiles-daemon.service


How can I use TLP to achieve the same effect as power-profiles-daemon?
----------------------------------------------------------------------
Firstly, it is important to understand the fundamental differences
between the two tools:

* TLP *automatically* selects the corresponding profile based on the power
  source: either `AC` or `BAT`.
* power-profiles-daemon offers three profiles: `power-saver`, `balanced`
  and `performance`. These profiles must be *manually* selected through
  the panel applet, as there is no automatic  switching between them.
  The default profile is `balanced`.
* The settings for power-profiles-daemon's profiles are hardcoded, while
  TLP's profiles can be customized to meet specific needs.

It is evident from the above that TLP cannot fully replicate the behavior
of power-profiles-daemon. However, two of power-profiles-daemon's profiles
can be mapped to TLP's `AC` and `BAT` profiles, as demonstrated in the
following examples:

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
    * `power-profiles-daemon <https://gitlab.freedesktop.org/upower/power-profiles-daemon>`_
      - Project homepage
