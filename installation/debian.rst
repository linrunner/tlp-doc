Debian
======
.. rubric:: Scope:

* Debian oldstable, stable, testing and unstable
* Linux Mint Debian Edition (LMDE)

.. note::

    Execute the commands below in a root shell.

Package Repository
------------------
TLP and ThinkPad-related packages below are available via the official Debian
repositories.

Newer TLP packages may be provided via `Debian backports`_. Add the following line
to your **/etc/apt/sources.list**: ::

    deb http://ftp.debian.org/debian DIST-backports main

Replace `DIST` with `buster` or `bullseye` according to your installation.

Update package data: ::

    apt update

Package Installation
--------------------
Install the following packages

* **tlp** *(main)* – Power saving
* **tlp-rdw** *(main)* – optional – :doc:`/settings/rdw`

either with your favorite package manager or the command: ::

    apt install tlp tlp-rdw

For `Debian backports`_ use: ::

    apt -t DIST-backports install tlp tlp-rdw

Replace `DIST` with `buster` or `bullseye` according to your installation.

ThinkPads only
--------------
.. include:: ../include/thinkpad-kernel-modules.rst

Depending on your model and kernel version external kernel module(s) are required
to provide battery charge thresholds and recalibration.

The output of :command:`tlp-stat -b` will guide you which package to install:

* **acpi-call-dkms** *(main)* – optional – External kernel module providing
  battery recalibration for ThinkPads since model year 2011 - e.g. T420/X220 and newer
* **tp-smapi-dkms** *(main)* – optional – External kernel module providing battery
  battery charge thresholds and recalibration for ThinkPads before model year 2011
  as well as specific :command:`tlp-stat -b` output until model year 2011

Install the appropriate package either with your favorite package manager
or the command ::

    apt install acpi-call-dkms

Replace `acpi-call-dkms` with `tp-smapi-dkms` where suitable.

.. warning::

    **Bullseye**: `acpi-call-dkms` packages in the official repositories are
    incompatible with **backports kernels ≥ 5.13** and may cause TLP battery care
    malfunction, system freezes and reboots.

    Solution: install `acpi-call-dkms` version 1.2.2 from `Debian backports`_: ::

        apt -t bullseye-backports install acpi-call-dkms


.. important::

    **Buster:** when using **kernel 5.10 from backports**, you must install
    `acpi-call-dkms` from `Debian backports`_ too: ::

        apt -t buster-backports install acpi-call-dkms

    Otherwise the :ref:`DKMS build will fail <faq-acpi-call-dkms-package>`.

.. note::

    * :ref:`faq-which-kernel-module` explains details
    * Refer to `Debian backports`_ for setup instructions
    * You must disable Secure Boot to use the ThinkPad specific packages


.. _`Debian backports`: https://backports.debian.org/Instructions/

