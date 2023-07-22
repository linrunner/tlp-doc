News
****

.. _news-top-1:

20.07.2023 - TLP 1.6 Beta 1 released / Call for Testers
=======================================================
The first beta of TLP 1.6 has finally arrived. Highlights are:

* *CPU_ENERGY_PERF_POLICY_ON_AC/BAT* now supports AMD Zen 2 or newer CPUs
  with *amd-pstate* driver
* Set CPU driver operation mode (*active*, *guided*, *passive*)
  with *amd-pstate* or *intel_pstate* driver
* Change system suspend mode (*deep*, *s2idle*)
* Battery Care support for System76 and Toshiba/Dynabook laptops
* Bugfix: deactivate *AHCI_RUNTIME_PM* and *PCIE_ASPM* before suspend to
  avoid resume freezes

Check the `release notes <https://github.com/linrunner/TLP/releases>`_
or see the `changelog <https://github.com/linrunner/TLP/blob/main/changelog>`_
for details on features and bugfixes.

.. rubric:: Call for Testers

As with every upcoming release I need help to complete the test coverage. So I'm
reaching out to the community of Linux users to test new and existing
functionality on their hardware.

Check the `beta page <https://download.linrunner.de/packages/>`_
for instructions and packages matching your distribution - then
`join the testers <https://github.com/linrunner/TLP/issues/700>`_.


.. _news-top-2:

17.02.2022 - How can I use Battery Care with my Laptop?
=======================================================
TLP includes battery charge thresholds for Lenovo (and IBM) ThinkPads from the
very beginning. That remained so for a long time. Recently - with version 1.4 and 1.5 -
numerous vendors have been added: ASUS, Huawei, LG, Lenovo (non-ThinkPad series),
Samsung and Sony.

The charge control options of the newly supported hardware vary greatly depending
on the vendor (or brand). The new article :doc:`/settings/bc-vendors` explains
configuration and prerequisites in detail.

Enjoy :-)


07.01.2022 - TLP 1.5 released
=============================
Happy New Year!

The final release of TLP 1.5 is out. New features are:

* Charge threshold support for Sony laptops
* Use the new kernel interface for recalibration of ThinkPad
  batteries (new sysfs node *charge_behaviour*), which will make the external
  kernel module *acpi_call* obsolete; *expected to appear with kernel 5.17*
* Support for switching NFC devices (new command :command:`nfc on|off`)

Check the `release notes <https://github.com/linrunner/TLP/releases>`_
and the complete `changelog <https://github.com/linrunner/TLP/blob/main/changelog>`_
for details.

**Release packages are provided via the repositories of your distribution.**

A big thanks to all contributors, testers and bug reporters!

Have fun :-)


20.12.2021 - TLP 1.5 Beta 1 released
====================================
In preparation for the next feature release today I publish the first beta of
TLP 1.5. The following features are included:

* Charge threshold support for Sony laptops
* Preparation for the upcoming kernel interface for recalibration of ThinkPad
  batteries, which will make the external kernel module *acpi_call* obsolete
* Support for switching NFC devices (new command :command:`nfc on|off`)

For a quick glance check the `release notes <https://github.com/linrunner/TLP/releases>`_
or see the `changelog <https://github.com/linrunner/TLP/blob/main/changelog>`_
for details on features and bugfixes.

.. rubric:: Call for Testers

As with every upcoming release I need help to complete the test coverage. So I'm
reaching out to the community of Linux users to test new and existing
functionality on their hardware.

Check the `beta page <https://download.linrunner.de/packages/>`_
for instructions and packages matching your distribution - then join
the testers.

24.09.2021 - TLP 1.4 released
=============================
After a successful beta test, I am proud to present the final release of
TLP 1.4 to you today. The release is packed with awesome new features, I would
like to list only the highlights here:

* Extended charge threshold support for laptops with a suitable kernel driver:
  ASUS, Huawei, LG, Lenovo (non-ThinkPad series), Samsung
* Select a platform profile to control system operating characteristics around
  power/performance levels, thermal and fan speed
* Enable Intel CPU HWP dynamic boost

Check the `release notes <https://github.com/linrunner/TLP/releases>`_
and the complete `changelog <https://github.com/linrunner/TLP/blob/main/changelog>`_
for all the details on features and bugfixes.

**Release packages are provided via the repositories of your distribution.**

A big thanks to all contributors, testers and bug reporters!

Have fun :-)


09.09.2021 - TLP 1.4 Beta 2 released / Call for Testers
=======================================================
Beta 2 accumulates all corrections from the preceding test.
Check the `release notes <https://github.com/linrunner/TLP/releases>`_
for details on the bugfixes.

.. rubric:: Call for Testers

A big thanks to all beta 1 testers and bug reporters!

You are now called upon to review beta 2.
New testers are welcome as well.
Check the `beta page <https://download.linrunner.de/packages/>`_
for instructions and packages matching your distribution.

Enjoy :-)


29.07.2021 - TLP 1.4 Beta 1 released / Call for Testers
=======================================================
Concluding an intensive development cycle I present to you the first beta of
TLP 1.4 - packed with awesome new features. Among the highlights are:

* Extended charge threshold support for laptops with a suitable kernel driver:
  ASUS, Huawei, LG, Lenovo (non-ThinkPad series), Samsung
* Select a platform profile to control system operating characteristics around
  power/performance levels, thermal and fan speed
* Enable Intel CPU HWP dynamic boost

For a quick glance check the `release notes <https://github.com/linrunner/TLP/releases>`_
or see the `changelog <https://github.com/linrunner/TLP/blob/main/changelog>`_
for details on features and bugfixes.

.. rubric:: Call for Testers

As with every upcoming release I need help to complete the test coverage. So I'm
reaching out to the community of Linux users to test new and existing
functionality on their hardware.

Check the `beta page <https://download.linrunner.de/packages/>`_
for instructions and packages matching your distribution - then join
the testers.

Have fun :-)
