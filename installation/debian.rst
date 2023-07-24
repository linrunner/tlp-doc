Debian
======
.. rubric:: Scope:

* Debian oldstable, stable, testing and unstable
* Linux Mint Debian Edition (LMDE)

.. note::

    Execute the commands below in a root shell or with a preceding :command:`sudo`.

Package Repository
------------------
TLP and ThinkPad-related packages below are available via the official Debian
repositories.

Newer TLP packages may be provided via `Debian backports`_. Add the following line
to your **/etc/apt/sources.list**: ::

    deb http://ftp.debian.org/debian DIST-backports main

Replace `DIST` with the codename of your installed Debian version.

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

Replace `DIST` with the codename of your installed Debian version.

ThinkPads only: External Kernel Modules
---------------------------------------
.. include:: ../include/thinkpad-kernel-modules.rst

Depending on your model and kernel version external kernel module(s) are required
to provide battery charge thresholds and recalibration.

The output of :command:`tlp-stat -b` will guide you which package to install:

* **tp-smapi-dkms** *(main)* – optional – External kernel module providing
  battery charge thresholds and recalibration for ThinkPads before model year 2011
  as well as specific :command:`tlp-stat -b` output until model year 2011
* **acpi-call-dkms** *(main)* – optional **(kernel 5.16 and older only)** –
  External kernel module providing battery recalibration for ThinkPads
  since model year 2011 - e.g. T420/X220 and newer


Install the appropriate package either with your favorite package manager
or the command ::

    apt install tp-smapi-dkms


.. warning::

    **Bullseye**: the `acpi-call-dkms` package in the official repositories is
    incompatible with **backports kernels ≥ 5.13** and may cause TLP battery care
    malfunction, system freezes and reboots.

    Solution: install `acpi-call-dkms` version 1.2.2 from `Debian backports`_: ::

        apt -t bullseye-backports install acpi-call-dkms


.. note::

    * :ref:`faq-which-kernel-module` explains details
    * Refer to `Debian backports`_ for setup instructions
    * You must disable Secure Boot to use the ThinkPad specific packages


.. _`Debian backports`: https://backports.debian.org/Instructions/

