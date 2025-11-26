power-profiles-daemon
=====================
By default, many distributions install power-profiles-daemon as an
optional component of their GNOME, KDE or Cinnamon desktop environments.
This article compares TLP and power-profiles-daemon and explains how to replace
the latter with TLP.

Does power-profiles-daemon save power over TLP?
-----------------------------------------------
Simply put, power-profiles-daemon is a CPU throttle that is most
effective when high or medium load applications are in use.
Unlike TLP, it has no settings to reduce power consumption when the CPU
is idle, such as when there is no user input.

power-profiles-daemon only covers a subset of TLP's settings:

1. `PLATFORM_PROFILE_ON_AC/BAT/SAV`
2. `CPU_ENERGY_PERF_POLICY_ON_AC/BAT/SAV`
3. `CPU_BOOST_ON_AC/BAT/SAV`
4. `AMDGPU_ABM_LEVEL_ON_AC/BAT/SAV`

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

**Conclusion:** To determine which tool provides superior results, conduct
comparative measurements on your target hardware using your specific usage
pattern.

How can I use TLP to achieve the same effect as power-profiles-daemon?
----------------------------------------------------------------------
Starting with *version 1.9*, TLP offers a complete replacement for
power-profiles-daemon. **tlp-pd** implements the same D-Bus API
that major Linux desktop environments like GNOME, KDE and Cinnamon
already use for choosing from three profiles with a mouse click.

The first advantage of TLP is that the three profiles can be customized
individually, whereas with power-profiles-daemon they are "hard-wired".

The second advantage of TLP is automatic switching: when AC power
is connected, the *performance* profile is activated; when changing to
battery operation, the *balanced* profile is activated. In addition,
you can switch to the power-saver profile at any time with a click of the
mouse, which remains set even when the power source changes.

As with almost everything in TLP, however, automatic switching can be
disabled via configuration, allowing you to always start your laptop
with the *balanced* profile, for example – see :ref:`faq-disable-auto-switch`.
In this way, TLP behaves identically to power-profiles-daemon.

.. seealso::

    * Settings: :doc:`/settings/platform`
    * Settings: :doc:`/settings/processor`
    * Settings: :doc:`/settings/operation`
    * :doc:`/introduction`
    * :doc:`/support/optimizing`

.. _faq-ppd-conflict:

Does power-profiles-daemon conflict with TLP?
---------------------------------------------
Yes, it does. Using both tools at the same time can lead to unpredictable
results as they partly change the same :ref:`kernel tunables <intro-how-it-works>`
(see above) and overwrite each other's tuning.

To prevent conflicts, many Linux distributions do not allow TLP and
power-profiles-daemon packages to be installed at the same time.
This results in the uninstallation of power-profiles-daemon when
TLP is installed (and vice versa).
If your distribution's package manager does not enforce this, it is
advisable to uninstall power-profiles-daemon when using TLP.

If your distribution allows for parallel installation, the behaviour
depends on the version of TLP:

*Version 1.6 and later* will automatically *not* apply the settings listed
above if a running power-profiles-daemon is detected. In addition the
:command:`tlp start` issues a warning about the conflict:

.. code-block:: none

    Warning: PLATFORM_PROFILE_ON_AC/BAT is not set because power-profiles-daemon is running.

*Version 1.5* merely detects the situation and the commands
:command:`tlp start` and :command:`tlp-stat -s` issue a message about the conflict:

.. code-block:: none

    Error: conflicting power-profiles-daemon.service is enabled, power saving will not apply on boot.
    >>> Invoke 'systemctl mask power-profiles-daemon.service' to correct this!

If you do not want to use power-profiles-daemon in parallel, you should uninstall
the package (preferred). If that would uninstall essential packages,
stop and disable the service with ::

    sudo systemctl stop power-profiles-daemon.service
    sudo systemctl mask power-profiles-daemon.service


.. seealso::

    * FAQ: :doc:`/faq/conflicts`
    * `power-profiles-daemon <https://gitlab.freedesktop.org/upower/power-profiles-daemon>`_
      - Project homepage
    * `TLP Issue #564 <https://github.com/linrunner/TLP/issues/564>`_
