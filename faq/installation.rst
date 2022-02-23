Installation
============

.. _faq-ppd-conflict:

Conflict with power-profiles-daemon
-----------------------------------

.. warning::

    `power-profiles-daemon <https://gitlab.freedesktop.org/hadess/power-profiles-daemon>`_
    (contained in GNOME 40 and newer) might be contained in the default install
    of your distribution.

    **Symptom:** `power-profiles-daemon.service` prevents TLP from making power
    saving settings at system startup. A check with ::

        systemctl status power-profiles-daemon.service tlp.service

    generates the following output in the conflict case: ::

        ● power-profiles-daemon.service - Power Profiles daemon
             Loaded: loaded (/lib/systemd/system/power-profiles-daemon.service; enabled; vendor preset: enabled)
             Active: active (running) since Wed 2021-09-08 09:55:50 CEST; 2min 13s ago
           Main PID: 413 (power-profiles-)
              Tasks: 3 (limit: 4506)
             Memory: 1.1M
             CGroup: /system.slice/power-profiles-daemon.service
                     └─413 /usr/libexec/power-profiles-daemon

        Sep 08 09:55:50 host systemd[1]: Starting Power Profiles daemon...
        Sep 08 09:55:50 host systemd[1]: Started Power Profiles daemon.

        ● tlp.service - TLP system startup/shutdown
             Loaded: loaded (/lib/systemd/system/tlp.service; enabled; vendor preset: enabled)
             Active: inactive (dead)
               Docs: https://linrunner.de/tlp

    Beginning with version 1.4 :command:`tlp start` and :command:`tlp-stat -s`
    will also detect the conflict: ::

        Error: conflicting power-profiles-daemon.service is enabled, power saving will not apply on boot.
        >>> Invoke 'systemctl mask power-profiles-daemon.service' to correct this!


    **Solutions:**

    a. Uninstall the `power-profiles-daemon` package (preferred)
    b. Disable `power-profiles-daemon` with ::


        sudo systemctl mask power-profiles-daemon.service


    and reboot.

    The following TLP version 1.5 distribution packages will take care of removing
    the conflicting *power-profiles-daemon* package when installed:

    * Arch Linux
    * Debian Bookworm/Sid
    * Ubuntu 22.04 (also the 21.10 package from the PPA)


.. seealso::

    * `TLP Issue #564 <https://github.com/linrunner/TLP/issues/564>`_
    * `Ubuntu Bug #1934944 <https://bugs.launchpad.net/ubuntu/+source/tlp/+bug/1934944>`_


Does TLP conflict with other power management tools?
----------------------------------------------------
Yes. Using another tool simultaneously means that TLP's settings get overwritten
by the other tools settings (and vice versa), so actual power saving gets
unpredictable. Special cases are explained in the following.

**Powertop:** please refer to :doc:`powertop`.

**thermald:** thermald's purpose is to limit power dissipation before the
laptop's temperature gets critical. TLP enables power saving features globally
to optimize battery power especially in idle and low workload situations.
TLP does not conflict with thermald.

**throttled:** only throttled's dynamic `HWP_Mode` setting interferes with TLP's
actions. If you want to use it, disable the feature in TLP by configuring
`CPU_ENERGY_PERF_POLICY_ON_AC=""`.


.. _faq-service-units:

Must I enable TLP's systemd service units?
------------------------------------------
Symptoms: :command:`tlp-stat -s` shows ::

    Error: tlp.service is not enabled, power saving will not apply on boot.
    >>> Invoke 'systemctl enable tlp.service' to correct this!

and/or ::

    Error: tlp-sleep.service is not enabled, power saving will not apply on boot.
    >>> Invoke 'systemctl enable tlp-sleep.service' to correct this!

Answer: *yes*, the service units are *indispensable* for correct operation:

* **tlp.service**: applies power saving settings and charge thresholds
  as well as switching radio devices on system boot and shutdown
* **tlp-sleep.service**: applies powers saving upon system suspend and resume
  *(not applicable for version 1.3 and higher)*

.. note::

    Debian, Fedora and Ubuntu enable the service by default as part of the
    package :doc:`/installation/index`, others such as Arch Linux don't.
    If unsure check the output of :command:`tlp-stat -s` for corresponding
    notes.


Does TLP run on my laptop (not a ThinkPad)?
-------------------------------------------
TLP runs on every laptop brand. A few features are available on IBM/Lenovo
ThinkPads only.

Does TLP make sense on newer laptops / with newer Linux versions?
-----------------------------------------------------------------
Yes, of course.

The Linux kernel has accumulated many power saving features over the years,
but not all are enabled by default. It seems to be really hard for the kernel
developers to fully debug power saving on all possible hardware, so power
saving stays disabled for many drivers and it's up to the user to enable it.

Conclusion: a userspace tool like TLP is still needed to enable power saving globally.

Should I install TLP inside a virtual machine?
----------------------------------------------
No. It is not effective to run a power management tool inside a virtual machine
guest. Install TLP in the host operating system instead.

Ubuntu/Debian: I do not use Network Manager, how do I install tlp without tlp-rdw?
----------------------------------------------------------------------------------
::

    sudo apt install --no-install-recommends tlp

Ubuntu: How do I prevent the installation of postfix as a dependency?
---------------------------------------------------------------------
The package `tlp` recommends `smartmontools` which pulls `postfix`
(via recommends too). Use: ::

    sudo apt install --no-install-recommends tlp tlp-rdw ethtool smartmontools


My Linux distribution does not provide a TLP package, how do I install it?
--------------------------------------------------------------------------
See :doc:`/installation/others`.

How do I install TLP on a development release of my distribution?
-----------------------------------------------------------------
TLP packages for new distribution versions appear in due time for the release.
If you want to use TLP with alpha or beta releases, download the packages for
the predecessor and install them manually with your favorite package manager.


What if I want a GUI?
---------------------
Get `TLPUI <https://github.com/d4nj1/TLPUI>`_.
