Installation
============

.. _faq-ppd-conflict:

Conflict with power-profiles-daemon
-----------------------------------
Most distributions install power-profiles-daemon by default with their
GNOME and KDE desktop environments. power-profiles-daemon uses kernel
settings that are also controlled by TLP, which leads to unpredictable
results.

Beginning with version 1.4 :command:`tlp start` and :command:`tlp-stat -s`
will detect the conflict: ::

    Warning: TLP's power saving will not apply on boot because the conflicting power-profiles-daemon.service is enabled.
    >>> Uninstall power-profiles-daemon or invoke 'systemctl mask power-profiles-daemon.service' to ensure the full functionality of TLP. to correct this!

**Solutions:**

a. Uninstall the `power-profiles-daemon` package (preferred)
b. Disable `power-profiles-daemon` with ::

    sudo systemctl mask power-profiles-daemon.service

.. seealso::

    * FAQ: :doc:`/faq/ppd`

Does TLP conflict with other power management tools?
----------------------------------------------------
Yes. Using another tool simultaneously means that TLP's settings get overwritten
by the other tools settings (and vice versa), so actual power saving gets
unpredictable. Special cases are explained in the following.

**power-profiles-daemon:** see above and refer to :doc:`/faq/ppd`.

**Powertop:** please refer to :doc:`powertop`.

**Slimbook Battery:** uses TLP as backend to apply power saving and
for this purpose continuously overwrites your TLP configuration.
If you want to configure TLP individually, you need to uninstall Slimbook
Battery first.

**system76-power:** works on the same set of :ref:`kernel settings
<intro-how-it-works>`. Do not use together with TLP.

**thermald:** thermald's purpose is to limit power dissipation before the
laptop's temperature gets critical. TLP enables power saving to optimize
battery power especially in idle and low workload situations.
TLP does not conflict with thermald.

**throttled:** only throttled's dynamic `HWP_Mode` setting interferes with TLP's
actions. If you want to use it, disable the feature in TLP by configuring
`CPU_ENERGY_PERF_POLICY_ON_AC=""`.


.. _faq-service-units:

Must I enable TLP's systemd service unit?
------------------------------------------
Symptoms: :command:`tlp-stat -s` shows ::

    Error: tlp.service is not enabled, power saving will not apply on boot.
    >>> Invoke 'systemctl enable tlp.service' to correct this!

Answer: *yes*, the service unit is *indispensable* for correct operation -
**tlp.service** applies power saving settings and charge thresholds
as well as switching radio devices on system boot and shutdown.

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
