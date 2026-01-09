News
****
`#TLPlinux <https://fosstodon.org/tags/TLPlinux>`_ - Follow us at
`@linrunner@fosstodon.org <https://fosstodon.org/@linrunner>`_

.. _news-top-1:

07.01.2026 - TLP 1.9.1 released
===============================
Version 1.9.1 primarily resolves a security issue. See the
`changelog <https://github.com/linrunner/TLP/blob/main/changelog#L12>`__
for details.

Happy new year! :-)

.. _news-top-2:

01.12.2025 - TLP 1.9 released
=============================
The beta test has been successfully completed, TLP 1.9.0 was just released.

The brand new **tlp-pd** enables easy profile switching with a mouse click.
tlp-pd replaces power-profiles-daemon by implementing the same D-Bus API
that major Linux desktop environments like GNOME, KDE and Cinnamon already use.

The `release notes <https://github.com/linrunner/TLP/releases/tag/1.9.0-beta.1>`__
provide a quick overview of the new features. All details, including bug fixes,
can be found in the `changelog <https://github.com/linrunner/TLP/blob/main/changelog>`__.

**Release packages** will soon be made available via the repositories of your
distribution, see :doc:`/installation/index`.

Thanks to everyone who contributed, tested and reported bugs!

Have fun! :-)

05.11.2025 - TLP 1.9 Beta 1 released / Call for Testers
=======================================================
The first beta version of TLP 1.9 is here.

Key innovation is the new *TLP profiles daemon*:
**tlp-pd** enables you to switch between performance (AC),
balanced (BAT) and power-saving (SAV) settings profiles
at the click of the mouse. It replaces power-profiles-daemon.

The `release notes <https://github.com/linrunner/TLP/releases/tag/1.9.0-beta.1>`__
provide a quick overview of the new features. All details, including bug fixes,
can be found in the `changelog <https://github.com/linrunner/TLP/blob/main/changelog>`_.

**Please take part in the** `beta test <https://github.com/linrunner/TLP/issues/832>`_ **!**
**I look forward to seeing your results.**


13.02.2025 - TLP 1.8 released
=============================
After the successful beta test, TLP 1.8 is finished.

Battery care is at the forefront of this new release, with charge thresholds
and recalibrate/discharge for Chromebooks and Framework laptops,
and charge thresholds for Dell laptops.
See the `release notes <https://github.com/linrunner/TLP/releases>`__
and the `full changelog <https://github.com/linrunner/TLP/blob/main/changelog>`_
for details.

**Release packages** will soon be made available via the repositories of your
distribution, see :doc:`/installation/index`.

Thanks to everyone who contributed, tested and reported bugs!

Enjoy! :-)

19.01.2025 - TLP 1.8 Beta 1 released / Call for Testers
=======================================================
I am pleased to announce the first beta version of TLP 1.8.

Battery care is at the forefront of this new release, with charge thresholds
and recalibrate/discharge for Chromebooks and Framework laptops,
and charge thresholds for Dell laptops.
See the `release notes <https://github.com/linrunner/TLP/releases>`__
and the `full changelog <https://github.com/linrunner/TLP/blob/main/changelog>`_
for details.

I'm looking for you great people to help me test new and existing features
on your hardware, as I do with every beta release.
Just visit the `beta page <https://download.linrunner.de/packages/>`_
for instructions and packages matching your distribution - then
`enter testing <https://github.com/linrunner/TLP/issues/781>`_!

27.09.2024 - TLP 1.7 released
=============================
The beta test was a success, and I'm proud to announce the final release of
TLP 1.7.

Enjoy charge threshold support for Apple Silicon Macbooks and MSI laptops,
as well as laptop screen power saving with AMD's Adaptive Backlight Modulation.
Check out the `release notes <https://github.com/linrunner/TLP/releases>`__
and the complete `changelog <https://github.com/linrunner/TLP/blob/main/changelog>`_
so you can see all the exciting new features and fixes.

**Release packages are provided via your distribution's repositories.**

A huge thank you to all of our wonderful contributors, testers, and bug
reporters!

Have fun! :-)


04.09.2024 - TLP 1.7 Beta 1 released / Call for Testers
=======================================================
I am proud to present the first beta of TLP 1.7. It took longer than expected,
but it was worth the wait.

This new version brings significant improvements, including charge threshold
support for Apple Silicon Macbooks and MSI laptops, as well as laptop screen
power saving with AMD's Adaptive Backlight Modulation.
For all the latest info on new features and fixes, check out the
`release notes <https://github.com/linrunner/TLP/releases>`__
and the `full changelog <https://github.com/linrunner/TLP/blob/main/changelog>`_.

As with every beta release, I'm looking for some awesome folks to help
me complete the test coverage. I'm reaching out to the community of Linux
users to test new and existing functionality on their hardware.
Just visit the `beta page <https://download.linrunner.de/packages/>`_
for instructions and packages matching your distribution - then
`join the test force <https://github.com/linrunner/TLP/issues/760>`_!


03.09.2023 - Optimizing Guide
=============================
TLP’s default settings should save as much energy on battery power as possible,
but in reality there are limits to what can be done based on the default settings.
The brand new :doc:`/support/optimizing` explores possibilities to tune
TLP individually and achieve even better battery runtime or performance
on AC respectively.

Enjoy and save!


24.08.2023 - TLP 1.6 released
=============================
After a successful beta test, I am proud to announce the final release of
TLP 1.6 with many exciting features. Highlights are:

* *CPU_DRIVER_OPMODE_ON_AC/BAT*: set CPU driver operation mode
  (*active*, *guided*, *passive*) with *amd-pstate* or *intel_pstate* driver
* *CPU_ENERGY_PERF_POLICY_ON_AC/BAT*: now supports AMD Zen 2 or newer CPUs
  with *amd-pstate* driver
* *MEM_SLEEP_ON_AC/BAT*: change system suspend mode (*deep*, *s2idle*)
* Battery Care support for System76 and Toshiba/Dynabook laptops
* Bugfix: deactivate *AHCI_RUNTIME_PM* and *PCIE_ASPM* before suspend to
  avoid resume freezes

Check the `release notes <https://github.com/linrunner/TLP/releases>`__
and the complete `changelog <https://github.com/linrunner/TLP/blob/main/changelog>`_
for details.

**Release packages are provided via the repositories of your distribution.**

A big thanks to all contributors, testers and bug reporters!

Have fun :-)

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

Check the `release notes <https://github.com/linrunner/TLP/releases>`__
or see the `changelog <https://github.com/linrunner/TLP/blob/main/changelog>`_
for details on features and bugfixes.

.. rubric:: Call for Testers

As with every upcoming release I need help to complete the test coverage. So I'm
reaching out to the community of Linux users to test new and existing
functionality on their hardware.

Check the `beta page <https://download.linrunner.de/packages/>`_
for instructions and packages matching your distribution - then
`join the testers <https://github.com/linrunner/TLP/issues/700>`_.


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

Check the `release notes <https://github.com/linrunner/TLP/releases>`__
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

For a quick glance check the `release notes <https://github.com/linrunner/TLP/releases>`__
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

Check the `release notes <https://github.com/linrunner/TLP/releases>`__
and the complete `changelog <https://github.com/linrunner/TLP/blob/main/changelog>`_
for all the details on features and bugfixes.

**Release packages are provided via the repositories of your distribution.**

A big thanks to all contributors, testers and bug reporters!

Have fun :-)


09.09.2021 - TLP 1.4 Beta 2 released / Call for Testers
=======================================================
Beta 2 accumulates all corrections from the preceding test.
Check the `release notes <https://github.com/linrunner/TLP/releases>`__
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

For a quick glance check the `release notes <https://github.com/linrunner/TLP/releases>`__
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
