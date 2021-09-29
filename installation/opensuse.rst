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

The output of :command:`tlp-stat -b` (version 1.2.2 or higher recommended) will
guide you which package to install:

* **acpi_call** – optional – External kernel module providing battery
  recalibration for newer ThinkPads (X220/T420 and later)
  - `Download acpi_call <https://software.opensuse.org/package/acpi_call>`_
* **acpi_call-kmp-default** - optional - Tumbleweed only; should be installed together with the acpi_call package 
  - `Download acpi_call-kmp-default <https://software.opensuse.org/package/acpi_call-kmp-default>`_
* **tp_smapi_kmp** – optional – External kernel module providing battery charge
  thresholds, recalibration and specific :command:`tlp-stat -b` output
  for older ThinkPads
  - `Download tp-smapi <https://software.opensuse.org/package/tp_smapi-kmp>`_

.. seealso::

    * Refer to :ref:`faq-which-kernel-module` for details

