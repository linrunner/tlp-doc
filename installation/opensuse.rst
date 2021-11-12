openSUSE
===============================
.. rubric:: Scope:

* Officially supported openSUSE Leap releases and Tumbleweed

.. note::

    Execute the commands below in a root shell.

Package Installation
--------------------
Packages are available from the official repositories:

* **tlp** – Power saving
* **tlp-rdw** – optional - :doc:`/settings/rdw`

Install them either with your favorite package manager or the command: ::

    zypper install tlp tlp-rdw

ThinkPads only
--------------
.. include:: ../include/thinkpad-kernel-modules.rst

Depending on your model and kernel version external kernel module(s) are required
to provide battery charge thresholds and recalibration.

For openSUSE `Tumbleweed` and newer ThinkPads (X220/T420 i.e. 2013 and later)
just install the required external kernel module `acpi_call` from the official
repositories with: ::

    zypper install acpi_call-kmp-default

For openSUSE `Leap` and/or older ThinkPads your mileage may vary as you have to
browse `openSUSE Software <https://software.opensuse.org/>`_ for community
packages or build the required module from source. The output of :command:`tlp-stat -b`
(version 1.2.2 or higher recommended) will guide you which external kernel module
is required:

* **acpi_call** – optional – External kernel module providing battery
  recalibration for newer ThinkPads (X220/T420 i.e. 2013 and later)
* **tp_smapi** – optional – External kernel module providing battery charge
  thresholds, recalibration and specific :command:`tlp-stat -b` output
  for older ThinkPads

.. seealso::

    * Refer to :ref:`faq-which-kernel-module` for details
