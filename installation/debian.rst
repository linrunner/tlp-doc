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

Debian 10.0 "Buster" and 9.0 "Stretch"
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Newer TLP packages are available via `Debian Backports`_.

Add the line matching your Debian version to your **/etc/apt/sources.list**: ::

    deb http://ftp.debian.org/debian buster-backports main
    deb http://ftp.debian.org/debian stretch-backports-sloppy main

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

or ::

    apt -t stretch-backports-sloppy install tlp tlp-rdw

instead.

ThinkPads only
^^^^^^^^^^^^^^
Depending on your model and kernel version external kernel module(s) are required
to provide battery charge thresholds and recalibration.

The output of :command:`tlp-stat -b` (version 1.2.2 or higher recommended) will guide
you which package to install:

* **acpi-call-dkms** *(main)* – optional – External kernel module providing
  battery charge thresholds and recalibration for newer ThinkPads (X220/T420 and later)
* **tp-smapi-dkms** *(main)* – optional – External kernel module providing battery
  charge thresholds, recalibration and specific :command:`tlp-stat -b` output
  for older ThinkPads

Install them either with your favorite package manager or the command ::

    apt install acpi-call-dkms tp-smapi-dkms

omitting the ones not required by your hardware.

.. note::

    * Refer to :ref:`faq-which-kernel-module` for details
    * You must disable Secure Boot to use the ThinkPad specific packages

.. _`Debian Backports`: https://backports.debian.org/Instructions/

