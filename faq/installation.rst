Installation
============
.. _faq-install-conflict:

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
