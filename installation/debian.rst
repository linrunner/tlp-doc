Debian
======
.. rubric:: Scope:

* Debian oldstable, stable, testing and unstable
* Linux Mint Debian Edition (LMDE)

.. note::

    Execute the commands below in a root shell.

Package Repository
------------------

Debian stable, testing and unstable
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TLP and ThinkPad-related packages below are available via the official Debian
repository.

Debian 10.0 "Buster"
^^^^^^^^^^^^^^^^^^^^
Newer TLP packages are available via `Debian backports`_.

Add the following line to your **/etc/apt/sources.list**: ::

    deb http://ftp.debian.org/debian buster-backports main

Update package data: ::

    apt update

Package Installation
--------------------
Install the following packages

* **tlp** *(main)* – Power saving
* **tlp-rdw** *(main)* – optional – :doc:`/settings/rdw`

either with your favorite package manager or the command: ::

    apt install tlp tlp-rdw

For `Debian Backports`_ use: ::

    apt -t buster-backports install tlp tlp-rdw

instead.

ThinkPads only
^^^^^^^^^^^^^^
.. include:: ../include/thinkpad-kernel-modules.rst

Depending on your model and kernel version external kernel module(s) are required
to provide battery charge thresholds and recalibration.

The output of :command:`tlp-stat -b` (version 1.2.2 or higher recommended) will guide
you which package to install:

* **acpi-call-dkms** *(main)* – optional – External kernel module providing
  battery recalibration for newer ThinkPads (X220/T420 and later)
* **tp-smapi-dkms** *(main)* – optional – External kernel module providing battery
  charge thresholds, recalibration and specific :command:`tlp-stat -b` output
  for older ThinkPads

Install the appropriate package either with your favorite package manager
or the command ::

    apt install acpi-call-dkms

Replace `acpi-call-dkms` with `tp-smapi-dkms` where suitable
(special case: X220/T420 generation makes use of both).

.. important::

    When using a kernel from Buster backports, you must install `acpi-call-dkms`
    from backports too: ::

        apt -t buster-backports install acpi-call-dkms

    Otherwise the :ref:`DKMS build will fail <faq-acpi-call-dkms-package>`.

.. note::

    * Refer to :ref:`faq-which-kernel-module` for details
    * You must disable Secure Boot to use the ThinkPad specific packages


.. _`Debian Backports`: https://backports.debian.org/Instructions/

