Ubuntu
======
.. rubric:: Scope:

* Officially supported Ubuntu releases
* Corresponding Linux Mint releases *but not* LMDE – refer to
  :doc:`debian` instead

Package Repository
------------------
Add the `TLP PPA`_ to your package sources with the commands: ::

   sudo add-apt-repository ppa:linrunner/tlp
   sudo apt update

.. note::

   TLP and ThinkPad-related packages below are available via the official Ubuntu
   repository. Nevertheless it is recommended to use the `TLP PPA`_ to stay with the
   latest TLP version.

Package Installation
--------------------
Install the packages

* **tlp** *(PPA or universe)* – Power saving
* **tlp-rdw** *(PPA or universe)* – optional – :doc:`/settings/rdw`

either with your favorite package manager or the command: ::

    sudo apt install tlp tlp-rdw

*Version 1.5 only*: due to a bug in the Ubuntu packages (official and PPA)
it is necessary to enable the service manually after installation: ::

    sudo systemctl enable tlp.service

The problem is resolved for version 1.6.


ThinkPads only: External Kernel Modules
---------------------------------------
.. include:: ../include/thinkpad-kernel-modules.rst

Depending on your model and kernel version external kernel module(s) are required
to provide battery charge thresholds and recalibration.

The output of :command:`tlp-stat -b` will guide you which package to install:

* **tp-smapi-dkms** *(universe)* – optional – External kernel module providing
  battery charge thresholds and recalibration for ThinkPads before model year 2011
  as well as specific :command:`tlp-stat -b` output until model year 2011
* **acpi-call-dkms** *(PPA or universe)* – optional **(kernel 5.16 and older only)** –
  External kernel module providing battery recalibration for ThinkPads
  since model year 2011 - e.g. T420/X220 and newer

Install the appropriate package either with your favorite package manager or
the command ::

    sudo apt install tp-smapi-dkms

.. warning::

    * In *Ubuntu 23.10* the `tp-smapi-dkms` package from the official
      repositories is broken and does not build and install the kernel module.
      Solution: download a working package from
      `Debian Sid <https://packages.debian.org/sid/all/tp-smapi-dkms/download>`_
      and install manually.

    * In *Ubuntu 20.04.4* the `acpi-call-dkms` package from the official
      repositories is incompatible with the provided kernels 5.13 or 5.15
      and may cause TLP battery care malfunction, system freezes and reboots.
      Solution: use `acpi-call-dkms` version 1.2.2 from the `TLP PPA`_ or `download
      from Ubuntu 22.04 <https://packages.ubuntu.com/jammy/all/acpi-call-dkms/download>`_
      and install manually.

.. seealso::

    * `TLP PPA`_ - Contains latest TLP packages for Ubuntu
    * :ref:`faq-which-kernel-module`
    * `Debian Bug #1034233 <https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=1034233>`_ -
      Cause of the not enabled service with the 1.5 packages
    * `Issue #615 <https://github.com/linrunner/TLP/issues/615>`_ - System freezes and reboots on Ubuntu
    * For systems with Secure Boot enabled, please refer to
      `DKMS Secure Boot <https://wiki.ubuntu.com/UEFI/SecureBoot/DKMS>`_
    * :ref:`faq-ppd-conflict` - FAQ

.. _`TLP PPA`: https://launchpad.net/~linrunner/+archive/ubuntu/tlp
